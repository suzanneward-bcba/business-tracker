# Business Setup & Operations Tracker — Setup

One file (`index.html`) plus two security-rule files. No build step.

## Try it right now (no setup)
Open `index.html` in a browser. While the Firebase config is empty, it runs in **local demo mode**: tasks are saved in that browser only. This is good for testing, but not for real use.

## Go live with Firebase (about 15 minutes)

### 1. Create the Firebase project
1. Go to https://console.firebase.google.com → **Add project** (e.g. `business-tracker`). Google Analytics is not needed.
2. **Build → Authentication → Get started → Sign-in method → Google → Enable.**
3. **Build → Firestore Database → Create database.** Choose location **europe-west3 (Frankfurt)** and **production mode**.
4. *(Phase 2 only)* **Build → Storage → Get started**, same region. Not needed yet.

### 2. Lock it down
1. Open `firestore.rules` and check the allowed email list (currently `suzanneward@adaptability.co`). That must be the Google account you sign in with. Add a second person's email later to give them access.
2. Firestore → **Rules** tab → paste the full contents of `firestore.rules` → **Publish**.
3. *(When Storage is enabled)* Storage → **Rules** → paste `storage.rules` → **Publish**.

With these rules, nobody else can read or write anything, even if they find the URL.

### 3. Connect the app
1. Project settings (⚙) → **Your apps → Web (</>)** → register the app (no Hosting needed).
2. Copy the `firebaseConfig` values into the `firebaseConfig` block near the top of the `<script>` in `index.html`.
   These values are not secrets. The security rules above are what protect the data.
3. Open the app and click **Sign in with Google**. The first sign-in creates the workspace and loads the example tasks.

### 4. Deploy (choose one)

**GitHub Pages**
1. Create a **private or public** repo (e.g. `suzanneward-bcba/business-tracker`) and add `index.html` (plus the rules files and this README if you like).
2. Repo → **Settings → Pages → Deploy from branch → main / root**.
3. Your app is at `https://suzanneward-bcba.github.io/business-tracker/`.
   Note: on free GitHub plans, Pages sites are publicly reachable even from private repos. That is fine here because the data is protected by login and the Firestore rules, but anyone with the link sees the sign-in screen.

**Netlify**
Drag the folder onto https://app.netlify.com/drop, or connect the GitHub repo for automatic deploys.

### 5. Authorize the domain
Firebase → **Authentication → Settings → Authorized domains → Add domain**:
`suzanneward-bcba.github.io` and/or `your-site.netlify.app`.
Without this step, Google sign-in shows an "unauthorized domain" error.

## How data is stored
- `workspaces/main` holds settings (companies, categories, email signature).
- `workspaces/main/tasks/{id}` holds one document per task: details, checklist, waiting/follow-up info, and history. It also has empty `contacts`, `documents` and `notes` lists, ready for Phase 2.
- Firestore keeps an offline cache, so changes made without a connection sync later.
- **Never** put bank PINs, passwords or online-banking credentials in tasks.

## Backups
Firestore → **Import/Export** can back up the database (requires the Blaze plan). CSV export inside the app is planned for Phase 2.

## Disclaimer
Checklists and example tasks are organizational aids, not legal or tax instructions. Verify requirements with the appropriate professional.
