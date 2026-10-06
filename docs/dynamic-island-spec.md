# Dynamic Island implementation spec

## Platform
- Kotlin
- Jetpack Compose
- minSdk 35
- targetSdk 35

## Architecture
- MainActivity: permissions and settings
- OverlayService: lifecycle owner for the island overlay
- NotificationListenerService: receives notification events
- MediaController layer: observes active media sessions
- IslandState: single UI state model exposed to Compose

## Overlay
Use TYPE_APPLICATION_OVERLAY with a transparent window and a ComposeView. Anchor the island to the top-center safe area. Avoid blocking the status bar except for the island surface itself.

## States
1. Idle: small black pill.
2. Notification: icon + app name/title; animate from compact to expanded.
3. Media: artwork/icon + track title + playback affordance.
4. Timer/progress: compact progress indicator.
5. Expanded: larger card with contextual actions.

## Interaction
- Tap: open contextual action / source app.
- Long press: expand.
- Swipe/dismiss: hide current event.

## Permissions
- ACTION_MANAGE_OVERLAY_PERMISSION for overlay.
- ACTION_NOTIFICATION_LISTENER_SETTINGS for notification access.
- Media sessions through MediaSessionManager with an enabled notification listener / notification access flow where required.

## Reliability
- Foreground service notification.
- Recreate overlay on service restart.
- Debounce high-frequency media updates.
- Do not poll continuously when event-driven APIs are available.
