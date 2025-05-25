

# 📄 CHANGES.md — Security Improvements for Login Page (Electron App)

## 🔐 Purpose:
This file lists the **required security enhancements** identified during a security audit of the Login Page project using `Electronegativity`.

---

## ✅ Required Changes

### 1. Add Content Security Policy (CSP)

**Why?**  
Prevents XSS attacks by controlling which resources the app can load.

**How to fix:**  
In your main HTML file (e.g., `index.html`), add this inside `<head>`:

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'; style-src 'self'">
````

---

### 2. Limit Navigation Events

**Why?**
Prevents malicious websites from being loaded in a new window or navigating away from the app.

**How to fix:**
Update your `main.js` or wherever you create the Electron `BrowserWindow`:

```javascript
mainWindow.webContents.on("will-navigate", (event, url) => {
  event.preventDefault(); // Prevent unwanted navigation
});

mainWindow.webContents.setWindowOpenHandler(({ url }) => {
  return { action: 'deny' }; // Deny opening new windows
});
```

---

### 3. Use `setPermissionRequestHandler` to Restrict Permissions

**Why?**
Stops untrusted content from requesting dangerous permissions like `openExternal`.

**How to fix:**
In your `main.js`, add:

```javascript
session.defaultSession.setPermissionRequestHandler((webContents, permission, callback) => {
  const allowedPermissions = []; // E.g., ['media', 'geolocation']
  if (allowedPermissions.includes(permission)) {
    callback(true);
  } else {
    callback(false);
  }
});
```

---

## 📦 Optional Improvements

### ✔️ Install ElectroNG for More Powerful Scans (Optional)

```bash
npm install -g electronng
```
