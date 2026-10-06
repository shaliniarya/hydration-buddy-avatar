# Hydration Buddy Avatar

A Claude Code skill that turns your own caricature into a desktop buddy that reminds you to drink water.

On a timer, your caricature pops up in the corner of your screen with a message like *"Hydration check, Vik! Grab a glass."* Click **Drink** once you have, or **Snooze** to be reminded again shortly. Snooze and the next reminder gets cheekier. It floats over whatever app you're using, even full-screen, without interrupting your typing.

You don't run any setup commands yourself. Claude Code does all of it.

## Requirements

- A Mac
- [Claude Code](https://claude.com/claude-code)

## Install

Paste this into Terminal:

```bash
git clone https://github.com/shaliniarya/hydration-buddy-avatar ~/.claude/skills/hydration-buddy-avatar
```

Then restart Claude Code.

## Use

1. In Claude Code, type `/hydration-buddy-avatar`.
2. Give Claude two caricature images: one standing normally, one holding a water bottle. No images yet? Claude gives you a prompt to paste into ChatGPT or Gemini (NanoBanana) along with a photo of yourself. Drag the downloaded images into the Claude Code chat.
3. Answer three quick questions:
   - your name
   - how often to remind you and how long a snooze lasts (defaults: 60 and 10 minutes)
   - optionally, someone to "tell on you" to if you snooze
4. Claude installs everything and starts the app. A 💧 icon appears in your menu bar. Click it and choose **Remind me now** to see your buddy straight away.

Claude also offers to start it automatically every time you log in.

## Change settings later

Ask Claude Code, for example: *"change my hydration buddy avatar to remind me every 45 minutes"*.

Or edit the settings block at the top of `~/hydration-buddy-avatar/buddy.py`, then quit from the 💧 menu and start it again.

## What's in this repo

| File | What it does |
| --- | --- |
| `SKILL.md` | Instructions Claude Code follows to set everything up |
| `buddy.py` | The app itself |
| `prepare_images.py` | Removes the white background from your caricature images |
