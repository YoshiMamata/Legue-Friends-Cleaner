# League Friends Manager V2 — Final

**Cleaned by YoshiMamata#MUE**

A standalone Windows application for organizing your League of Legends friend list. It starts from CMD and opens a clean, modern browser interface. **No Python, Go, installation, additional account, or API key required.**

## 1. Quick start

1. Extract the complete ZIP to a normal folder, such as your Desktop.
2. Start **League of Legends**, sign in, and keep the client open.
3. Double-click `Launch.cmd` or `LeagueFriendsManager.exe`. Alternatively run `LeagueFriendsManager.exe` from CMD.
4. A CMD window opens, followed by the app in your default browser.
5. **First required step:** locate the League Client lockfile:
   - Click **Auto-detect** to search common installation locations and the running League process;
   - Or click **Browse** to select it with the Windows file dialog;
   - Or enter the full path, e.g. `C:\Riot Games\League of Legends\lockfile`.
6. Click **Connect & load friends**. The dashboard appears only after a valid connection.
7. Switch interface language using **RO / EN** in the upper right. Your preference is saved.

**Important:** Keep the CMD window open while you use the app. The “Exit app” button stops the local server. This build targets **64-bit Windows 10/11** and needs a modern browser (Edge, Chrome, or Firefox).

## 2. Features

- Read your friends from **League Client** via its local API (LCU).
- Search by **Riot ID / name, friend note, group**.
- Filter by **status**: All / Online / Offline / Unknown.
- Filter by **activity**: Any / Inactive for at least 7, 30, 90 or 180 days / Unknown date.
- Filter by **friend group**.
- Sort by **name A–Z, name Z–A, recent to old, old to recent, group A–Z**.
- Individual checkboxes, Select filtered, Clear selection. Selection persists between filters; check the total selected count before deleting.
- **Protect / Remove protection** for selected friends. Protected contacts have a star, are saved locally and can never enter bulk deletion.
- Export the **currently filtered** friends as CSV (saved to your backup folder and downloaded by the browser).
- **Bulk delete** with explicit confirmation, full-list CSV backup, lockfile/account/identity rechecks, and a pause between deletes.
- **Stop remaining** requests while a batch is running. Already-completed deletions cannot be undone.
- **Change lockfile** to reconnect when League has restarted.
- After a successful cleanup the UI displays **Cleaned by YoshiMamata#MUE**.

## 3. What “activity” means

The friend list comes from League Client. If available, the app also reads Riot Client's local `/chat/v4/friends` endpoint and uses the `last_online_ts` for matching friend PUUIDs.

**This is last Riot online activity, NOT the time of the player's last League match.** A missing timestamp is **Unknown**, never assumed inactive. Currently connected friends count as 0 inactive days. Filters for Inactive ≥ N days **exclude unknown dates**.

Riot may withhold this data or change the endpoint. If so, alphabetic, status, group, selection and deletion features remain usable; missing activity is displayed as unknown.

## 4. Safe deletion and backups

1. Select the contacts. Any protected friends are automatically excluded.
2. Read the confirmation list and type precisely `DELETE N` in English or `STERGE N` in Romanian, where **N** is the number of eligible contacts.
3. Before any deletion, the app rechecks the lockfile, account (where identifiable), and fresh friend list, then creates a CSV backup of **all loaded contacts**, not just selected ones.
4. Each friend is verified and removed individually, with short pauses. You may stop the remaining requests.

**A CSV backup is not an Undo button.** It helps you identify contacts you removed but does not automatically re-add them. Review your selection carefully.

Local files:

- Backups: `%LOCALAPPDATA%\LeagueFriendsManager\backups`
- Saved language and protected friends: `%LOCALAPPDATA%\LeagueFriendsManager\settings.json`

The lockfile password is **kept only in process memory** and only used for HTTPS calls to `127.0.0.1`. It is never written to backups, settings, or logs. The browser interface is bound to `127.0.0.1` and uses a random session token for local API access. Never share a lockfile's contents.

## 5. Demo mode

Run `Launch_Demo.cmd` to explore the same dashboard with **fictional contacts**. Filtering, protection, and language switching work; **deletion is disabled in demo mode**. No League client is required.

## 6. Troubleshooting

**Cannot connect:** Ensure League is open and the chosen lockfile is **inside the League of Legends folder**, rather than the separate Riot Client lockfile.

**Auto-detect finds nothing:** Use Browse or type the complete path. Your install may be in a custom directory.

**Unknown activity:** The Riot Client local endpoint may not be available or may not provide the timestamp. This does not mean everyone is inactive.

**Connection lost after League restart:** Click Change lockfile and reconnect with the new lockfile.

**Browser doesn't open:** Copy the printed URL (`http://127.0.0.1:xxxxx`) from CMD into your browser.

**Windows warns about the EXE:** It is a standalone executable that is **not digitally signed**. Source code is included under `source` for review and independent compilation. Do not globally disable Windows security features.

## 7. Source code and tests

The `source/` directory contains Go 1.23 code, HTML/CSS/JavaScript frontend, and tests. To rebuild (optional, developers only):

```cmd
cd source
go test ./...
go build -o ..\LeagueFriendsManager.exe .
```

You do **not** need Go or Python to use the supplied executable.

## 8. Compatibility and Riot notice

The Windows x64 EXE was built successfully and tested against a simulated local HTTPS League client, including backup and delete scenarios. **It has not been live-tested with a real Riot account in this environment** and compatibility with future League releases is not guaranteed.

League Client API (LCU) is **not officially supported** for third-party applications. Riot makes no guarantees about endpoint stability or documentation. If you publicly distribute this project, review Riot's Developer policies and app registration requirements.

> League Friends Manager isn't endorsed by Riot Games and doesn't reflect the views or opinions of Riot Games or anyone officially involved in producing or managing Riot Games properties. Riot Games, and all associated properties are trademarks or registered trademarks of Riot Games, Inc.
