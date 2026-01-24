# Quick Start Guide - Health Guardian

## 🚀 Getting Started in 5 Minutes

### Step 1: Open the App
```
1. Open your browser
2. Navigate to: file:///E:/nodejs/medisreminder/registration.html
   OR serve it with: python -m http.server 8000
   Then: http://localhost:8000/registration.html
```

### Step 2: Register (2 minutes)
```
STEP 1 - Personal Info:
  Name: John Doe
  Age: 35
  Click: Continue

STEP 2 - Language & Gender:
  Language: English
  Gender: Male
  Click: Continue

STEP 3 - Voice Preferences:
  Voice: Male
  Volume: Normal
  Click: Complete Setup

→ Redirects to home.html
```

### Step 3: Add Your First Medicine (1 minute)
```
Home Dashboard → Click "+ Add Medicine"

Fill the form:
  Name: Aspirin
  Dosage: 1 tablet
  Instruction: After Meal
  Time: 14:30 (2:30 PM)
  Start Date: Today
  End Date: (leave empty for ongoing)
  Frequency: Daily

Click: Add Medicine

→ Medicine added successfully!
```

### Step 4: Test Reminders (1 minute)
```
Option A - Automatic:
  1. Wait until 14:30
  2. Reminder modal will pop up automatically
  
Option B - Manual Test (for debugging):
  1. Go to app.html
  2. Look for "🧪 Test Reminder (Debug)" button
  3. Click to manually trigger reminder
  
When modal appears:
  - Click "✓ Done" to mark medicine taken
  - Click "⏰ Remind Later" to reschedule in 15 min
  - Click "✕ Skip" to skip this dose
```

### Step 5: View Your Data
```
HOME SCREEN:
  - Shows today's medicines
  - Shows greeting with your name
  - Quick access to all features

MEDICINES SCREEN:
  - See all your medicines
  - Edit or delete any medicine
  - Add more medicines

PROGRESS SCREEN:
  - Select medicine from dropdown
  - View calendar with color-coded days:
    🟢 Green = Taken
    🔴 Red = Missed  
    🟡 Yellow = Skipped
    ⚪ Gray = Not due today
  - See adherence percentage

SETTINGS:
  - Change language, voice, volume
  - Edit your profile
  - Enable accessibility features
  - Export data backup
  - Clear all data
```

---

## 📱 Common Actions

### How to Add a Medicine
```
Home → + Add Medicine → Fill Form → Add Medicine
```

### How to Mark Medicine as Taken
```
When reminder shows:
  Click "✓ Done"
  
OR manually:
  Home → Click medicine → Mark as Taken
```

### How to Change Language
```
Settings → Select New Language
(UI updates immediately)
```

### How to Test Voice
```
Settings → Test Voice Reminder Button
(Hear a sample reminder in your language)
```

### How to Edit a Medicine
```
Medicines → Click medicine → Edit button → Update
```

### How to Delete a Medicine
```
Medicines → Click medicine → Delete button → Confirm
```

### How to View Progress
```
Progress → Select medicine → View calendar
```

### How to Export Data
```
Settings → Data Management → Export Data
(Downloads JSON file as backup)
```

### How to Backup Everything
```
Settings → Export Data
(Save the JSON file to your computer)
```

### How to Start Over
```
Settings → Clear All Data
(Confirm twice, then redirects to registration)
```

---

## ⚙️ Feature Details

### 1. REMINDER SYSTEM
- ✅ Automatically checks every 10 seconds
- ✅ Triggers at exact medicine time
- ✅ Shows medicine photo, name, dosage, instruction
- ✅ Plays voice reminder in your language
- ✅ Vibrates device (if enabled)
- ✅ Three action options

### 2. VOICE REMINDERS
Supported Languages:
- English
- Spanish
- French
- German
- Hindi
- Gujarati
- Marathi
- Tamil
- Telugu
- Chinese (Simplified)
- Japanese

Voice Gender: Male / Female / Neutral

Volume Level: Low / Normal / High

### 3. MEDICINE FREQUENCIES
- **Daily**: Every day
- **Weekly**: Select specific days (Mon, Tue, Wed, etc.)
- **Monthly**: Same date each month
- **Custom**: Take X days, pause Y days (e.g., Take 3 days, pause 2 days)

### 4. ACCESSIBILITY OPTIONS
- Large text size
- High contrast mode
- Vibration alerts toggle

### 5. DATA MANAGEMENT
- All data stored locally (no internet needed)
- Export as JSON backup
- Clear all data option
- Profile editing

---

## 🔧 Troubleshooting

### Reminder didn't trigger
1. Check current time matches medicine time (HH:MM format)
2. Ensure medicine start date is today or earlier
3. Check medicine frequency includes today
4. Open browser console (F12) for error messages
5. Try manual test button in debug section

### Voice not playing
1. Check browser supports Web Speech API (Chrome, Edge, Firefox)
2. Check notification permission (may need to grant)
3. Try test voice button in Settings
4. Check volume level in Settings (not mute)
5. Try different browser if issues persist

### Data not saving
1. Enable localStorage (usually default)
2. Check browser isn't in private/incognito mode
3. Clear browser cache and try again
4. Try exporting data to verify storage works
5. If storage full, delete old medicines

