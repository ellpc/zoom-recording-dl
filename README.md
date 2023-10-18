# zoom-rec-dl

Save Zoom cloud recordings to a local directory. A cross-platform video and audio download script.

## Requirements

- Windows, macOS, and Linux - any operating system that supports [Node.js] and [npm].
- Zoom cloud recording share link that does not require any additional authentication.

[node.js]: https://nodejs.org/
[npm]: https://www.npmjs.com/

---

For protected cloud recordings, the shareable link

- Should have the passcode embedded in the link.
- Should show a media player and a download button.
- Should not show an 'Enter the passcode to watch'.

Reference the [documentation](https://support.zoom.us/hc/en-us/articles/11692220055821) and update the following settings in the Zoom web portal.

```
ADMIN / Account Management / Account Settings
e.g. https://us06web.zoom.us/account/setting

[v] Allow cloud recording sharing - By disabling this setting, nobody else except the host can access the shareable link.

[v] Cloud recording downloads - Allow anyone with a link to the cloud recording to download
[ ] └── Only the host can download cloud recordings

[ ] Require users to authenticate before viewing cloud recordings

[ ] Set recording as on-demand by default - Users must register before they can watch the recording

[v] Require passcode to access shared cloud recordings
[v] └── Embed passcode in the shareable link for one-click access
```

## Prerequisites

[Node.js] and [npm] are required. The latest Node.js LTS installer can be downloaded [here](https://nodejs.org/en/download/).

- [npm] should be installed. (Included in the Node.js installer)
- `Add to PATH` option should be checked. (Windows only)

## Instructions

1. Create a `urls.txt` file and add URLs to the text file, one in each line.
2. Open a terminal in the directory where the `urls.txt` file is located.[^open-terminal]
3. Execute the `npx zoom-rec-dl@latest` command[^pnpm] in the terminal.

[^open-terminal]: When `ls` or `dir` command is executed, the `urls.txt` file should be listed.
[^pnpm]: pnpm users can use the `pnpm dlx zoom-rec-dl@latest` command instead.

```
Need to install the following packages:
  zoom-rec-dl@x.x.x
Ok to proceed? (y) ← Press enter
```

## Advanced

### Get Notification on Completion

Create a `sendgrid.json` file alongside `urls.txt` to send an email when the download is completed.

```json
{
  "API_KEY": "SendGrid-API-Key",
  "SENDER": "sender@domain.com",
  "RECEIVERS": ["receiver@domain.com"]
}
```

### Periodically Download Recordings

Use the following command to download recordings regularly. [pm2] should be installed globally.

[pm2]: https://pm2.keymetrics.io/docs/usage/quick-start/

```shell
# Example command
% pm2 start "npx zoom-rec-dl@latest" --name "Zoom Download" --time --no-autorestart --cron "0 0 * * *"

# Runs every midnight based on the system time.
# pnpm users can replace `npx` with `pnpm dlx`.
```

```shell
# CLI Options

# Specify an app name
--name <app_name>

# Prefix logs with time
--time

# Do not auto restart app
--no-autorestart

# Specify cron for forced restart
--cron <cron_pattern>
```
