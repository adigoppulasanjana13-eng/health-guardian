# Health Guardian - Data Flow & Backend Integration Guide

## 🔄 Complete Data Flow for Backend Integration

This document explains how to connect this frontend application to a real backend without rewriting the existing code.

---

## 📊 Current Frontend Architecture

```
┌─────────────────────────────────┐
│     HTML/CSS/DOM (UI)          │
│   (app.html, registration.html)│
└────────────┬────────────────────┘
             │ (Calls functions)
┌────────────▼────────────────────┐
│    App Logic (app-main.js)      │
│  - Screen navigation            │
│  - Form handling                │
│  - UI rendering                 │
└────────────┬────────────────────┘
             │ (Uses methods)
┌────────────▼────────────────────┐
│  Business Logic (Classes)       │
│  - DataManager                  │
│  - ReminderSystem               │
└────────────┬────────────────────┘
             │ (Reads/Writes)
┌────────────▼────────────────────┐
│    localStorage API             │
│  (Browser native storage)       │
└─────────────────────────────────┘
```

---

## 🔌 Migration Strategy: API Adapter Pattern

Instead of rewriting, we'll create an **API layer** that acts as an adapter between the existing code and backend APIs.

### Step 1: Create API Adapter (New File: `js/api-client.js`)

```javascript
// js/api-client.js
class APIClient {
    constructor(baseURL = '/api') {
        this.baseURL = baseURL;
        this.token = localStorage.getItem('authToken');
    }

    // Helper method for all API calls
    async request(method, endpoint, data = null) {
        const options = {
            method,
            headers: {
                'Content-Type': 'application/json',
                'Authorization': `Bearer ${this.token}`
            }
        };

        if (data) {
            options.body = JSON.stringify(data);
        }

        try {
            const response = await fetch(`${this.baseURL}${endpoint}`, options);
            
            if (!response.ok) {
                if (response.status === 401) {
                    // Token expired, redirect to login
                    window.location.href = 'registration.html';
                }
                throw new Error(`API Error: ${response.status}`);
            }

            return await response.json();
        } catch (error) {
            console.error('API request failed:', error);
            throw error;
        }
    }

    // User endpoints
    async registerUser(userData) {
        return this.request('POST', '/auth/register', userData);
    }

    async loginUser(email, password) {
        const response = await this.request('POST', '/auth/login', {email, password});
        this.token = response.token;
        localStorage.setItem('authToken', response.token);
        return response;
    }

    async logoutUser() {
        localStorage.removeItem('authToken');
        this.token = null;
    }

    async getUserProfile() {
        return this.request('GET', '/user');
    }

    async updateUserProfile(userData) {
        return this.request('PUT', '/user', userData);
    }

    // Medicine endpoints
    async getMedicines() {
        return this.request('GET', '/medicines');
    }

    async addMedicine(medicine) {
        return this.request('POST', '/medicines', medicine);
    }

    async updateMedicine(id, medicine) {
        return this.request('PUT', `/medicines/${id}`, medicine);
    }

    async deleteMedicine(id) {
        return this.request('DELETE', `/medicines/${id}`);
    }

    // Progress endpoints
    async getMedicineProgress(medicineId) {
        return this.request('GET', `/progress/${medicineId}`);
    }

    async recordProgress(medicineId, date, status) {
        return this.request('POST', '/progress', {
            medicineId,
            date,
            status
        });
    }

    // Settings endpoints
    async getSettings() {
        return this.request('GET', '/settings');
    }

    async updateSetting(key, value) {
        return this.request('PUT', '/settings', {key, value});
    }

    // Export endpoints
    async exportData() {
        return this.request('GET', '/data/export');
    }
}

// Initialize global API client
const apiClient = new APIClient();
```

### Step 2: Create Hybrid DataManager (Updated `js/data-manager.js`)

