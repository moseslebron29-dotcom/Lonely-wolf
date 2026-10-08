# 🐺 LONELY WOLF v3.0.0

LONELY WOLF is a Node.js WhatsApp bot project with a mobile pairing website and Render deployment configuration.

## Current working foundation

### WhatsApp
- Real WhatsApp pairing-code request
- Multi-device authentication
- Session credentials saved to the configured auth directory
- Automatic reconnect unless the account is logged out
- Incoming command dispatcher

### Web
- Mobile-first pairing page
- Number validation
- Pairing-code display
- Connection status
- Rate-limited pairing endpoint
- No secrets in browser JavaScript

### Commands currently implemented

`.menu`
`.help`
`.support`
`.ping`
`.alive`
`.owner`
`.runtime`
`.id`
`.calc`
`.groupid`
`.tagall`
`.hidetag`
`.admins`
`.joke`
`.quote`
`.8ball`
`.truth`
`.dare`
`.wyr`
`.riddle`
`.sticker`
`.toimage`
`.tourl`

The architecture is modular, so additional command categories can be added without rewriting the WhatsApp connection layer.

## Android → GitHub → Render

You do NOT need Linux installed on your Android phone.

### 1. Create a GitHub repository

Create a new repository named:

`lonely-wolf`

Upload the contents of this ZIP so that `package.json`, `src/`, `web/`, and `render.yaml` are at the repository root.

Do not upload `.env` or WhatsApp authentication files.

### 2. Connect GitHub to Render

In Render:

1. Create a new Web Service.
2. Select the `lonely-wolf` repository.
3. Runtime: Node.
4. Build command:
   `npm install`
5. Start command:
   `npm start`

If you use the included `render.yaml`, Render can read the service configuration from the Blueprint.

### 3. Environment variables

Set:

`OWNER_NUMBER` = your WhatsApp number in international digits.

`PAIRING_SECRET` = a long private random value if you use it for additional protection.

The project already sets:

`AUTH_DIR=/var/data/auth`

### 4. Persistent storage

WhatsApp authentication files need persistent storage.

The included Render configuration requests a persistent disk mounted at:

`/var/data`

The auth files are stored under:

`/var/data/auth`

If your selected Render plan does not support the required persistent disk, do not assume the service filesystem is permanent. Use an appropriate persistent session-storage architecture instead.

### 5. Open the pairing site

After deployment, open the Render service URL in Chrome on Android.

The page is:

`https://YOUR-RENDER-SERVICE.onrender.com/`

Enter your WhatsApp number in international digits and request the pairing code.

Complete the linking process in WhatsApp's Linked Devices flow.

### 6. Test the bot

Send:

`.ping`

Expected:

`🏓 Pong!`

Then:

`.menu`

## Important security rules

Never commit:

- `.env`
- WhatsApp auth/session files
- private API keys
- database passwords

The pairing page must never contain server secrets.

## Adding future commands

Use one module per category:

`src/commands/media.js`
`src/commands/group.js`
`src/commands/fun.js`

Register the module in:

`src/commands.js`

Keep third-party API keys in environment variables.

## API-dependent commands

Commands such as advanced image generation, search, downloads, translation, weather, sports, and AI require external providers. They should be implemented only when a reliable provider and credentials are available.

Do not fake an API response and do not hardcode private credentials.

## Safety

This build intentionally excludes adult/sexual command categories. It also avoids features intended to bypass platform security, abuse rate limits, or conceal malicious activity.
