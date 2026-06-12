# Nivryn Discord Release Announcements

Nivryn announces every published GitHub Release in two Discord channels:

- Announcements: a short release summary and download link.
- Changelog: the complete GitHub release notes, split across messages when necessary.

## 1. Create the Discord webhooks

Repeat these steps for the announcement and changelog channels:

1. Open the Nivryn Discord server.
2. Open **Server Settings > Integrations > Webhooks**.
3. Select **New Webhook**.
4. Name it `Nivryn Updates`.
5. Select the intended channel.
6. Select **Copy Webhook URL**.

Treat webhook URLs like passwords. Do not post or commit them.

## 2. Add GitHub Actions secrets

Open the `trxpworks/NivrynClient` repository:

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

The role is pinged only for production announcements, never labelled tests.

## 3. Run it manually

1. Open the repository's **Actions** tab.
2. Select **Publish Nivryn release to Discord**.
3. Select **Run workflow**.
4. Leave `release_tag` as `latest`, or enter a release tag such as `v1.0.0`.
5. Leave `test_mode` disabled to post a normal production announcement.
6. Use `notes_override` when the selected release has no description or needs a corrected changelog.
7. Enable `dry_run` to preview the payload without posting it.

For a labelled test message, enable `test_mode`. The test version and notes
fields are ignored during normal production runs.

## 4. Publish an update

Create a GitHub Release and write its full changelog in the release description.
When the release is published, GitHub automatically:

1. Posts a concise summary in the announcement channel.
2. Posts the full release notes in the changelog channel.
3. Adds direct links to up to four attached release files.
4. Pings the configured update role, when present.

Draft releases do not trigger announcements. Prereleases are labelled Preview.
Manual production runs load their title, notes, channel, files, and URL from the
selected GitHub Release, so they match automatic announcements and do not show
the `TEST` label.
