# Figma Watchdog — releases

Built installers for **Figma Watchdog**, an internal Shore360 tool.

Grab the latest `Figma Watchdog Setup *.exe` from
[Releases](../../releases/latest) and run it.

Setup instructions: ask Ren, or see `READ ME FIRST.md` inside the install.

---

### Why this repository is public

The application source lives in a **private** repository. Only the compiled
installers are published here.

This exists because `electron-updater` cannot read a private repository's
releases without a GitHub token with `repo` scope on every user's machine —
and that token would grant read/write access to *all* of the owner's private
repositories. Publishing the binaries separately removes the need for any
credential at all.

### What is (and is not) in these builds

The installer contains an Azure **client id**, which is public by design for a
desktop app — PKCE and the registered redirect URI are what secure that flow —
and a **tenant restricted to shore360agency.com**, so sign-in is limited to
Shore360 accounts. Someone outside the organisation who downloads this cannot
sign in to anything.

There is **no** Figma token, **no** mail token, and **no** user data in any
build. Those live only on each user's own machine.

### What the app can access

Read-only, and narrowly:

- **Figma**: comments only, via read-only scopes. It cannot create, edit,
  resolve or delete anything.
- **Outlook**: `Mail.ReadBasic` only — Microsoft itself blocks it from reading
  message bodies — and a hard sender allowlist restricts it to mail from
  `figma.com`. It cannot send, move, or delete mail.
