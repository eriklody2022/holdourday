# Hold Our Day

**One date. Every vendor.** A Northern Colorado wedding planner: couples hold a venue date, then find caterers, photographers, florists, DJs and bakers free that same day, and request meetings with each.

- Hosting: GitHub Pages (static files in this repo)
- Data and sign-in: Firebase (Firestore + Google sign-in), free tier

## Files

| File | What it is |
|---|---|
| `index.html` | The whole site: layout, venue and vendor list, and app logic |
| `firebase-config.js` | Your Firebase project settings (you paste these in) |
| `firestore.rules` | Database security rules (you paste these into Firebase) |
| `CNAME` | Tells GitHub Pages to serve the site at holdourday.com |
| `.nojekyll` | Tells GitHub Pages to serve files as-is |

## 1. Set up Firebase (about 10 minutes)

1. Go to https://console.firebase.google.com and click **Create a project**. Name it `hold-our-day`. Google Analytics is optional.
2. **Add a web app:** on the project overview, click the `</>` (Web) icon, name it `Hold Our Day`, and skip Firebase Hosting. Copy the `firebaseConfig` values it shows into `firebase-config.js`.
3. **Turn on sign-in:** Build → **Authentication** → Get started → **Sign-in method** → enable **Google**.
4. **Allow your domains:** Authentication → **Settings** → **Authorized domains** → add `holdourday.com`, `www.holdourday.com` and `YOUR-GITHUB-USERNAME.github.io`.
5. **Create the database:** Build → **Firestore Database** → Create database → choose a US location → start in **production mode**.
6. **Paste the rules:** Firestore → **Rules** tab → replace everything with the contents of `firestore.rules` → **Publish**.

## 2. Put it on GitHub Pages

1. Create a new repository, for example `holdourday`, and upload all the files in this folder (including `CNAME` and `.nojekyll`).
2. Repo **Settings** → **Pages** → Source: **Deploy from a branch** → Branch: `main`, folder `/ (root)` → Save.
3. Under **Custom domain**, confirm it shows `holdourday.com`. Tick **Enforce HTTPS** once it's available (can take up to an hour after DNS is set).

## 3. Point holdourday.com at GitHub (Namecheap)

Namecheap → Domain List → holdourday.com → **Manage** → **Advanced DNS**. Delete the default parking records, then add:

| Type | Host | Value |
|---|---|---|
| A Record | @ | 185.199.108.153 |
| A Record | @ | 185.199.109.153 |
| A Record | @ | 185.199.110.153 |
| A Record | @ | 185.199.111.153 |
| CNAME Record | www | YOUR-GITHUB-USERNAME.github.io. |

DNS usually takes effect within an hour, sometimes up to 24.

## 4. Make yourself the vendor desk

1. Open the live site, click **Sign in with Google**.
2. At the bottom of the page you'll see **Your account ID**. Copy it.
3. Firebase → Firestore → **Start collection** → Collection ID `admins` → Document ID: paste your account ID → add any field (e.g. `name` = `Erik`) → Save.
4. Reload the site. The **Vendor desk** tab appears.

Add a vendor-desk login for anyone else the same way.

## How data is stored

- `calendar/{vendorId}`: booked and blocked dates. Public to read; only the vendor desk can write.
- `pending/{userId}`: each couple's active holds, dates only. Public to read, so everyone sees "tentative hold".
- `requests/{userId}`: each couple's holds with names, email, phone and meeting times. Only that couple and the vendor desk can read.
- `admins/{userId}`: who can use the vendor desk.

## Notes

- Venue and vendor details come from public wedding guides and may be out of date. Edit the `VENUES` and `VENDORS` lists near the top of the script in `index.html`.
- In this version the vendor desk sees every hold. Giving each vendor a login that shows only their own holds is a later step.