```javascript
// js/data-manager.js (UPDATED)
class DataManager {
    constructor(useAPI = false) {
        this.useAPI = useAPI; // Toggle: false = localStorage, true = API
        this.storageKey = 'healthGuardianData';
        // ... rest of constructor
    }

    // Updated: Use API if available, fallback to localStorage
    async getMedicines() {
        try {
            if (this.useAPI && window.apiClient) {
                return await apiClient.getMedicines();
            }
        } catch (error) {
            console.warn('API call failed, falling back to localStorage');
        }
        return this.getData(this.medicinesKey) || [];
    }

    async addMedicine(medicine) {
        try {
            if (this.useAPI && window.apiClient) {
                return await apiClient.addMedicine(medicine);
            }
        } catch (error) {
            console.warn('API call failed, using localStorage');
        }
        
        // Fallback to localStorage
        const medicines = this.getData(this.medicinesKey) || [];
        medicine.id = Date.now().toString();
        medicine.createdAt = new Date().toISOString();
        medicines.push(medicine);
        this.setData(this.medicinesKey, medicines);
        return medicine;
    }

    async updateMedicine(id, updates) {
        try {
            if (this.useAPI && window.apiClient) {
                return await apiClient.updateMedicine(id, updates);
            }
        } catch (error) {
            console.warn('API call failed, using localStorage');
        }

        // Fallback
        const medicines = this.getData(this.medicinesKey) || [];
        const index = medicines.findIndex(m => m.id === id);
        if (index !== -1) {
            medicines[index] = {...medicines[index], ...updates};
            this.setData(this.medicinesKey, medicines);
            return medicines[index];
        }
        return null;
    }

    async setMedicineProgress(medicineId, date, status) {
        try {
            if (this.useAPI && window.apiClient) {
                return await apiClient.recordProgress(medicineId, date, status);
            }
        } catch (error) {
            console.warn('API call failed, using localStorage');
        }

        // Fallback
        const progress = this.getData(this.progressKey) || {};
        if (!progress[medicineId]) {
            progress[medicineId] = {};
        }
        progress[medicineId][date] = status;
        this.setData(this.progressKey, progress);
    }

    // Add similar patterns for other methods
}

// Initialize with API flag
const dataManager = new DataManager(false); // Start with localStorage
// Later: new DataManager(true) to use API
```

---

## 🔌 Backend API Specification

### Authentication Endpoints

```
POST /api/auth/register
Content-Type: application/json

Request:
{
    "fullName": "John Doe",
    "email": "john@example.com",
    "password": "securePassword123",
    "age": 35,
    "gender": "male",
    "language": "english",
    "voiceGender": "male",
    "volumeLevel": "normal"
}

Response (200):
{
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "user": {
        "id": "user_123",
        "fullName": "John Doe",
        "email": "john@example.com",
        "age": 35,
        ...
    }
}
```

```
POST /api/auth/login
Content-Type: application/json

Request:
{
    "email": "john@example.com",
    "password": "securePassword123"
}

Response (200):
{
    "token": "eyJhbGciOiJIUzI1NiIs...",
    "user": {...}
}
```

```
POST /api/auth/logout
Headers: Authorization: Bearer {token}

Response (200):
{
    "success": true
}
```

---

### User Endpoints

```
GET /api/user
Headers: Authorization: Bearer {token}

Response (200):
{
    "id": "user_123",
    "fullName": "John Doe",
    "email": "john@example.com",
    "age": 35,
    "gender": "male",
    "language": "english",
    "voiceGender": "male",
    "volumeLevel": "normal",
    "largeText": false,
    "highContrast": false,
    "vibration": true,
    "createdAt": "2025-01-17T10:30:00Z",
    "updatedAt": "2025-01-17T10:30:00Z"
}
```

```
PUT /api/user
Headers: Authorization: Bearer {token}
Content-Type: application/json

Request:
{
    "fullName": "John Updated",
    "age": 36,
    "language": "spanish"
}

Response (200):
{
    "id": "user_123",
    "fullName": "John Updated",
    ...
}
```

---

### Medicine Endpoints

```
GET /api/medicines
Headers: Authorization: Bearer {token}

Response (200):
[
    {
        "id": "med_123",
        "userId": "user_123",
        "name": "Aspirin",
        "dosageAmount": 1,
        "dosageUnit": "tablet",
        "intakeInstruction": "after-meal",
        "intakeTime": "14:30",
        "startDate": "2025-01-17",
        "endDate": "2025-06-17",
        "frequency": "daily",
        "selectedDays": null,
        "intakeDays": null,
        "pauseDays": null,
        "photoURL": "https://bucket.s3.amazonaws.com/med_123.jpg",
        "notes": "Take with water",
        "createdAt": "2025-01-17T10:35:00Z",
        "updatedAt": "2025-01-17T10:35:00Z"
    }
]
```

