# 🩺 Health Guardian - Complete Application Overview

## 📊 Application Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                   Health Guardian App                        │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │          Landing Page (index.html)                   │   │
│  │  Auto-redirects to registration or main app         │   │
│  └──────────────────────────────────────────────────────┘   │
│                           ↓                                  │
│  ┌──────────────────────────────────────────────────────┐   │
│  │   Registration (registration.html + app.js)         │   │
│  │   Step 1: Name, Age                                 │   │
│  │   Step 2: Language, Gender                          │   │
│  │   Step 3: Voice Preference, Volume                  │   │
│  └──────────────────────────────────────────────────────┘   │
│                           ↓                                  │
│  ┌──────────────────────────────────────────────────────┐   │
│  │          Main Application (app.html)                │   │
│  │                                                      │   │
│  │  ┌────────────────────────────────────────────────┐ │   │
│  │  │         Content Area (Screen Manager)          │ │   │
│  │  │                                                │ │   │
│  │  │  ┌─────────────────────────────────────────┐  │ │   │
│  │  │  │ 🏠 HOME SCREEN (Today's Medicines)      │  │ │   │
│  │  │  │  • Shows scheduled medicines for today  │  │ │   │
│  │  │  │  • Quick status indicators              │  │ │   │
│  │  │  │  • FAB button to add medicine           │  │ │   │
│  │  │  └─────────────────────────────────────────┘  │ │   │
│  │  │                                                │ │   │
│  │  │  ┌─────────────────────────────────────────┐  │ │   │
│  │  │  │ 💊 MEDICINES SCREEN (Full List)        │  │ │   │
│  │  │  │  • All added medicines                  │  │ │   │
│  │  │  │  • Edit & Delete functionality          │  │ │   │
│  │  │  │  • Complete details view                │  │ │   │
│  │  │  └─────────────────────────────────────────┘  │ │   │
│  │  │                                                │ │   │
│  │  │  ┌─────────────────────────────────────────┐  │ │   │
│  │  │  │ ➕ ADD MEDICINE (Detailed Form)          │  │ │   │
│  │  │  │  • Photo upload                         │  │ │   │
│  │  │  │  • Name, dosage, instructions           │  │ │   │
│  │  │  │  • Time & date selection                │  │ │   │
│  │  │  │  • Frequency options (4 types)          │  │ │   │
│  │  │  │  • Weekly day selector                  │  │ │   │
│  │  │  │  • Custom cycle configuration           │  │ │   │
│  │  │  └─────────────────────────────────────────┘  │ │   │
│  │  │                                                │ │   │
│  │  │  ┌─────────────────────────────────────────┐  │ │   │
│  │  │  │ 📊 PROGRESS SCREEN (Calendar View)      │  │ │   │
│  │  │  │  • Select medicine from dropdown        │  │ │   │
│  │  │  │  • Calendar with color coding:          │  │ │   │
│  │  │  │    🟢 Taken, 🔴 Missed, 🟡 Skipped    │  │ │   │
│  │  │  │  • Monthly navigation                   │  │ │   │
│  │  │  │  • Progress legend                      │  │ │   │
│  │  │  └─────────────────────────────────────────┘  │ │   │
│  │  │                                                │ │   │
│  │  │  ┌─────────────────────────────────────────┐  │ │   │
│  │  │  │ ⚙️ SETTINGS SCREEN (Customization)      │  │ │   │
│  │  │  │  • User profile info                    │  │ │   │
│  │  │  │  • Language selection (11 languages)    │  │ │   │
│  │  │  │  • Voice preference (M/F/Neutral)      │  │ │   │
│  │  │  │  • Volume level adjustment              │  │ │   │
│  │  │  │  • Accessibility options                │  │ │   │
│  │  │  │  • Data export/import                   │  │ │   │
│  │  │  │  • Clear all data option                │  │ │   │
│  │  │  └─────────────────────────────────────────┘  │ │   │
│  │  │                                                │ │   │
│  │  └────────────────────────────────────────────────┘ │   │
│  │                                                      │   │
│  │  ┌────────────────────────────────────────────────┐ │   │
│  │  │    🔔 REMINDER MODAL (Full-Screen Alert)      │ │   │
│  │  │    • Medicine image                           │ │   │
│  │  │    • Name & dosage                            │ │   │
│  │  │    • Intake instructions                      │ │   │
│  │  │    • Voice reminder playing                   │ │   │
│  │  │    • Three action buttons:                    │ │   │
│  │  │      [✓ Done] [⏰ Later] [✕ Skip]             │ │   │
│  │  └────────────────────────────────────────────────┘ │   │
│  │                                                      │   │
│  │  ┌────────────────────────────────────────────────┐ │   │
│  │  │    Bottom Navigation Bar                      │ │   │
│  │  │  [🏠 Home] [💊 Meds] [📊 Progress] [⚙️ Set] │ │   │
│  │  └────────────────────────────────────────────────┘ │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## 📱 Screen Flows

### Screen Transitions
```
index.html
    ↓
User Registered? 
    ├─ YES → app.html (Main App)
    └─ NO  → registration.html (3-step form)
                    ↓
                [Completes]
                    ↓
              → app.html (Main App)

Main App Navigation:
    Home ↔ Medicines ↔ Progress ↔ Settings
     ↓
  Add Medicine (Overlay)
     ↓
  Edit/Delete (Form Population)
```

## 🗄️ Data Storage Structure

```
localStorage["healthGuardianUser"]
{
    fullName: "John Smith",
    age: 65,
    gender: "male",
    language: "english",
    voiceGender: "male",
    volumeLevel: "high",
    registrationDate: "2026-01-16T10:30:00Z"
}

localStorage["healthGuardianMedicines"]
[
    {
        id: "1705408200000",
        name: "Aspirin",
        dosageAmount: 500,
        dosageUnit: "mg",
        intakeInstruction: "after-meal",
        intakeTime: "09:00",
        startDate: "2026-01-16",
        endDate: "",
        frequency: "daily",
        photoData: "[base64 image]",
        notes: "Take with water",
        createdAt: "2026-01-16T10:30:00Z"
    },
    {
        id: "1705408300000",
        name: "Vitamin D",
        dosageAmount: 1,
        dosageUnit: "tablet",
        intakeInstruction: "with-meal",
        intakeTime: "10:00",
        startDate: "2026-01-16",
        endDate: "",
        frequency: "weekly",
        selectedDays: [1, 3, 5],  // Mon, Wed, Fri
        photoData: "[base64 image]",
        notes: "",
        createdAt: "2026-01-16T10:30:00Z"
    }
]

localStorage["healthGuardianProgress"]
{
    "1705408200000": {
        "2026-01-16": "taken",
        "2026-01-15": "missed",
        "2026-01-14": "taken"
    },
    "1705408300000": {
        "2026-01-15": "taken",
        "2026-01-12": "skipped"
    }
}

localStorage["healthGuardianSettings"]
{
    language: "english",
    voiceGender: "male",
    volumeLevel: "high",
    largeText: false,
    highContrast: false,
    vibration: true
}
```

## 🔄 Feature Flow Diagrams

### Medicine Addition Flow
```
User Clicks "+" Button
        ↓
Add Medicine Form Loads
        ↓
Select Frequency:
    ├─ Daily: Skip day selection
    ├─ Weekly: Show day selector
    ├─ Monthly: Skip day selection
    └─ Custom: Show cycle input fields
        ↓
Fill Form (Name, Dosage, Time, etc.)
        ↓
Click "Add Medicine"
        ↓
Validate Form
    ├─ Valid: Save to localStorage
    └─ Invalid: Show error message
        ↓
Show Success Notification
        ↓
Redirect to Medicines List
```

### Reminder Flow
```
System checks every minute:
    Time matches scheduled time? + Medicine due today?
        ↓
    YES: Check progress status
        ├─ Taken: Skip
        ├─ Skipped: Skip
        └─ Pending: Show Reminder
            ↓
        Full-Screen Modal Opens
            ↓
        Voice Reminder Plays (Repeats every 3 sec)
            ↓
        Device Vibrates
            ↓
    User chooses:
        ├─ ✓ Done: Mark as taken, close
        ├─ ⏰ Later: Snooze 15 min, close
        └─ ✕ Skip: Mark as skipped, close
```

### Progress Tracking Flow
```
User navigates to Progress Screen
        ↓
Select Medicine from Dropdown
        ↓
Fetch current month dates
        ↓
For each date:
    Is medicine scheduled?
    ├─ YES: Check status
    │   ├─ taken: 🟢 Green
    │   ├─ missed: 🔴 Red
    │   ├─ skipped: 🟡 Yellow
    │   └─ pending: ⚪ Gray
    └─ NO: ⚪ Not Due
        ↓
Render Calendar Grid
        ↓
User can navigate months
```

## 🎯 Feature Details

### 1. Frequency Engines

**Daily**
```
Every day at specified time
No conditions - always due
```

**Weekly**
```
Due on: Monday(1), Wednesday(3), Friday(5)
Check day of week every day
If matches selected days → due
```

**Monthly**
```
Due on: Same date each month (e.g., 15th)
Check current date against start date
If day matches → due
```

**Custom Cycle**
```
Intake: 1 day, Pause: 2 days
Pattern: Take-Skip-Skip-Take-Skip-Skip...

Days since start = current date - start date
Cycle position = (days since start) % (intake + pause)
If cycle position < intake days → due
```

### 2. Voice Reminder System

```
Trigger:
    Scheduled time reached
    Medicine due today
    Status not yet recorded
        ↓
Get Settings:
    Language
    Voice Gender
    Volume Level
        ↓
Generate Text:
    "Hi [Name], it's time to take [medicine].
     Please take [dosage] [unit] of [name].
     [Instruction]."
        ↓
Web Speech API:
    Create utterance
    Set language code
    Select voice by gender
    Set volume
    Set auto-repeat
        ↓
Play Voice
    Repeat every 3 seconds
    Until user acknowledges
```

## 📐 CSS Architecture

```
main.css:
├─ CSS Variables (Colors, Fonts, Spacing)
├─ Reset & Base Styles
├─ Layout (Grid, Flex, Fixed)
├─ Navigation (Bottom Nav, Buttons)
├─ Screens (Hidden/Active states)
├─ Forms (Inputs, Buttons, Validation)
├─ Cards (Medicine Cards, List Items)
├─ Modal (Reminder Popup)
├─ Calendar (Progress Tracking)
├─ Animations (Fade, Slide, Bounce)
├─ Responsive Breakpoints
│   ├─ Desktop (1200px+)
│   ├─ Tablet (768px)
│   └─ Mobile (480px, 400px)
└─ Accessibility (Large Text, High Contrast)
```

## 🔐 Data Flow Security

```
All data stored locally in browser
        ↓
No external APIs called (except voice synthesis)
        ↓
No user data sent to servers
        ↓
User has full control
        ↓
Can export data anytime
        ↓
Can clear data anytime
        ↓
Data persists across sessions
        ↓
Each browser = separate database
```

## ⚡ Performance Optimizations

```
Lightweight:
    ✓ No external libraries (vanilla JS)
    ✓ No backend required
    ✓ CSS animations (hardware accelerated)
    ✓ Minimal DOM manipulation

Fast Loading:
    ✓ Single page app (no page reloads)
    ✓ Screen switching (instant)
    ✓ Data access (direct from localStorage)

Memory Efficient:
    ✓ Event listeners cleaned up
    ✓ Modals hidden, not removed
    ✓ Data indexed by ID
    ✓ Lazy rendering

Responsive:
    ✓ Mobile-first design
    ✓ Touch-friendly buttons
    ✓ Adaptive layouts
    ✓ Fast interactions
```

## 🌍 Supported Languages

```
English (en-US)
    ├─ Voice synthesis available
    └─ Full UI support

Spanish (es-ES)
    ├─ Voice synthesis available
    └─ Full UI support

French (fr-FR)
    ├─ Voice synthesis available
    └─ Full UI support

German (de-DE)
    ├─ Voice synthesis available
    └─ Full UI support

Hindi (hi-IN)
    ├─ Voice synthesis available
    └─ Full UI support

Gujarati (gu-IN)
    ├─ Voice synthesis available
    └─ Full UI support

Marathi (mr-IN)
    ├─ Voice synthesis available
    └─ Full UI support

Tamil (ta-IN)
    ├─ Voice synthesis available
    └─ Full UI support

Telugu (te-IN)
    ├─ Voice synthesis available
    └─ Full UI support

Chinese Simplified (zh-CN)
    ├─ Voice synthesis available
    └─ Full UI support

Japanese (ja-JP)
    ├─ Voice synthesis available
    └─ Full UI support
```

## 🎨 Color Scheme

```
Primary: #667eea (Blue-Purple)
Primary Dark: #764ba2 (Dark Purple)
Success: #27ae60 (Green - taken)
Danger: #e74c3c (Red - missed)
Warning: #f39c12 (Orange - skipped)
Secondary: #95a5a6 (Gray - buttons)
Light: #ecf0f1 (Light gray - backgrounds)
Lighter: #f8f9fa (Lighter gray - main bg)
Dark: #2c3e50 (Dark - text)
Text: #333 (Dark gray - text)
Text Light: #666 (Medium gray - secondary text)
Border: #e0e0e0 (Light border)
```

## 📊 Statistics & Tracking

```
For each medicine, track:
    ├─ Total doses scheduled
    ├─ Total doses taken
    ├─ Total doses missed
    ├─ Total doses skipped
    ├─ Adherence percentage
    └─ Trends over time

Progress indicators:
    ├─ Daily: Today's status
    ├─ Weekly: Last 7 days
    ├─ Monthly: Current month
    └─ Yearly: Full year view (future)
```

---

**Health Guardian v1.0** - Smart Medication Management System  
Built with modern web technologies for accessibility and reliability 🩺✅
