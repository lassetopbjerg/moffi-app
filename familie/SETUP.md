# Familietavlen – setup

The family board lives at **https://lassetopbjerg.github.io/moffi-app/familie/** and uses the same Firebase project as Møffi (`moffi-app`). Its data is stored under `/familie` in the Realtime Database; Møffi keeps using `/entries` and is not changed.

Only two Google accounts get in: `lassetopbjerg@gmail.com` and `idaholtpedersen@gmail.com`. That list is in two places, `ALLOWED` in `index.html` and `database.rules.json`, and they must match.

Nothing secret lives in this folder. The Firebase config and the Google client ID are public identifiers; access is controlled by sign-in and the database rules.

## 1. Firebase: turn on Google sign-in (console.firebase.google.com → moffi-app)

1. **Authentication → Get started** (if you haven't used it before) → **Sign-in method** → **Google** → **Enable**, choose your email as support email → **Save**.
2. On the same Google screen, open **Web SDK configuration** and copy the **Web client ID** (ends in `.apps.googleusercontent.com`). It goes into `googleClientId` at the top of the script in `index.html`.
3. **Authentication → Settings → Authorized domains → Add domain:** `lassetopbjerg.github.io`

## 2. Firebase: database rules

**Realtime Database → Rules.** Replace the rules with the contents of `database.rules.json` and press **Publish**.

These rules keep Møffi working exactly as today (`/entries` open), and lock `/familie` to the two accounts. If your current rules already restrict `entries` in some way, keep your `entries` block and only add the `familie` block.

## 3. Google Cloud: allow the board to use Google Calendar (console.cloud.google.com, project **moffi-app**)

1. **APIs & Services → Library →** search **Google Calendar API → Enable**.
2. **Google Auth Platform** (called *OAuth consent screen* in older menus):
   - **Branding:** app name e.g. *Familietavlen*, your email as support and developer contact.
   - **Audience:** *External*, keep it in **Testing**, and under **Test users** add both Gmail addresses.
   - **Data access → Add or remove scopes:** add `https://www.googleapis.com/auth/calendar.events` → Save.
3. **Credentials → OAuth 2.0 Client IDs →** *Web client (auto created by Google Service)* → **Authorized JavaScript origins → Add URI:** `https://lassetopbjerg.github.io` → **Save**. (It can take a few minutes to take effect.)

Because the app stays in *Testing*, Google shows a "Google hasn't verified this app" screen the first time. Tap **Advanced → Go to …** to continue. That's expected for a private family app.

## How it behaves

- **Sign-in** is with Google. Anyone else who opens the address sees a login screen and gets no data.
- **Calendar:** tap **Forbind kalender** once. Browsers only get calendar access for about an hour at a time, so after a while the board asks you to tap it again. A small server function in round two can make this permanent.
- **Install on the phone:** iPhone: open in Safari → Share → *Føj til hjemmeskærm*. Android: Chrome menu → *Installer app*.

## Round two (needs Firebase's pay-as-you-go plan and an Anthropic API key)

Claude features from the Claude version of the board: menu suggestions from the Netto/REMA offers, price estimates, sorting dictated text into shopping/calendar/tasks, and the weekly offers fetch. These need a Cloud Function holding the API key, so the key never appears in this public repo.
