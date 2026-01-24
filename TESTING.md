# 🧪 Testing Guide - Health Guardian

## How to Test the App

### ✅ Test Setup

1. **Open the app**
   - Open `index.html` in your web browser
   - Should see landing page with "Launch App" button

2. **Verify registration**
   - Click "Launch App"
   - Should redirect to `registration.html` (first time users)
   - Complete 3-step registration

3. **Verify app loads**
   - After registration, should see main dashboard
   - Bottom navigation should show: Home, Medicines, Progress, Settings

---

## 🧪 Feature Testing Checklist

### USER ONBOARDING
- [ ] Registration form loads
- [ ] Step 1: Can enter name and age
- [ ] Step 1: Validates name (min 2 chars)
- [ ] Step 1: Validates age (1-150)
- [ ] Step 2: Language selector works
- [ ] Step 2: Gender selector works (optional)
- [ ] Step 3: Voice preference selector works
- [ ] Step 3: Volume level selector works
- [ ] Data saves to localStorage after completion
- [ ] App redirects to home after registration
- [ ] On return visit, skips registration

### HOME SCREEN
- [ ] Shows today's date
- [ ] Displays medicines scheduled for today
- [ ] Shows medicine image, name, dosage
- [ ] Shows medicine time and instructions
- [ ] Status circle visible for each medicine
- [ ] FAB button appears (bottom right)
- [ ] Empty state when no medicines scheduled
- [ ] Status circle clickable (toggles taken/missed/skipped)

### ADD MEDICINE FORM
- [ ] Photo upload works
- [ ] Photo preview shows selected image
- [ ] Medicine name field works
- [ ] Dosage amount field accepts numbers
- [ ] Dosage unit dropdown has correct options
- [ ] Intake instruction dropdown works
- [ ] Time picker shows HH:MM format
- [ ] Start date defaults to today
- [ ] Start date minimum is today
- [ ] End date is optional
- [ ] Notes field is optional
- [ ] Frequency buttons: Daily selected by default
- [ ] Weekly frequency shows day selector
- [ ] Day selector buttons toggle active state
- [ ] Custom cycle shows intake/pause inputs
- [ ] Form validation shows errors for required fields
- [ ] Submit button saves medicine to list
- [ ] Success notification appears after save
- [ ] Form resets after submission
- [ ] Redirects to medicines list after add

### MEDICINES LIST
- [ ] Shows all added medicines
- [ ] Each medicine shows: image, name, dosage, time, instructions
- [ ] Edit button (✏️) works
- [ ] Clicking edit populates form with medicine data
- [ ] Delete button (🗑️) works
- [ ] Delete shows confirmation dialog
- [ ] Medicine deleted after confirmation
- [ ] List updates after edit/delete
- [ ] Empty state when no medicines

### MEDICINE EDITING
- [ ] Form pre-fills with medicine data
- [ ] Button text changes to "Update Medicine"
- [ ] Can modify any field
- [ ] Changes save correctly
- [ ] Back button cancels editing
- [ ] Success notification shows
- [ ] List reflects changes

### DAILY PROGRESS
- [ ] Medicine selector shows all medicines
- [ ] Selecting medicine shows calendar
- [ ] Current month displays correctly
- [ ] Calendar shows 7 days of week headers
- [ ] Dates show in grid layout
- [ ] Previous/Next month buttons work
- [ ] Color coding shows:
  - [ ] 🟢 Green for taken medicines
  - [ ] 🔴 Red for missed medicines
  - [ ] 🟡 Yellow for skipped medicines
  - [ ] ⚪ Gray for not due
- [ ] Legend shows explanation
- [ ] Dates in future show different color

### FREQUENCY CALCULATION
#### Daily Frequency
- [ ] Add medicine with "Daily" frequency
- [ ] Verify shows in Today's Medicines
- [ ] Verify shows for next several days in progress

#### Weekly Frequency
- [ ] Add medicine with "Weekly" frequency
- [ ] Select Mon, Wed, Fri
- [ ] Verify appears only on those days
- [ ] Verify not on Tue, Thu, Sat, Sun