```
POST /api/medicines
Headers: Authorization: Bearer {token}
Content-Type: application/json

Request:
{
    "name": "Aspirin",
    "dosageAmount": 1,
    "dosageUnit": "tablet",
    "intakeInstruction": "after-meal",
    "intakeTime": "14:30",
    "startDate": "2025-01-17",
    "endDate": "2025-06-17",
    "frequency": "daily",
    "photoData": "data:image/jpeg;base64,..." (or file upload)
}

Response (201):
{
    "id": "med_123",
    "name": "Aspirin",
    ...
}
```

```
PUT /api/medicines/{id}
Headers: Authorization: Bearer {token}
Content-Type: application/json

Request:
{
    "dosageAmount": 2,
    "intakeTime": "15:00"
}

Response (200):
{
    "id": "med_123",
    "dosageAmount": 2,
    "intakeTime": "15:00",
    ...
}
```

```
DELETE /api/medicines/{id}
Headers: Authorization: Bearer {token}

Response (204):
(empty)
```

---

### Progress Endpoints

```
GET /api/progress/{medicineId}
Headers: Authorization: Bearer {token}

Response (200):
{
    "medicineId": "med_123",
    "data": {
        "2025-01-17": "taken",
        "2025-01-16": "missed",
        "2025-01-15": "taken"
    }
}
```

```
POST /api/progress
Headers: Authorization: Bearer {token}
Content-Type: application/json

Request:
{
    "medicineId": "med_123",
    "date": "2025-01-17",
    "status": "taken"  // taken, missed, skipped
}

Response (201):
{
    "medicineId": "med_123",
    "date": "2025-01-17",
    "status": "taken",
    "recordedAt": "2025-01-17T14:30:00Z"
}
```

---

### Settings Endpoints

```
GET /api/settings
Headers: Authorization: Bearer {token}

Response (200):
{
    "language": "english",
    "voiceGender": "male",
    "voiceLanguage": "",
    "volumeLevel": "normal",
    "largeText": false,
    "highContrast": false,
    "vibration": true
}
```

```
PUT /api/settings
Headers: Authorization: Bearer {token}
Content-Type: application/json

Request:
{
    "language": "spanish",
    "voiceGender": "female"
}

Response (200):
{
    "language": "spanish",
    "voiceGender": "female",
    ...
}
```

---

### Data Export Endpoint

```
GET /api/data/export
Headers: Authorization: Bearer {token}

Response (200):
{
    "user": {...},
    "medicines": [...],
    "progress": {...},
    "settings": {...},
    "exportDate": "2025-01-17T14:30:00Z"
}
```

---

## 🗄️ Database Schema Examples

### MongoDB Collections

```javascript
// Users Collection
db.users.insertOne({
    _id: ObjectId(),
    email: "john@example.com",
    passwordHash: "$2b$10$...", // bcrypt hash
    fullName: "John Doe",
    age: 35,
    gender: "male",
    language: "english",
    voiceGender: "male",
    voiceLanguage: "",
    volumeLevel: "normal",
    largeText: false,
    highContrast: false,
    vibration: true,
    createdAt: new Date(),
    updatedAt: new Date()
})

db.medicines.insertOne({
    _id: ObjectId(),
    userId: ObjectId("user_id"),
    name: "Aspirin",
    dosageAmount: 1,
    dosageUnit: "tablet",
    intakeInstruction: "after-meal",
    intakeTime: "14:30",
    startDate: new Date("2025-01-17"),
    endDate: new Date("2025-06-17"),
    frequency: "daily",
    selectedDays: [],
    intakeDays: null,
    pauseDays: null,
    photoURL: "https://bucket.s3.amazonaws.com/...",
    notes: "Take with water",
    createdAt: new Date(),
    updatedAt: new Date()
})

db.progress.insertOne({
    _id: ObjectId(),
    userId: ObjectId("user_id"),
    medicineId: ObjectId("medicine_id"),
    date: new Date("2025-01-17"),
    status: "taken", // taken, missed, skipped
    recordedAt: new Date(),
    recordedBy: "user" // user, system, api
})
```

---

