# Health Guardian - Implementation Guide

## Architecture Overview

This is a **browser-based HealthTech application** with NO backend server. All data is stored in localStorage and managed through modular JavaScript files.

### File Structure

```
medisreminder/
├── registration.html          → User onboarding & setup
├── app.html                   → Main app with multi-screen UI
├── home.html                  → Dashboard with quick access
├── app.js                     → Registration form logic
├── app-main.js                → Main app screen logic & navigation
├── js/
│   ├── data-manager.js        → localStorage operations
│   ├── reminder-system.js     → Notifications & voice reminders
│   └── reminder-modal.html    → Modal HTML for reminders
├── styles/
│   └── main.css               → Styling for all screens
└── assets/                    → Images & media
```

## Data Flow

### 1. **User Registration** (registration.html → app.js)
- User fills 3-step registration form
- Data saved to localStorage as `healthGuardianUser`
```javascript
{
  fullName: "John Doe",
  age: 35,
  gender: "male",
  language: "english",
  voiceGender: "male",
  volumeLevel: "normal",
  registrationDate: "2025-01-17T10:30:00Z",
  medications: []
}
```
- Redirects to home.html or app.html

### 2. **Medicine Management** (data-manager.js)
- Medicines stored in localStorage as `healthGuardianMedicines`
```javascript
{
  id: "1234567890",
  name: "Aspirin",
  dosageAmount: 1,
  dosageUnit: "tablet",
  intakeInstruction: "after-meal",
  intakeTime: "14:30",
  startDate: "2025-01-17",
  endDate: "2025-06-17",
  frequency: "daily" | "weekly" | "monthly" | "custom",
  selectedDays: [1, 3, 5], // for weekly
  intakeDays: 3, // for custom cycle
  pauseDays: 2,  // for custom cycle
  photoData: "base64-encoded-image",
  notes: "Take with water"
}
```

### 3. **Progress Tracking** (data-manager.js)
- Daily status stored in localStorage as `healthGuardianProgress`
```javascript
{
  "medicine-id-1": {
    "2025-01-17": "taken",
    "2025-01-16": "missed",
    "2025-01-15": "skipped"
  },
  "medicine-id-2": {
    "2025-01-17": "taken"
  }
}
```

### 4. **Reminder System** (reminder-system.js)
- Runs setInterval every 10 seconds to check for due medicines
- When time matches `intakeTime`:
  - Shows modal with medicine info
  - Plays voice reminder using Web Speech API
  - Uses user's preferred language, voice gender, and volume
  - Vibrates device (if enabled)
- User marks as: Taken / Skipped / Remind Later
- Status saved to progress tracking

### 5. **Settings Management** (data-manager.js)
- Stored in localStorage as `healthGuardianSettings`
```javascript
{
  language: "english",
  voiceGender: "female",
  voiceLanguage: "",  // empty = use app language
  volumeLevel: "normal",
  largeText: false,
  highContrast: false,
  vibration: true
}
```

## How Each Feature Works

### ✅ Home Dashboard (home.html)
- Displays user greeting with stored name
- Shows today's medicines using `dataManager.getMedicinesToday()`
- Quick access buttons to add medicine, view all, track progress, settings
- Navigation cards for different sections

### ✅ Medicine Management (app.html)
- **Add Medicine**: Form with all fields
  - Photo upload (converts to Base64)
  - Name, dosage amount & unit
  - Intake instruction (before/after/with meal, empty stomach, anytime)
  - Time (HH:MM format)
  - Start & end dates
  - Frequency: Daily / Weekly (select days) / Monthly / Custom cycle
  - Notes
- **Edit Medicine**: Pre-fills form and updates record
- **Delete Medicine**: Removes from storage and progress data
- **List All**: Shows all active medicines with quick status view

### ✅ Reminder System (reminder-system.js)
- **Automatic Reminders**: 
  - Background check every 10 seconds
  - Compares current time with `intakeTime`
  - Only triggers if status not already marked
- **Voice Reminders** (Web Speech API):
  - Uses user's language preference
  - Respects voice gender preference
  - Respects volume level
  - Repeats after 3 seconds
- **Notification API**: Uses browser notifications if permitted
- **Vibration**: Uses Vibration API (200ms, 100ms, 200ms pattern)
- **Modal Interactions**: User can mark Taken/Skipped/Remind Later

### ✅ Daily Progress Tracking (app-main.js)
- **Mark as Taken**: Sets status to "taken" for that day
- **Mark as Missed**: Sets status to "missed" 
- **Skip**: Sets status to "skipped"
- **Adherence Summary**: Shows % of medicines taken per day/week/month
- **Calendar View**: Visual representation of compliance

### ✅ Settings Page (app-main.js)
- **Language Selection**: Changes UI language (10+ supported)
- **Voice Gender**: Male/Female for voice reminders
- **Voice Language**: Override for voice (separate from UI language)
- **Volume Level**: Low / Normal / High
- **Accessibility**:
  - Larger text size
  - High contrast mode
  - Vibration alerts toggle
- **Profile Management**: Edit name, age, gender
- **Data Export**: Downloads backup as JSON
- **Clear All Data**: Resets everything and returns to registration

## Screen Navigation Flow

