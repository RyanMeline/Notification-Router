# Notification Router
Forwards GitHub notifications to Slack and Discord

# Secrets used:

Note - The Slack secrets and Discord secrets are optional, omitting either or both of them won't break the workflow, it just won't run that step.  

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
Have the scheduler make a POST request to "https://api.github.com/repos/[YOUR USERNAME]/[REPO NAME]/actions/workflows/Notify.yml/dispatches"  
---
When setting up the worker for cloudflair, replace with src/index.js contents with:
```
export default {
  async scheduled(event, env, ctx) {
    const response = await fetch(
      "https://api.github.com/repos/USERNAME/REPO/actions/workflows/WORKFLOW_NAME.yml/dispatches",
      {
        method: "POST",
        headers: {
          "Authorization": `Bearer ${env.GITHUB_TOKEN}`,
          "Accept": "application/vnd.github+json",
          "User-Agent": "cloudflare-worker",
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          ref: "main" // the branch to run the workflow on
        })
      }
    );
    console.log("Status:", response.status);
  }
};
```
