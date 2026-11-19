# Discord Missions Automator

A tool designed to automate Discord Missions using **Discord PTB** or **Discord Canary**.

## Features

- **Accept All Missions:** Automatically accepts all available missions.
- **Run All Missions Simultaneously:** All missions are processed at the same time instead of one by one.
- **Individual Time Management:** Each mission respects its own required duration. For example, 15-minute missions still take 15 minutes, but all missions progress simultaneously.
- **Video Missions:** Videos are played for approximately 1 second and then closed, allowing the mission to register as accepted.

## Requirements

- Discord PTB or Discord Canary.
- Access to Discord Missions.

## How to Use

1. Open **Discord PTB** or **Discord Canary**.
2. Press `CTRL + SHIFT + I` to open Developer Tools.
3. Navigate to the **Console** tab.
4. Paste the provided code using `CTRL + V`.
5. Discord may display a warning asking you to type:

   ```text
   allow pasting
   ```

6. Press `ENTER`.
7. Paste the code again and execute it.

## How It Works

The tool processes all available missions simultaneously, respecting the individual time requirements of each mission.

For example:
- A 15-minute mission takes 15 minutes to complete.
- A 10-minute mission takes 10 minutes to complete.
- Both missions progress simultaneously.

Video missions are handled separately by briefly playing the video for approximately one second before closing it.

## Compatibility

- Discord PTB
- Discord Canary

---

**Disclaimer:** This is an unofficial community project and is not affiliated with or endorsed by Discord. Use at your own risk.