```
registration.html (onboarding)
    ↓ (user completes setup)
home.html (dashboard with quick access)
    ↓ (click "Add Medication" or "View All")
app.html (main app with tabs)
    ├─ Home Screen: Today's medicines
    ├─ Medicines Screen: All medicines
    ├─ Add/Edit Medicine: Form
    ├─ Progress Screen: Adherence tracking
    └─ Settings Screen: Preferences
```

## Key Technologies Used

| Feature | Technology |
|---------|-----------|
| Reminders | `setInterval` + datetime comparison |
| Voice | Web Speech API (`SpeechSynthesisUtterance`) |
| Notifications | Notification API (browser native) |
| Vibration | Vibration API (`navigator.vibrate`) |
| Storage | localStorage (5-10MB limit) |
| Image | Base64 encoding from file input |
| Language | Manual translation object + `translate()` function |

## Connecting to a Real Backend Later

### Migration Path (localStorage → Backend API)

**Step 1: Add API Layer** (Create `api-client.js`)
```javascript
class APIClient {
  async getUserData() {
    // Instead of: localStorage.getItem('healthGuardianUser')
    // Do: return await fetch('/api/user', { headers: auth })
  }
  
  async addMedicine(medicine) {
    // Instead of: dataManager.addMedicine(medicine)
    // Do: return await fetch('/api/medicines', { method: 'POST', body: medicine })
  }
  
  async updateProgress(medicineId, date, status) {
    // Instead of: dataManager.setMedicineProgress(...)
    // Do: return await fetch(`/api/progress/${medicineId}`, { method: 'PUT' })
  }
}
```

**Step 2: Create Backend API** (Node.js/Express)
```
GET  /api/user              → Get user profile
PUT  /api/user              → Update user
GET  /api/medicines         → List user's medicines
POST /api/medicines         → Add medicine
PUT  /api/medicines/:id     → Update medicine
DELETE /api/medicines/:id   → Delete medicine
GET  /api/progress          → Get adherence data
POST /api/progress          → Record medicine taken/missed/skipped
GET  /api/settings          → Get user settings
PUT  /api/settings          → Update settings
```

**Step 3: Add Authentication**
```javascript
// Add JWT token to API calls
async function authenticatedFetch(url, options = {}) {
  const token = localStorage.getItem('authToken');
  return fetch(url, {
    ...options,
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json',
      ...options.headers
    }
  });
}
```

**Step 4: Sync Strategy**
- Cache data locally (offline-first)
- Sync with backend when online
- Handle conflicts (last-write-wins or merge)
- Show sync status to user

### Database Schema (Example - MongoDB)

```javascript
// Users Collection
{
  _id: ObjectId,
  fullName: String,
  age: Number,
  gender: String,
  email: String,
  passwordHash: String,
  language: String,
  voiceGender: String,
  voiceLanguage: String,
  volumeLevel: String,
  createdAt: Date,
  updatedAt: Date
}

// Medicines Collection
{
  _id: ObjectId,
  userId: ObjectId,
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
  notes: String,
  photoURL: String,
  createdAt: Date,
  updatedAt: Date
}

// Progress Collection
{
  _id: ObjectId,
  userId: ObjectId,
  medicineId: ObjectId,
  date: Date,
  status: String, // "taken", "missed", "skipped"
  recordedAt: Date
}

// Settings Collection
{
  _id: ObjectId,
  userId: ObjectId,
  settingKey: String,
  settingValue: any,
  updatedAt: Date
}
```

## Testing the Application

1. **Register User**: Go to registration.html
2. **Add Medicine**: Click "+ Add Medicine" button
3. **Test Reminder**: Set time to current time ± 1 minute
4. **Check Voice**: Go to Settings → Test Voice Reminder
5. **Track Progress**: Mark medicines taken/skipped in home screen
6. **Export Data**: Settings → Export Data (downloads JSON)
7. **Test Languages**: Change language in settings (UI updates immediately)

## Offline Capability

✅ **Fully works offline**:
- All data stored locally in localStorage
- Reminders work without internet
- Voice synthesis works offline
- Notifications work offline
- Only need internet for: Backend sync, Data export/import, Cross-device sync

## Performance Considerations

- **Reminder check**: 10-second interval (configurable)
- **Storage limit**: ~5-10MB per origin (localStorage)
- **Memory**: Loaded data fit in ~1-2MB
- **Battery**: Voice synthesis uses device CPU/GPU
- **Notifications**: Battery efficient (native browser)

## Security Considerations

⚠️ **Current Implementation (Frontend Only)**:
- No authentication (anyone with browser access can see data)
- Data not encrypted
- No server-side validation

✅ **When Adding Backend**:
- Add user authentication (JWT/OAuth)
- Encrypt sensitive data in transit (HTTPS)
- Validate all inputs on server
- Use CORS for API security
- Add rate limiting
- Log all data access

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Reminders not triggering | Check browser console for errors, ensure time format is HH:MM |
| Voice not working | Check browser voice permissions, test in Settings |
| Data not saving | Check localStorage limits (5-10MB), clear old data |
| Language not changing | Refresh page, clear cache, check translation keys |
| Notifications not showing | Grant notification permission, check browser settings |

---

**This architecture allows for seamless migration from frontend-only to full-stack application without major refactoring!**
