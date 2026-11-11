# Discord Rich Presence

JOLT 0.1.1 uses Discord's documented local named-pipe RPC. The ordinary Discord desktop client must be running and signed in. No bot, token, client secret, OAuth authorization, Local API, web server or additional permanent process is required. Rich Presence remains **off by default** and can be disabled entirely in Settings → Discord. Existing privacy settings are preserved.

The default public Application ID is **1570159402856751135**. It can be changed in the existing Discord settings page. Invalid or missing IDs prevent connections and show an actionable message. The Discord Developer Portal application should be named **JOLT**. The activity type is Listening, with the real track title, artist, album art when supported, a JOLT brand badge, and a clock-based timeline. Existing time/status/link/Play song preferences are retained. Pausing removes the running timeline; the existing clear-after-pause setting controls how long paused metadata remains. Stopping, disabling presence and exiting clear the activity.

Track changes and seeks are coalesced to no more than one activity update per five seconds. Discord advances its own timeline. Pipe reads use IO completion events on the existing runtime; there is no per-second Discord polling timer. Reconnect attempts use bounded exponential backoff only while playback needs a presence. A missing or disconnected client never blocks the UI or audio engine. Handshakes must return READY and activity replies are checked. Connection and command waits are bounded.

## Download JOLT

The intended stable installer address is:

https://github.com/subliminalsidkthemeaning/JOLT/releases/latest/download/JOLT-Setup-x64.exe

The portable ZIP remains a separate, versioned asset. The installer retains its internal version metadata while its public asset filename remains **JOLT-Setup-x64.exe**.

JOLT activates the Download JOLT button only after an anonymous HTTPS request succeeds and returns a Windows executable signature. It reads only the initial response bytes, does not execute anything, and caches successful verification with the settings. An unverified, failed, local, non-HTTPS or otherwise invalid address cannot become an activity button. Enable Discord or choose **Save and verify** to check the configured URL. Changing the URL invalidates its previous verification. A viewer's button opens their browser and follows normal browser download/security behavior. Discord commonly hides activity buttons from their owner; verify with another account or viewer.

## Upload the official logo

The prepared **jolt.png** is 1024 × 1024 PNG, under 1 MB, derived by high-quality scaling from the existing official bee with the same square crop used by JOLT's icons. The original supplied JPEG remains unchanged. It is not a newly generated or redrawn logo.

1. Open https://discord.com/developers/applications/1570159402856751135 .
2. Select **JOLT**, then **Rich Presence → Art Assets** (or the current equivalent Assets page).
3. Upload the provided **jolt.png**. Set its asset key to **jolt** and save. Discord normalizes asset keys to lowercase.
4. Allow Discord's asset cache time to refresh; restart JOLT's Discord connection if needed.
5. The current album image is used when its public HTTPS URL fits Discord's field limits. Otherwise the large image uses **jolt**. The small branding image also uses **jolt**. Until this asset is uploaded, Discord may omit the fallback/badge.

The application name and asset are managed in your Developer Portal. JOLT does not request your Discord credentials or change Discord activity-privacy settings. If an activity is hidden, check those preferences yourself.

Official references: https://docs.discord.com/developers/topics/rpc and https://docs.discord.com/developers/discord-social-sdk/development-guides/setting-rich-presence .
