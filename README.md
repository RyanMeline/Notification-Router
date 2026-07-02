# Secrets used:
## GitHub:
GH_AUTH_TOKEN  
- Token with the 'Notifications' permission
For external scheduler
- A Token with the full 'repo' permission  

## Slack:
SLACK_BOT_TOKEN
- Oauth token from an App with the chat:write permission
SLACK_MEMBER_ID
- Channel ID for messages to be sent | User ID for DMs

## Discord:
DISCORD_WEBHOOK_URL
- The Webhook URL for whatever channel you want the notifications forwarded to

# External Scheduler
Cron is unreliable at best, so I used an external scheduler.  
Whatever external scheduler you want to use is fine, I used cloudflair because its easy to set up and uses secrets.  
Have the scheduler call "https://api.github.com/repos/\<YOUR USERNAME\>/Notification-Router/actions/workflows/Notify.yaml/dispatches"
