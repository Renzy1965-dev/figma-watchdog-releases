# Figma Watchdog

A small pixel dog lives along the bottom of your screen. When someone
**requests access to a Figma file** — or comments on one — it **barks** and
shows you what happened. Click the bubble to jump straight to it.

It exists because Figma only puts access requests in its in-app bell. If you
are not looking at Figma, you never see them.

---

## Install

### 1. Download

Go to **[the latest release](../../releases/latest)** and download:

```
Figma Watchdog Setup <version>.exe
```

### 2. Run it, and get past the Windows warning

Windows shows a blue **"Windows protected your PC"** screen.

> Click **More info**, then **Run anyway**.

This is expected. Windows shows that for any application that has not been
code signed, and a signing certificate costs a few hundred dollars a year. It
does **not** mean anything is wrong with the app. If you would rather check
first, ask Ren before continuing.

### 3. Connect your mailbox

The dog appears at the bottom of your screen, and a dog icon appears in your
system tray (bottom-right, near the clock).

1. **Right-click the tray icon** → **Connect Outlook (read-only)…**
2. Your browser opens Microsoft's own sign-in page.
3. **Sign in as the mailbox that receives Figma's emails** — for shared files
   that is usually `support@shore360agency.com`, not your personal address.

The tray then shows which mailbox it is watching, so you can confirm you picked
the right one.

### 4. Check it works

**Right-click the tray icon → Test the bark.**

If you see a speech bubble and hear a bark, you are done. Nothing else to set
up — no tokens, no configuration files.

---

## Updates

Automatic. The app checks once a day, downloads quietly in the background, and
applies the update the next time you quit. It will never restart itself while
you are working. You never need to download from this page again.

## Uninstall

**Settings → Apps → Installed apps → Figma Watchdog.**

---

## What it can see — and what it can't

This matters, so it is spelled out plainly.

**It cannot read your emails.** The only mail permission it requests is
`Mail.ReadBasic`, which Microsoft defines as *"read mail in user mailboxes
except for body, bodyPreview, uniqueBody, and attachments."*

**Microsoft enforces that — not the app.** The sign-in never grants the right
to fetch the text of an email, so this app physically cannot read message
contents. In a shared mailbox, your colleagues' emails are unreadable at the
API level, not merely unread by policy.

On top of that:

- It only ever asks for mail **from `figma.com`**. Anything from another sender
  is discarded before it is even examined.
- It reads four things per message: sender, subject, when it arrived, and a
  link to open it.
- It **cannot** send, reply, forward, move, flag, archive or delete anything.
- It stores message **ids** only — never any email content.
- Nothing is uploaded anywhere. Everything stays on your machine.

Revoke access any time at <https://myaccount.microsoft.com/> under
**Privacy → Apps and services**, or via tray → **Disconnect mailbox**.

---

## Using it

- The dog wanders the bottom of your screens, sits, and sleeps when quiet.
- A new access request makes it **bark**, with an orange badge showing unread
  count.
- **Click the bubble** to open the email — Figma's Approve/Deny buttons are in
  there. **Click the dog** for the next unread one.
- A bubble you ignore hides after 30 seconds but is **not** discarded — it stays
  on the badge until you look at it.
- Everything else is click-through. The dog never blocks what is beneath it.

**Tray menu** (right-click the dog icon near the clock):

| Item | What it does |
| --- | --- |
| Test the bark | Fires a test alert, to check it works |
| Mute bark | Keeps the bubble, silences the sound |
| Pause watching | Stops checking entirely |
| Show the dog | Hide it during a screen share or presentation |
| Start with Windows | Have it there every morning |
| Disconnect mailbox | Revokes access, deletes the local token |
| Quit | Closes it |

---

## Something wrong?

| Problem | Fix |
| --- | --- |
| No dog on screen | Check the tray icon exists, then tray → **Show the dog** |
| Never barks | Tray → **Test the bark** first. If that works, the app is fine — search Outlook for `figma`; if there are no Figma emails at all, that account's Figma email notifications may be switched off (Figma → Settings → Notifications) |
| Watching the wrong mailbox | The tray shows which one. **Disconnect mailbox**, connect again, pick the right account |
| Too many barks | Tray → **Open settings file**, set `MAIL_NOTIFY=access` for access requests only |

---

## Why this repository is public

The application source lives in a **private** repository. Only compiled
installers are published here.

`electron-updater` cannot read a private repository's releases without a GitHub
token carrying `repo` scope on every user's machine — and that token would grant
read/write access to *all* of the owner's private repositories. Publishing the
binaries separately removes the need for any credential at all.

**Nothing sensitive is in a build.** The installer contains an Azure *client id*
(public by design for desktop apps — PKCE and the registered redirect URI are
what secure that flow) and a tenant restricted to `shore360agency.com`, so
sign-in is limited to Shore360 accounts. Someone outside the organisation who
downloads this cannot sign in to anything. There is no Figma token, no mail
token, and no user data in any build — those exist only on each user's own
machine.