#### Monthly Frequency
- [ ] Add medicine with "Monthly" frequency on 15th
- [ ] Verify appears on 15th each month

#### Custom Cycle
- [ ] Add medicine: Intake 1 day, Pause 2 days
- [ ] Verify pattern: Take-Skip-Skip-Take-Skip-Skip
- [ ] Continue tracking for 10+ days
- [ ] Verify pattern continues correctly

### SETTINGS
- [ ] User profile section shows name and age
- [ ] Language selector shows all 11 languages
- [ ] Language selection saves
- [ ] Voice selector shows Male/Female/Neutral
- [ ] Voice selection saves
- [ ] Volume selector shows Low/Normal/High
- [ ] Volume selection saves
- [ ] Large Text checkbox toggles
- [ ] Text size increases when checked
- [ ] High Contrast checkbox toggles
- [ ] Contrast increases when checked
- [ ] Vibration checkbox toggles
- [ ] Export Data button works
- [ ] File downloads as JSON
- [ ] Clear Data button shows confirmation
- [ ] Requires double confirmation
- [ ] After clear, redirects to registration

### BOTTOM NAVIGATION
- [ ] 4 nav buttons visible
- [ ] Current screen button highlighted
- [ ] Clicking button switches screens
- [ ] All screens accessible from nav
- [ ] Nav icons and labels visible
- [ ] Mobile: Labels hidden on small screens

### RESPONSIVE DESIGN
#### Desktop (1200px+)
- [ ] Layout looks good
- [ ] All content visible
- [ ] No horizontal scrolling
- [ ] Calendar shows full month

#### Tablet (768px)
- [ ] Content fits screen
- [ ] Buttons appropriately sized
- [ ] Medicine cards display correctly
- [ ] Navigation works smoothly

#### Mobile (480px)
- [ ] Content stacks vertically
- [ ] Buttons large enough to tap
- [ ] Form fields readable
- [ ] Bottom nav accessible
- [ ] No content cut off

#### Small Mobile (375px)
- [ ] All features accessible
- [ ] Text readable
- [ ] Buttons tappable (40px+)
- [ ] FAB button positioned correctly

### ACCESSIBILITY FEATURES
- [ ] Large text mode increases all fonts
- [ ] High contrast mode improves visibility
- [ ] Vibration can be toggled on/off
- [ ] Color scheme supports colorblindness
- [ ] Form labels visible
- [ ] Focus indicators visible on form fields
- [ ] Keyboard navigation works

### DATA PERSISTENCE
- [ ] After adding medicine, refresh page
- [ ] Medicine still there
- [ ] After marking done, refresh page
- [ ] Status still marked
- [ ] Close browser, reopen
- [ ] All data still there
- [ ] Settings preferences persist
- [ ] User data persists

### BROWSER COMPATIBILITY
Test in each browser:
- [ ] Chrome/Chromium
- [ ] Firefox
- [ ] Safari
- [ ] Edge

### ERROR HANDLING
- [ ] Try submitting blank form
- [ ] Error message appears
- [ ] Can't select age > 150
- [ ] Can't select age < 1
- [ ] Can't set end date before start date
- [ ] Can't select past date for medicine
- [ ] Graceful errors if voice not available

### VOICE FEATURES
- [ ] Voice synthesis works in settings
- [ ] Language changes affect voice
- [ ] Voice gender preference works
- [ ] Volume adjustment works
- [ ] Falls back gracefully if not available

### REMINDER MODAL
- [ ] Shows medicine image
- [ ] Shows medicine name
- [ ] Shows dosage
- [ ] Shows instructions
- [ ] Three buttons visible
- [ ] Close button works
- [ ] Can mark as done
- [ ] Can snooze
- [ ] Can skip

---

## 🧪 Test Scenarios

### Scenario 1: Simple Daily Medicine
1. Register user
2. Add "Aspirin, 500mg, Daily at 9:00 AM, After meal"
3. Go to Today's medicines - should show
4. Go to Progress - should show calendar
5. Click status dot - mark as taken
6. Refresh page - should still be marked taken
7. ✅ PASS

