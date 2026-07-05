# 🩺 Health Guardian

A smart medication reminder web app that helps people — especially elderly users and caregivers — stay on track with complex medicine schedules, without needing a phone app store or account setup.

Built with Claude and Antigravity (AI-assisted development) for the Visionova Hackathon 2026.

---

## The Problem

Missing a dose isn't just forgetfulness — for people managing multiple medicines with different timings, meal instructions, and cycles (like "take 1 day, skip 2 days" for antibiotics), it's genuinely hard to track manually. Most reminder apps assume a single daily pill, require sign-up, and don't work well for elderly users who may struggle with small text, complex UI, or English-only voice alerts.

## The Solution

Health Guardian is a browser-based medicine tracker that:
- Lets you add each medicine with a photo, dosage, and custom schedule (daily, weekly, monthly, or custom intake/pause cycles)
- Sends full-screen, voice-narrated reminders in 11 languages including Hindi, Telugu, Tamil, and Marathi — so instructions aren't a barrier
- Tracks adherence on a color-coded calendar, so you (or a caregiver) can see at a glance what was taken, missed, or skipped
- Works fully offline with no login — all data stays in the browser via localStorage
- Includes accessibility options (large text, high contrast, vibration alerts) built for users who need them most

## Screenshots

<table>
<tr>
<td align="center"><b>Landing Page</b><br><img width="445" height="590" alt="landing page" src="https://github.com/user-attachments/assets/cf80ca83-4e29-4700-9a0a-0feb5a80451f" />

<td align="center"><b>Onboarding</b><br><img width="420" height="900" alt="onboarding" src="https://github.com/user-attachments/assets/1b121b0c-2e06-491b-91f9-6c968a519c1c" />

<td align="center"><b>Dashboard</b><br><img width="420" height="900" alt="dashboard" src="https://github.com/user-attachments/assets/3b934908-5dba-40ab-ac57-a08aaf4ac4ed" />

</tr>
</table>
## Why Not Just Use a Phone Alarm?

An alarm reminds you of a *time*. Health Guardian reminds you of a *treatment plan* and tracks whether you followed it — showing the medicine, dosage, and instructions on the reminder, handling schedules like custom intake/pause cycles, logging adherence on a calendar, and offering voice reminders in 11 languages plus large text and high contrast for accessibility.

## How It Works

1. **Onboarding** — user enters name, age, and preferred language/voice once
2. **Add a medicine** — photo, dosage, timing, and schedule type (daily / weekly / monthly / custom cycle)
3. **Get reminded** — at the scheduled time, a full-screen alert plays a voice reminder in the user's language and vibrates the device
4. **Respond** — mark as Done, Snooze 15 min, or Skip
5. **Track progress** — a monthly calendar shows adherence history per medicine, color-coded green/red/yellow

## Tech Stack

HTML5, CSS3, Vanilla JavaScript, Web Speech API (voice reminders), Web Notifications API, localStorage (offline data persistence)

## Run It Locally

No build step or server needed:
```bash
git clone https://github.com/adigoppulasanjana13-eng/health-guardian.git
cd health-guardian
```
Open `index.html` directly in a browser.

## What I'd Build Next

- Cloud sync so data isn't tied to a single browser/device
- Caregiver notifications for missed doses
- PDF export of medication history for doctor visits

---

**Note:** This project was built with AI-assisted development (Claude + Antigravity) as part of exploring applied AI for healthcare use cases.