### App looks wrong
1. Try different browser
2. Clear browser cache (Ctrl+Shift+Delete)
3. Zoom to default (Ctrl+0)
4. Try incognito/private mode
5. Refresh page (Ctrl+R)

### Medicine time format wrong
- Use 24-hour format: HH:MM
- Examples: 08:00 (8 AM), 14:30 (2:30 PM), 21:00 (9 PM)

### Reminder keeps repeating
- That's normal! Click "✓ Done" to stop
- Or click "⏰ Remind Later" to snooze 15 minutes

---

## 💾 Data Storage

All data is stored in your browser's localStorage:
- **healthGuardianUser** → Your profile
- **healthGuardianMedicines** → All medicines
- **healthGuardianProgress** → Daily taken/missed/skipped status
- **healthGuardianSettings** → Your preferences

This means:
✅ App works offline
✅ Data persists between sessions
✅ No account needed
⚠️ Data is local only (not synced across devices)

---

## 📊 Data Backup

### Automatic Backup (Download JSON)
1. Settings → Data Management → Export Data
2. File downloads: `health-guardian-backup-YYYY-MM-DD.json`
3. Save to your computer

### Restore (Manual Import)
- Currently manual: Copy JSON to localStorage
- (Future feature will allow import button)

### Clear Everything
1. Settings → Data Management → Clear All Data
2. Confirm twice
3. Returns to registration page
4. All data deleted (irreversible!)

---

## 🌐 Supported Browsers

| Browser | Support | Notes |
|---------|---------|-------|
| Chrome | ✅ Full | Best experience |
| Edge | ✅ Full | Recommended |
| Firefox | ✅ Full | Good support |
| Safari | ⚠️ Limited | Voice may not work |
| Opera | ✅ Full | Should work fine |
| Mobile Chrome | ✅ Full | Touch-friendly |
| Mobile Firefox | ✅ Full | Good mobile support |
| Mobile Safari | ⚠️ Limited | Some features missing |

---

## 🚨 Important Notes

1. **One User Per Browser**: This app stores one user's data per browser
2. **No Cloud Sync**: Data doesn't sync across devices
3. **No Account**: No login/password needed (trade-off for privacy)
4. **Private Browsing**: Won't work in incognito/private mode
5. **Storage Limit**: ~5-10MB per browser (plenty for 100+ medicines)

---

## 💡 Tips & Tricks

### Tip 1: Set Reminder Times Wisely
- Breakfast: 08:00
- Lunch: 12:30
- Evening: 18:00
- Bedtime: 21:00

### Tip 2: Use Notes Field
Example: "Take with water", "Store in cool place"

### Tip 3: Set End Dates
If taking medicine for specific duration, set end date
Medicine automatically stops showing after end date

### Tip 4: Use Photos
Upload medicine package photo for visual confirmation

### Tip 5: Weekly Patterns
Perfect for "every other day" type medicines:
- Use Weekly frequency
- Select: Mon, Wed, Fri (or custom pattern)

### Tip 6: Custom Cycles
Example: Medication that's "3 days on, 2 days off"
- Frequency: Custom Cycle
- Intake Days: 3
- Pause Days: 2

### Tip 7: Test Before Relying
Test reminder system once after setup
Click manual test button to verify

### Tip 8: Backup Regularly
Export data monthly as backup
Attach to email or save to cloud

---

## 📞 Need Help?

1. Check the **Test the Application** section below
2. Review **Troubleshooting** section above
3. Check browser console (F12 → Console tab) for errors
4. Try different browser or device
5. Clear cache and try again

---

## ✅ Test the Application

### Test 1: Basic Reminder (5 min)
```
✓ Set medicine time to current time ± 1 minute
✓ Wait for reminder modal to appear
✓ Click different buttons (Done/Skip/Remind Later)
✓ Verify modal closes and action is recorded
```

### Test 2: Voice Reminder (3 min)
```
✓ Go to Settings
✓ Click "Test Voice Reminder"
✓ Verify you hear voice speaking
✓ Check language matches your setting
```

### Test 3: Progress Tracking (3 min)
```
✓ Mark several medicines as Taken
✓ Go to Progress screen
✓ Select medicine
✓ Verify calendar shows green days for today
✓ Check adherence % is calculated
```

### Test 4: Language Change (2 min)
```
✓ Change language to Spanish
✓ Verify ALL text changes immediately
✓ Change back to English
✓ Verify text reverts
```

### Test 5: Data Export (2 min)
```
✓ Go to Settings
✓ Click "Export Data"
✓ Verify JSON file downloads
✓ Open file in text editor
✓ Verify your data is there
```

### Test 6: Offline Mode (2 min)
```
✓ Turn off internet
✓ Verify app still works
✓ Add medicine
✓ Mark medicine as taken
✓ Verify data is still there after refresh
✓ Turn internet back on
```

---

## 🎯 You're All Set!

Everything is ready to use. Start adding your medicines and let Health Guardian help you stay on track with your medications!

**Key Reminders:**
- ✅ Medicines must be set to today or earlier start date
- ✅ Time format is 24-hour (08:00, 14:30, 21:00)
- ✅ Reminders check every 10 seconds
- ✅ All data is offline
- ✅ Backup data regularly
- ✅ Test reminder system once after setup

**Happy tracking! 💊**
