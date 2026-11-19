# Discord Missions Automator

A tool designed to automate Discord Missions using **Discord PTB** or **Discord Canary**.

## Features

- **Video Mission Activation:** Open each available video mission, play it for approximately 1 second, and close it to register the mission.
- **Accept All Missions:** After activating all video missions, manually accept the remaining missions.
- **Simultaneous Execution:** All accepted 15-minute missions run simultaneously instead of one by one.
- **Automatic Progress Tracking:** Tracks the progress of each mission independently.
- **Background Execution:** Once the script is running, you only need to keep Discord open. You can continue using Discord normally while missions progress.

## Requirements

- Discord PTB or Discord Canary.
- Access to Discord Missions.

## How to Use

1. Open **Discord PTB** or **Discord Canary**.
2. Open each available video mission.
3. Play each video for approximately **1 second**, then close it.
4. Repeat this process for every video mission.
5. Accept all remaining available missions (15-minute missions).
6. Press `CTRL + SHIFT + I` to open Developer Tools.
7. Navigate to the **Console** tab.
8. Paste the script using `CTRL + V`.
9. If Discord displays a warning, type:

   ```text
   allow pasting
   ```

10. Press `ENTER`.
11. Paste the script again and execute it.

## How It Works

After all missions have been activated and accepted, execute the script.

All supported 15-minute missions begin progressing simultaneously. Each mission maintains its own progress and required duration.

**Example:**

If you have 5 missions of 15 minutes, all 5 progress at the same time rather than being completed one after another.

Once the script is running:

- Keep Discord PTB or Canary open.
- You can continue using Discord normally.
- Missions continue progressing simultaneously.
- Each mission completes independently.

## Compatibility

- Discord PTB
- Discord Canary

---

**Disclaimer:** This is an unofficial community project and is not affiliated with or endorsed by Discord. Use at your own risk.
