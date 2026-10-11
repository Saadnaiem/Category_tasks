# Team Workspace

This is a static React app loaded from `index.html`. It connects directly from the
browser to the existing Firebase project configured there (Firebase Authentication
and Cloud Firestore); Render hosts the app and does not replace or proxy the database.

## Deploy to Render

1. Push this folder to a GitHub or GitLab repository. Keep `render.yaml` and the
   app files at the repository root.
2. In Render, choose **New + → Blueprint**, connect the repository, and apply the
   `team-workspace` static-site service from `render.yaml`.
3. Wait for the first deploy, then copy the assigned `*.onrender.com` URL.
4. In Firebase Console, open **Authentication → Settings → Authorized domains**
   and add the Render hostname only (for example, `team-workspace.onrender.com`,
   without `https://` or a path). Add any custom domain there as well.
5. Open the deployed URL and test Google sign-in, team/private task reads and
   writes, comments, attachments, and presence with an authorized account.

No build command, server, Firebase Admin key, or database migration is needed.
The app's Firebase web configuration is in `index.html`; Firebase web API keys are
client identifiers, not service-account credentials. Never put a service-account
key in this static site.

## Firebase access requirements

- Enable Google as a sign-in provider in Firebase Authentication.
- The app has a client-side email allow-list. Enforce authorization in Firestore
  Security Rules too; client-side checks alone are not a security boundary.
- Verify the existing Firestore rules permit only the intended authenticated
  users to read/write `team_todos`, their own `private_todos`, and the app's
  `user_presence` and attachment chunk documents. Do not deploy with open rules.
- If sign-in is rejected, check the Firebase authorized domains and Google
  provider settings. If data access is denied, inspect the Firestore rules and
  browser console; Render cannot grant Firebase access.

The installable web app shell is cached by the service worker. Authentication,
real-time collaboration, and Firestore operations require an internet connection.