### Scenario 2: Weekly Medicine
1. Add "Vitamin D, 1 tablet, Weekly on Mon/Wed/Fri, 10:00 AM"
2. Go to Progress - calendar shows
3. Check Monday - should be green (not done yet)
4. Check Tuesday - should be gray (not due)
5. Navigate to next month - pattern continues correctly
6. ✅ PASS

### Scenario 3: Medicine Course with Dates
1. Add "Antibiotic, 500mg, Daily, 01/16/2026 to 01/23/2026"
2. On 01/16 - should appear in today
3. On 01/23 - should appear in today
4. On 01/24 - should NOT appear (course ended)
5. ✅ PASS

### Scenario 4: Custom Cycle
1. Add "Medicine, 1 tablet, Custom Cycle: 1 intake, 2 pause"
2. Day 1: Should be due
3. Day 2: Should NOT be due
4. Day 3: Should NOT be due
5. Day 4: Should be due
6. Pattern continues...
7. ✅ PASS

### Scenario 5: Multi-Language Support
1. Register with English
2. Go to Settings - select Spanish
3. Add medicine
4. Verify reminders will play in Spanish
5. Change to Hindi
6. Verify voice setting works
7. ✅ PASS

### Scenario 6: Data Export
1. Add 3 medicines with progress
2. Go to Settings - click Export
3. JSON file downloads
4. Open file - contains all medicines and progress
5. ✅ PASS

### Scenario 7: Accessibility
1. Go to Settings
2. Enable Large Text - all fonts increase
3. Enable High Contrast - colors change
4. Disable Vibration
5. Refresh page - settings persist
6. ✅ PASS

---

## 🐛 Known Issues & Workarounds

### Issue: Voice not working
**Workaround**: Requires internet connection for voice synthesis on some browsers

### Issue: Notifications blocked
**Workaround**: Allow notifications in browser settings

### Issue: Data lost after clearing browser cache
**Workaround**: Export data regularly as backup

### Issue: Custom cycle calculation
**Workaround**: Verify dates carefully when setting up

---

## 📝 Test Log Template

```
Date: __________
Tester: __________
Browser: __________
Device: __________

Feature: __________
Expected Result: __________
Actual Result: __________
Status: [ ] PASS [ ] FAIL

Issues Found:
1. ________________________
2. ________________________

Notes:
________________________
________________________
```

---

## ✅ Pre-Release Checklist

- [ ] All screens load without errors
- [ ] All forms validate properly
- [ ] Data persists correctly
- [ ] No console errors (F12)
- [ ] Responsive on all screen sizes
- [ ] Works in all major browsers
- [ ] Voice reminders work
- [ ] Progress tracking accurate
- [ ] Settings apply correctly
- [ ] Data export works
- [ ] No memory leaks
- [ ] Performance acceptable
- [ ] Documentation complete
- [ ] README clear and helpful
- [ ] Quick start guide complete

---

## 🚀 Performance Testing

### Load Time
- [ ] App loads in < 2 seconds

### Responsiveness
- [ ] Navigation switches instant
- [ ] Form inputs responsive
- [ ] Calendar rendering smooth
- [ ] No jank or stuttering

### Memory Usage
- [ ] Add 20 medicines
- [ ] App still responsive
- [ ] No memory leak over 1 hour use

### localStorage Usage
- [ ] Typical usage: 1-5 MB
- [ ] Monitor with DevTools
- [ ] No excessive growth

---

## 🔒 Security Testing

- [ ] No sensitive data in console logs
- [ ] No data sent to external servers
- [ ] localStorage not exposing passwords
- [ ] No SQL injection possible
- [ ] Form inputs sanitized
- [ ] XSS protection verified

---

## 📱 Mobile Testing Checklist

- [ ] Touch events work (no hover)
- [ ] Buttons large enough (40px+)
- [ ] Forms don't zoom in
- [ ] Bottom nav always visible
- [ ] Keyboard doesn't cover inputs
- [ ] No horizontal scrolling
- [ ] Mobile browsers compatible

---

Thank you for testing Health Guardian! Your feedback helps us make it better. 🩺✅
