# Notification System - Discord Hook

Telegram Explorer allows to send notifications through Discord WebHooks. Each WebHook is linked to a specific channel.

This way you can configure many notification hooks, one for each need or category you like.

Every Notification is defined in the configuration files.

**Configuration Spec:**

For each notification hook you must set a configuration using the default name schema *NOTIFIER.DISCORD.<HOOK_NAME\>*

**Parameters:**

  * **webhook** > Required - Discord Webhook URI
  * **prevent_duplication_for_minutes** > Required - Time (in minutes) that the system keep track of messages sent to Discord servers to prevent others message with same content to be sent to the webhook. If you don't want to use this feature, just set the parameter to 0.
  * **timeout_seconds** > Optional - Timeout (in seconds) that waits to send the message. If the message sent take more that time, the message will be ignored.
    * Default: 30
  * **media_attachments_enabled** > Optional - Enable/Disable the behavior for sending downloaded medias on messages that have been reported. 
    * Default: false
  * **media_attachments_max_size_bytes** > Optional - Set the max size in bytes to send the medias on the notifications.
    * Default: 10000000

=true
media_attachments_max_size_bytes=10000000
**Changes on Configuration File**
```ini
[NOTIFIER.DISCORD.MY_HOOK_1]
webhook=https://discord.com/api/webhooks/1157896186751897357/o7foobar4txvAvKSdeadHiI-9XYeXaGlQtd-5PtrrX_eCE0XElWktpPqjrZ0KbeefPtQC
prevent_duplication_for_minutes=240
timeout_seconds=30
media_attachments_enabled=true
media_attachments_max_size_bytes=10000000

[NOTIFIER.DISCORD.MY_HOOK_2]
webhook=https://discord.com/api/webhooks/1128765187657681875/foobarqOMFp_4tM2ic2mbeefNPOZqJnBZZdfaubQv2vJgbYzfdeadZd5aqGX6FmCmbNjX
prevent_duplication_for_minutes=240
media_attachments_enabled=false
media_attachments_max_size_bytes=10000000

[NOTIFIER.DISCORD.MY_HOOK_3]
webhook=https://discord.com/api/webhooks/1256789875462124045/bQ9TZqOzgA05PLVu8E2LU3N5foobarFU8-0nQbeefP5oIgAUOlydeadf7Uc19Hs00OJQ
prevent_duplication_for_minutes=60
timeout_seconds=30
media_attachments_enabled=true
media_attachments_max_size_bytes=25000000

[NOTIFIER.DISCORD.MY_HOOK_4]
webhook=https://discord.com/api/webhooks/1487651987651004895/mR0v3zOywH3Z5HvdeadrGEqqndkcYepgCM-Q6foobardjAMXAEbeefuA_F7-h5JcBM4RT
prevent_duplication_for_minutes=240
media_attachments_enabled=true
```