## 🔄 Sync Strategy: Offline-First with Sync

```javascript
// js/sync-manager.js (NEW FILE)
class SyncManager {
    constructor() {
        this.queue = [];
        this.isOnline = navigator.onLine;
        this.setupListeners();
    }

    setupListeners() {
        window.addEventListener('online', () => {
            this.isOnline = true;
            this.syncQueue();
        });
        window.addEventListener('offline', () => {
            this.isOnline = false;
        });
    }

    async queueOperation(operation) {
        this.queue.push({
            id: Date.now(),
            operation,
            timestamp: new Date(),
            synced: false
        });

        // Save queue to localStorage
        localStorage.setItem('syncQueue', JSON.stringify(this.queue));

        // Try to sync if online
        if (this.isOnline) {
            await this.syncQueue();
        }
    }

    async syncQueue() {
        const unsynced = this.queue.filter(q => !q.synced);
        
        for (const item of unsynced) {
            try {
                await this.executeSyncOperation(item.operation);
                item.synced = true;
            } catch (error) {
                console.error('Sync failed:', error);
                // Will retry later
            }
        }

        // Save updated queue
        localStorage.setItem('syncQueue', JSON.stringify(this.queue));
    }

    async executeSyncOperation(operation) {
        switch(operation.type) {
            case 'addMedicine':
                return await apiClient.addMedicine(operation.data);
            case 'updateMedicine':
                return await apiClient.updateMedicine(operation.id, operation.data);
            case 'recordProgress':
                return await apiClient.recordProgress(...operation.data);
            case 'updateSettings':
                return await apiClient.updateSetting(operation.key, operation.value);
        }
    }
}

const syncManager = new SyncManager();
```

---

## 🔐 Security Implementation

### 1. Authentication (Add to Backend)

```javascript
// Node.js/Express example
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

app.post('/api/auth/register', async (req, res) => {
    const {email, password, fullName, age} = req.body;
    
    // Validate input
    if (!email || !password || !fullName) {
        return res.status(400).json({error: 'Missing required fields'});
    }

    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);

    // Create user in database
    const user = await User.create({
        email,
        passwordHash: hashedPassword,
        fullName,
        age
    });

    // Generate JWT token
    const token = jwt.sign(
        {userId: user._id, email: user.email},
        process.env.JWT_SECRET,
        {expiresIn: '30d'}
    );

    res.json({token, user});
});

// Middleware: Verify token
function authenticateToken(req, res, next) {
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1];

    if (!token) {
        return res.status(401).json({error: 'No token'});
    }

    jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
        if (err) return res.status(403).json({error: 'Invalid token'});
        req.user = user;
        next();
    });
}

// Protected route
app.get('/api/user', authenticateToken, async (req, res) => {
    const user = await User.findById(req.user.userId);
    res.json(user);
});
```

### 2. Input Validation (Backend)

```javascript
// Validate all inputs
function validateMedicine(medicine) {
    const errors = [];
    
    if (!medicine.name || medicine.name.length < 2) {
        errors.push('Invalid medicine name');
    }
    
    if (!medicine.dosageAmount || medicine.dosageAmount <= 0) {
        errors.push('Invalid dosage amount');
    }
    
    if (!medicine.intakeTime || !/^\d{2}:\d{2}$/.test(medicine.intakeTime)) {
        errors.push('Invalid time format');
    }
    
    if (errors.length > 0) {
        throw new ValidationError(errors);
    }
    
    return true;
}
```

### 3. Authorization (Backend)

```javascript
// Ensure user can only access their own data
app.get('/api/medicines', authenticateToken, async (req, res) => {
    const medicines = await Medicine.find({userId: req.user.userId});
    res.json(medicines);
});

// Ensure user can only update their own medicine
app.put('/api/medicines/:id', authenticateToken, async (req, res) => {
    const medicine = await Medicine.findById(req.params.id);
    
    if (medicine.userId.toString() !== req.user.userId) {
        return res.status(403).json({error: 'Not authorized'});
    }
    
    Object.assign(medicine, req.body);
    await medicine.save();
    res.json(medicine);
});
```

---

## 🔄 Migration Checklist

