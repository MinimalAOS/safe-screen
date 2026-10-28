# Safe Screen

**A safer space to grow.** A standalone, installable web-app prototype for a family-focused phone interface. Safe Screen is its own project and does not modify iOS.

## What's in this prototype

- Guided first-run setup with light, dark, or device-matched appearance.
- Separate Safe Screen unlock PIN and parental access code, each validated at 4–10 digits. They must differ during setup and when changed.
- PIN hashes with per-code random salts in browser storage (SHA-256 where Web Crypto is available). This is a useful prototype measure, not a tamper-proof authentication system.
- Responsive interface with smooth motion, reduced-motion support, safe-area padding, and bottom navigation.
- Child-facing app launcher with curated browser, learning cards, an original short story, calculator, local journal, and Family Hub.
- Parent settings for launcher visibility, in-app time limit, bedtime pause, browsing preferences, approved starting points, appearance, PIN changes, and local reset.
- A sandboxed website preview for approved starting points.
- Offline app-shell cache via a service worker when hosted over HTTPS.
- No analytics, advertising SDK, remote account, or server-side data collection.

## Important safety and iPhone limitation

This is a **front-end prototype, not a production parental-control product**. A website or Home Screen web app runs inside iOS's browser environment. It cannot replace the iPhone Home Screen, prevent a child from opening native apps or Settings, revoke system permissions, enforce an iPhone-wide lock code, count usage in other apps, prevent the user from leaving the web app, or independently enforce a network-wide website blocklist.

The daily counter measures time while Safe Screen is visible and active. It does not measure whole-device usage, and a web app can be bypassed by closing it, clearing browser data, changing device settings, or navigating around its UI. Browser storage is editable by the device owner. Journal text and preferences are local and are not encrypted as a secure vault. Do not rely on this prototype to keep a child safe from unrestricted internet access.

The Safe Browser only restricts its **starting-point list** and loads third-party content inside a restricted iframe sandbox. Some sites will fail to load, and this is not a reliable content filter: navigation, redirects, embedded resources, and content on a site may not be reviewed or blocked. The `strictSearch` option is a saved product preference for future work; it does not operate a network-level strict search service.

## Run it locally

1. Download and unzip the project.
2. Open `index.html` in a modern browser to try the interface. The main UI works locally; service workers require HTTPS or localhost.
3. For a local HTTP server, run this from the project folder if Python is installed:

   ```bash
   python3 -m http.server 8000
   ```

   Then visit `http://localhost:8000`.

4. For a more representative installable experience, deploy to GitHub Pages (HTTPS) and add the site to the iPhone Home Screen from Safari. The exact iOS share/menu wording can vary by version.

## Publish to GitHub Pages

1. Create a new public or private repository named `safe-screen`.
2. Upload the contents of this folder to the repository root, keeping `index.html`, `manifest.webmanifest`, `icon.svg`, and `sw.js` together.
3. In **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`, then save.
4. Open the published HTTPS URL on iPhone in Safari and choose **Add to Home Screen** from the Share menu to launch the web app in its standalone presentation when supported.

No build step, framework, package install, API key, database, or paid domain is required. A `github.io` address is enough.

## Reset the demo

Open Parents → Device → Reset Safe Screen. The action removes this app's saved setup and local demo data in the current browser. It does not reset the iPhone or other browsers.

## Security design notes

- PIN values are not saved directly; salted hashes are stored in `localStorage`.
- This is still client-side code on a device controlled by the user. A determined user can inspect or modify the JavaScript and local data. A parent code in a static website is a UI gate, not a secure system boundary.
- There is no cross-device management in this version. Settings do not sync to another phone, and no remote control is available.
- The approved-site list is a curated directory, not a web-filtering proxy or DNS filter.
- Avoid entering sensitive personal data. The journal is a simple local demo.

## Path to a real iPhone parental-control product

To enforce rules beyond the Safe Screen interface, build and sign a native iOS app in Swift/SwiftUI and integrate Apple's Screen Time technology. Apple documents the **FamilyControls**, **ManagedSettings**, and **DeviceActivity** frameworks for guardian-authorized app/category/web-domain restrictions and usage schedules. Distribution requires the relevant Apple capability/entitlement approvals, appropriate family authorization and app extensions. A robust browser/content-filtering design may also need a controlled browsing architecture, threat-model review, carefully tested URL policy, and independent security/privacy review.

Start with Apple's official references:

- [Screen Time Technology Frameworks](https://developer.apple.com/documentation/screentimeapidocumentation)
- [Configuring Family Controls](https://developer.apple.com/documentation/Xcode/configuring-family-controls)
- [Manage settings on devices in a Family Sharing group](https://developer.apple.com/documentation/managedsettings/connectionwithframeworks)

A native implementation has to be designed, implemented, permissioned, and tested separately; this repository does not contain a native iOS enforcement layer.

## Project structure

```text
safe-screen/
├── index.html             # UI, styles, setup flow, and local demo behaviour
├── manifest.webmanifest   # Installable web-app metadata
├── icon.svg               # Safe Screen app icon
├── sw.js                  # Offline app-shell cache
├── README.md              # Setup, deployment, and safety limitations
└── LICENSE                # MIT license
```

## License

MIT. See [`LICENSE`](LICENSE).
