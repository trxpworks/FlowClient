# Discord Release Announcements

Flow Client announces every published GitHub Release in two Discord channels:

- Announcements: a short release summary and download link.
- Changelog: the complete GitHub release notes, split across messages when necessary.

## 1. Create the Discord webhooks

Repeat these steps for the announcement and changelog channels:

1. Open the Flow Client Discord server.
2. Open **Server Settings > Integrations > Webhooks**.
3. Select **New Webhook**.
4. Name it `Flow Client Updates`.
5. Select the intended channel.
6. Select **Copy Webhook URL**.

Treat webhook URLs like passwords. Do not post or commit them.

## 2. Add GitHub Actions secrets

Open the `trxpworks/FlowClient` repository:

1. Open **Settings > Secrets and variables > Actions**.
2. Under **Secrets**, create:
   - `DISCORD_ANNOUNCEMENTS_WEBHOOK`
   - `DISCORD_CHANGELOG_WEBHOOK`
3. Paste the corresponding Discord webhook URL into each secret.

Optional update-role ping:

1. Enable Discord Developer Mode in **User Settings > Advanced**.
2. Right-click the update role and select **Copy Role ID**.
3. In GitHub Actions, open the **Variables** tab.
4. Create `DISCORD_UPDATE_ROLE_ID` with that numeric role ID.
5. Ensure the webhook can mention the role in the announcement channel.

The role is pinged only for real published releases, never manual tests.

## 3. Test safely

1. Open the repository's **Actions** tab.
2. Select **Announce release on Discord**.
3. Select **Run workflow**.
4. Leave `dry_run` enabled for the first run.
5. Inspect the workflow log to preview both Discord payloads.
6. Run it again with `dry_run` disabled to post a labelled test message.

## 4. Publish an update

Create a GitHub Release and write its full changelog in the release description.
When the release is published, GitHub automatically:

1. Posts a concise summary in the announcement channel.
2. Posts the full release notes in the changelog channel.
3. Adds direct links to up to four attached release files.
4. Pings the configured update role, when present.

Draft releases do not trigger announcements. Prereleases are labelled Preview.