### Phase 1: Prepare Backend
- [ ] Set up Node.js/Express server
- [ ] Set up MongoDB database
- [ ] Create database schema
- [ ] Implement authentication
- [ ] Create API endpoints
- [ ] Add input validation
- [ ] Add error handling
- [ ] Set up CORS
- [ ] Enable HTTPS

### Phase 2: Create API Adapter
- [ ] Create `js/api-client.js`
- [ ] Implement all API methods
- [ ] Add error handling
- [ ] Add token refresh logic
- [ ] Test API calls

### Phase 3: Update DataManager
- [ ] Update `js/data-manager.js` to use API
- [ ] Add fallback to localStorage
- [ ] Test API + localStorage hybrid mode
- [ ] Implement retry logic

### Phase 4: Test & Deploy
- [ ] Test with backend
- [ ] Test offline mode
- [ ] Test sync when back online
- [ ] Load testing
- [ ] Security testing
- [ ] Deploy backend
- [ ] Deploy frontend

---

## 📱 No Code Changes Needed!

The beautiful part of this architecture is **the UI code (`app.html`, `app-main.js`, `app.js`) doesn't need to change at all!**

Only the data layer changes:
- Frontend UI: ✅ Same
- Business logic: ✅ Same (with minimal updates)
- Data source: ✅ Changed (from localStorage to API)

This is the **Adapter Pattern** in action!

---

## 🚀 Deployment with Backend

### Option 1: Same Domain
```
Backend: https://example.com/api/
Frontend: https://example.com/

In api-client.js:
const apiClient = new APIClient('/api');
```

### Option 2: Different Domain (CORS)
```
Backend: https://api.example.com/
Frontend: https://app.example.com/

In api-client.js:
const apiClient = new APIClient('https://api.example.com');

In backend (Express):
app.use(cors({
    origin: 'https://app.example.com',
    credentials: true
}));
```

### Option 3: Docker Compose
```docker-compose
version: '3'
services:
  frontend:
    build: ./frontend
    ports:
      - "80:80"
  
  backend:
    build: ./backend
    ports:
      - "3000:3000"
    environment:
      MONGODB_URL: mongodb://mongo:27017/healthguardian
  
  mongo:
    image: mongo:latest
    ports:
      - "27017:27017"
```

---

## 📚 Backend Stack Recommendations

| Layer | Options |
|-------|---------|
| **Runtime** | Node.js, Python, Java, Go |
| **Framework** | Express, Flask, Spring, Gin |
| **Database** | MongoDB, PostgreSQL, MySQL |
| **Auth** | JWT, OAuth 2.0, Firebase Auth |
| **Storage** | AWS S3, Google Cloud Storage, Azure Blob |
| **Hosting** | Heroku, AWS, Google Cloud, DigitalOcean |

---

## 🎓 Example: Complete Backend in Express.js

