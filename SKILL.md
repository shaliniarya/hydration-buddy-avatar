---
name: hydration-buddy-avatar
description: Builds and runs a floating macOS water-reminder buddy from the user's caricature images. Use when the user asks to set up, build, install or create their hydration buddy avatar / hydration reminder app.
---

# Hydration Buddy Avatar setup

Build the user a macOS menu-bar app that pops up their caricature on a timer and asks them to drink water, with Drink and Snooze buttons. Do every step yourself with your tools. Never ask the user to type commands. This folder contains `buddy.py` (the app) and `prepare_images.py` (image cleanup).

## 1. Check the machine
- Confirm macOS (`uname`). If not macOS, stop and say it only runs on Mac.
- Run `python3 --version`. If missing, run `xcode-select --install` and tell the user to click Install in the popup, then wait and re-check.

## 2. Get the images
Ask the user for two caricature images on a plain white background: an idle pose and a reminder pose holding a water bottle. If they don't have them, give them these prompts for ChatGPT or Gemini (NanoBanana), to use with a photo of themselves:

> Turn this person into a cute Pixar-style cartoon caricature, full body, standing, plain solid white background, no text or watermark.

then, in the same chat:

> Same character, same outfit, full body, holding a blue water bottle and pointing at it, smiling. Plain solid white background.

Ask them to drag both downloaded files into this chat (that gives you the paths). One image is fine; use it for both poses.

## 3. Ask three questions
- Name the buddy should call them.
- How often to remind them (default 60 minutes) and snooze length (default 10 minutes).
- Optional: a name to "tell on them" to after a snooze (e.g. a partner). Blank skips it.

## 4. Build it
1. `mkdir -p ~/hydration-buddy-avatar/assets`
2. Copy `buddy.py` and `prepare_images.py` from this skill's folder into `~/hydration-buddy-avatar/`.
3. `cd ~/hydration-buddy-avatar && python3 -m venv venv && ./venv/bin/pip install --quiet rumps pyobjc-framework-Cocoa Pillow`
4. `./venv/bin/python prepare_images.py "<idle image>" "<reminder image>" assets`
5. Edit the settings block at the top of `~/hydration-buddy-avatar/buddy.py`: `USER_NAME`, `REMIND_EVERY_MINUTES`, `SNOOZE_MINUTES`, `ACCOUNTABILITY_NAME`.

## 5. Run it
`nohup ~/hydration-buddy-avatar/venv/bin/python ~/hydration-buddy-avatar/buddy.py > ~/hydration-buddy-avatar/buddy.log 2>&1 &`

Tell the user a 💧 icon is now in the menu bar; "Remind me now" there shows the buddy instantly. If `buddy.log` shows an error, fix it before finishing.

## 6. Offer auto-start at login
If they say yes, write `~/Library/LaunchAgents/com.hydrationbuddyavatar.plist` (expand `~` to the real home path):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0"><dict>
  <key>Label</key><string>com.hydrationbuddyavatar</string>
  <key>ProgramArguments</key><array>
    <string>HOME/hydration-buddy-avatar/venv/bin/python</string>
    <string>HOME/hydration-buddy-avatar/buddy.py</string>
  </array>
  <key>RunAtLoad</key><true/>
</dict></plist>
```

Kill the manually started copy first (`pkill -f "hydration-buddy-avatar/buddy.py"`), then `launchctl load ~/Library/LaunchAgents/com.hydrationbuddyavatar.plist`.

## Changing things later
Edit the settings block in `~/hydration-buddy-avatar/buddy.py`, then restart: quit from the 💧 menu and rerun step 5 (or `launchctl unload` / `load` the plist).
