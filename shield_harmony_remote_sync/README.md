# Shield App Launcher for Harmony Remote

This blueprint creates an automation that automatically launches applications on your NVIDIA Shield when your Harmony Remote activity changes. When you select an activity like "Watch YouTube" on your Harmony Remote, the corresponding app will automatically launch on your Shield.

## Features

- Automatically launches apps on your NVIDIA Shield based on Harmony Remote activities
- Includes package names for 30+ popular streaming apps
- Customizable delay for Shield startup
- Easy to configure with a simple activity naming convention
- Support for custom app mappings

## Prerequisites

- Home Assistant with the following integrations:
  - Logitech Harmony Hub integration
  - Android TV integration for your NVIDIA Shield
- A Harmony Remote with activities configured for your Shield

## Installation

1. Click the button below to import this blueprint into your Home Assistant instance:

   [![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2FADeane6%2Fha_blueprints%2Frefs%2Fheads%2Fmain%2Fshield_harmony_remote_sync%2Fshield_harmony_remote_sync.yaml)

   Alternatively, copy the YAML code and manually import it via **Settings -> Automations & Scenes -> Blueprints -> Import Blueprint**.

2. Create a new automation using this blueprint via **Settings -> Automations & Scenes -> Create Automation -> Use Blueprint**.

## Configuration

### Harmony Remote Setup

For this blueprint to work properly, your Harmony Remote activities need to follow a specific naming convention:

1. **Activity Naming Convention**: All activities that should trigger app launching must start with "Watch " followed by the app name.

   Examples:

   - "Watch YouTube"
   - "Watch Netflix"
   - "Watch Plex"
   - "Watch Disney+"

2. **Setting Up Activities in Harmony**:

   - Open the Harmony app on your mobile device
   - Select your Harmony Hub
   - Go to "Activities"
   - Create a new activity or edit an existing one
   - Name it following the convention (e.g., "Watch YouTube")
   - Configure the activity to use your Shield device
   - Set up the appropriate power and input settings

3. **Verify Activity Names**:
   - In Home Assistant, go to **Developer Tools -> States**
   - Find your Harmony Remote entity (e.g., `remote.harmony_hub`)
   - Look at the `activity_list` attribute to see all your available activities
   - Make sure your activities follow the "Watch [App]" naming convention

### Blueprint Configuration

When setting up the automation with this blueprint, you'll need to configure the following:

1. **Harmony Remote**: Select your Harmony Remote entity (e.g., `remote.harmony_hub`)

2. **Shield Android TV Entity**: Select your Shield Android TV entity (e.g., `media_player.shield_android_tv`)

3. **Shield Startup Delay**: Set the delay in seconds to wait before launching an app if the Shield was off (default: 15 seconds). This is to allow for the harmony remote turning on any media system and the shield.

4. **Custom Application Mappings**: (Optional) Add custom mappings for activities to app package names if needed

### App Mappings

The blueprint includes default mappings for many popular apps. You only need to add custom mappings in these cases:

1. For apps not included in the default list
2. If your Harmony activity names don't match the default ones
3. If you want to override a default mapping

To find the correct package name for an app not in the default list:

1. Connect to your Shield via ADB: `adb connect [Shield IP address]`
2. Run: `adb shell pm list packages`
3. Find the package name in the list (e.g., `package:com.example.app`)

Then add it to your custom mappings:

```yaml
custom_app_mappings:
  "Watch My Custom App": "com.example.app"
```

## Example Configuration

Here's an example of a complete configuration:

```yaml
harmony_remote: remote.harmony_hub
shield_android_tv: media_player.shield_android_tv
startup_delay: 15
custom_app_mappings:
  "Watch Movies": "org.xbmc.kodi"
  "Watch TV Shows": "com.plexapp.android"
  "Watch My Photos": "com.google.android.apps.photos"
```

## Troubleshooting

If apps aren't launching correctly:

1. **Check Activity Names**: Verify that your Harmony activity names exactly match what's in your mappings
2. **Check Package Names**: Make sure the package names are correct for your apps
3. **Increase Startup Delay**: If apps don't launch when the Shield was off, try increasing the startup delay
4. **Check Logs**: Look at the Home Assistant logs to see if there are any errors
5. **Test Manually**: Try launching the app manually using the Developer Tools -> Services with the `media_player.play_media` service

## Default App Mappings

The blueprint includes mappings for these common apps (activity name -> package name):

- Watch YouTube -> com.google.android.youtube.tv
- Watch Netflix -> com.netflix.ninja
- Watch Prime Video -> com.amazon.amazonvideo.livingroom
- Watch Disney+ -> com.disney.disneyplus
- Watch Plex -> com.plexapp.android
- Watch HBO Max -> com.hbo.hbonow
- Watch Hulu -> com.hulu.livingroomplus
- Watch Kodi -> org.xbmc.kodi
- Watch Spotify -> com.spotify.tv.android
- ...and many more!

## Advanced Usage

### Creating Multiple Activities for the Same App

You can create multiple Harmony activities that launch the same app. For example:

- "Watch Movies" -> Kodi
- "Watch TV Shows" -> Plex
- "Watch Kids Movies" -> Disney+

Just add these to your custom mappings with the appropriate package names.

### Customizing Delays

If some apps need more time to launch than others, you can create separate automations with different delay settings for specific activities.

## Feedback and Contributions

If you find issues or have suggestions for improvements, please open an issue or pull request on the GitHub repository.
