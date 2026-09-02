# simple-totp

[Русская версия](README.ru.md)

A two-factor authentication code generator in a single HTML file. Open it in a browser and it shows the same 6-digit codes an authenticator app would — for GitHub, Google, Yandex, or any other site that offers "scan this QR code with an authenticator app".

No installation, no account, no internet connection, no phone.

## Why it exists

Many sites now force two-factor authentication and only suggest installing a mobile or desktop authenticator app. If you don't want another app — or you are on an old machine where those apps no longer install — this file does exactly the same job. It is one file of plain HTML and JavaScript, about 11 KB, that you can read yourself before trusting it.

## What it does

- Generates standard TOTP codes (RFC 6238: HMAC-SHA1, 6 digits, 30-second period), accepted by every site that supports authenticator apps.
- Stores several accounts: type `github` in the search box and the matching key lights up, so you never have to guess which code is which.
- Works fully offline. Nothing is sent anywhere; there is no network code in the file at all.
- Runs on old browsers, including Internet Explorer 9+ on Windows 7, as well as any current Firefox, Chrome, or Edge.
- Backup and restore: one button prints all your keys as plain text so you can copy them into a file; pasting that text back on another computer restores everything.

## How to use it

1. Download **totp.html** ([direct link](https://raw.githubusercontent.com/ed1385/simple-totp/main/totp.html) — right-click, Save link as) and open it by double-clicking. It works from a local disk or a USB stick.
2. On the site where you are enabling two-factor authentication, look under the QR code for a link such as "setup key", "can't scan?" or "enter this text code". It gives you a string of letters A–Z and digits 2–7.
3. In totp.html, fill in the site name (for example github.com) and paste that key, then press **Save in browser**.
4. A 6-digit code appears with a countdown bar. Type it into the site to finish enabling two-factor authentication.
5. From then on, whenever a site asks for a code, open totp.html, start typing the site name, and read the highlighted code.

**Save your recovery codes.** Every site gives you a list of one-time recovery codes when you enable 2FA. Keep them, and keep a copy of your setup keys too (the **Show keys** button). If you lose both the keys and the recovery codes, nobody can get you back into the account.

## Where the keys are stored

In the browser's own storage (localStorage), on that computer, for that browser, tied to the location of the file. Nothing is written next to totp.html and nothing is stored inside the file itself — the file is identical for everyone.

That means the keys will not be there if you open the file in a different browser, on a different computer, or after clearing browsing data. Use **Show keys** to make a text backup, and **Load from text** to restore it elsewhere.

## Security

Your keys stay on your machine, which is the point — but it also means the machine is what protects them. Anyone with access to that browser profile can read them, so use it on a computer you control, and keep your text backup somewhere separate from the computer.

The system clock must be roughly accurate (within about 30 seconds), because the code is derived from the current time. If a site rejects a freshly generated code, check the clock first.

## How it works

A TOTP code is `HMAC-SHA1(secret_key, current_time / 30 seconds)`, truncated to 6 digits. The site and your generator hold the same secret and read the same clock, so they arrive at the same number independently — no messages are exchanged, which is why the file needs no network access. SHA-1, HMAC and Base32 decoding are implemented in the file in plain JavaScript, with no external libraries. Correctness is verified against the RFC 6238 reference test vectors.

## License

MIT — see [LICENSE](LICENSE).