```javascript
// server.js
const express = require('express');
const mongoose = require('mongoose');
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');
const cors = require('cors');
const dotenv = require('dotenv');

dotenv.config();
const app = express();

// Middleware
app.use(cors());
app.use(express.json());

// MongoDB Connection
mongoose.connect(process.env.MONGODB_URL, {
    useNewUrlParser: true,
    useUnifiedTopology: true
});

// Schema
const UserSchema = new mongoose.Schema({
    email: {type: String, unique: true, required: true},
    passwordHash: {type: String, required: true},
    fullName: String,
    age: Number,
    gender: String,
    language: String,
    voiceGender: String,
    voiceLanguage: String,
    volumeLevel: String,
    largeText: Boolean,
    highContrast: Boolean,
    vibration: Boolean,
    createdAt: {type: Date, default: Date.now}
});

const MedicineSchema = new mongoose.Schema({
    userId: {type: mongoose.Schema.Types.ObjectId, required: true},
    name: String,
    dosageAmount: Number,
    dosageUnit: String,
    intakeInstruction: String,
    intakeTime: String,
    startDate: Date,
    endDate: Date,
    frequency: String,
    selectedDays: [Number],
    intakeDays: Number,
    pauseDays: Number,
    photoURL: String,
    notes: String,
    createdAt: {type: Date, default: Date.now}
});

const ProgressSchema = new mongoose.Schema({
    userId: {type: mongoose.Schema.Types.ObjectId, required: true},
    medicineId: {type: mongoose.Schema.Types.ObjectId, required: true},
    date: Date,
    status: String,
    recordedAt: {type: Date, default: Date.now}
});

const User = mongoose.model('User', UserSchema);
const Medicine = mongoose.model('Medicine', MedicineSchema);
const Progress = mongoose.model('Progress', ProgressSchema);

// Auth Routes
app.post('/api/auth/register', async (req, res) => {
    try {
        const {email, password, fullName, age, gender, language, voiceGender, volumeLevel} = req.body;
        
        const hashedPassword = await bcrypt.hash(password, 10);
        
        const user = new User({
            email,
            passwordHash: hashedPassword,
            fullName,
            age,
            gender,
            language,
            voiceGender,
            volumeLevel
        });
        
        await user.save();
        
        const token = jwt.sign(
            {userId: user._id},
            process.env.JWT_SECRET,
            {expiresIn: '30d'}
        );
        
        res.status(201).json({token, user});
    } catch (error) {
        res.status(500).json({error: error.message});
    }
});

app.post('/api/auth/login', async (req, res) => {
    try {
        const {email, password} = req.body;
        
        const user = await User.findOne({email});
        if (!user) return res.status(400).json({error: 'User not found'});
        
        const validPassword = await bcrypt.compare(password, user.passwordHash);
        if (!validPassword) return res.status(400).json({error: 'Invalid password'});
        
        const token = jwt.sign(
            {userId: user._id},
            process.env.JWT_SECRET,
            {expiresIn: '30d'}
        );
        
        res.json({token, user});
    } catch (error) {
        res.status(500).json({error: error.message});
    }
});

// Middleware to verify JWT
function authenticateToken(req, res, next) {
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1];
    
    if (!token) return res.status(401).json({error: 'No token'});
    
    jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
        if (err) return res.status(403).json({error: 'Invalid token'});
        req.user = user;
        next();
    });
}

// Medicine Routes
app.get('/api/medicines', authenticateToken, async (req, res) => {
    try {
        const medicines = await Medicine.find({userId: req.user.userId});
        res.json(medicines);
    } catch (error) {
        res.status(500).json({error: error.message});
    }
});

app.post('/api/medicines', authenticateToken, async (req, res) => {
    try {
        const medicine = new Medicine({
            userId: req.user.userId,
            ...req.body
        });
        await medicine.save();
        res.status(201).json(medicine);
    } catch (error) {
        res.status(500).json({error: error.message});
    }
});

app.put('/api/medicines/:id', authenticateToken, async (req, res) => {
    try {
        const medicine = await Medicine.findById(req.params.id);
        if (medicine.userId.toString() !== req.user.userId) {
            return res.status(403).json({error: 'Not authorized'});
        }
        Object.assign(medicine, req.body);
        await medicine.save();
        res.json(medicine);
    } catch (error) {
        res.status(500).json({error: error.message});
    }
});

app.delete('/api/medicines/:id', authenticateToken, async (req, res) => {
    try {
        await Medicine.findByIdAndDelete(req.params.id);
        res.status(204).send();
    } catch (error) {
        res.status(500).json({error: error.message});
    }
});

// Progress Routes
app.post('/api/progress', authenticateToken, async (req, res) => {
    try {
        const progress = new Progress({
            userId: req.user.userId,
            ...req.body
        });
        await progress.save();
        res.status(201).json(progress);
    } catch (error) {
        res.status(500).json({error: error.message});
    }
});

// Start Server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
```

---

## 📊 Summary: Zero Frontend Changes!

```
Current State (localStorage):
├─ registration.html ✅ (No changes)
├─ home.html ✅ (No changes)
├─ app.html ✅ (No changes)
├─ app.js ✅ (No changes)
├─ app-main.js ✅ (Minimal changes - language strings only)
├─ data-manager.js ✅ (Add API calls, keep fallback)
├─ reminder-system.js ✅ (No changes)
└─ styles/main.css ✅ (No changes)

NEW Files to Add:
├─ js/api-client.js (New adapter layer)
└─ js/sync-manager.js (Optional, for offline sync)

Backend to Create:
├─ Express.js server
├─ MongoDB database
├─ API endpoints
├─ Authentication
└─ Data validation
```

---

**This architecture ensures that the frontend application can seamlessly transition from localStorage to a full backend system without rewriting the UI code!**
