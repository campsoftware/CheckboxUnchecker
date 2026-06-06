# CheckboxUnchecker

One-liner to untick every checkbox on a webpage. Perfect for PACER, government portals, or any long form where you want to start fresh.

---

## Pick your path

| Platform    | Method                                          | Best for                                        |
| ----------- | ----------------------------------------------- | ----------------------------------------------- |
| **Mac**     | [AppleScript (Safari)](#mac-applescript-safari) | Automating / Shortcuts / Script Editor          |
| **Any**     | [Bookmarklet](#universal-bookmarklet)           | Reusable one-click button in your bookmarks bar |
| **Mac**     | [Browser Console](#mac-browser-console)         | Any Mac browser, one-off use                    |
| **Windows** | [Browser Console](#windows-browser-console)     | Any Windows browser, one-off use                |

---

## Mac: AppleScript (Safari)

Open **Script Editor** (`⌘ + Space` → type "Script Editor" → Enter), then paste this in (or save as an `.scpt` and trigger it from the Shortcuts app):

```applescript
tell application "Safari"
    activate
    tell current tab of window 1
        do JavaScript "document.querySelectorAll('input[type=\"checkbox\"]').forEach(cb => cb.checked = false);"
    end tell
end tell
```

If you use **Chrome** or **Brave** instead of Safari:

```applescript
tell application "Google Chrome"
    activate
    execute front window's active tab javascript "document.querySelectorAll('input[type=\"checkbox\"]').forEach(cb => cb.checked = false);"
end tell
```

---

## Universal: Bookmarklet

Add a one-click button to your bookmarks bar that works on **any** site.

1. Create a new bookmark (any URL — e.g., `google.com`).
2. Edit it and replace the URL with this:

```javascript
javascript:(function(){document.querySelectorAll('input[type="checkbox"]').forEach(cb=>cb.checked=false);})();
```

3. Name it **Uncheck All**.
4. Click it whenever you’re on a page with annoying pre-ticked boxes.

---

## Mac: Browser Console

1. Press `Cmd + Option + J` (Chrome/Brave/Edge) or `Cmd + Option + K` (Firefox).  
   Safari: `Cmd + Option + C` — enable the Develop menu in **Safari Settings → Advanced** first.
2. Paste this and hit **Return**:

```javascript
document.querySelectorAll('input[type="checkbox"]').forEach(cb => cb.checked = false);
```

---

## Windows: Browser Console

1. Press `F12` (or `Ctrl + Shift + J` in Chrome/Edge/Brave, `Ctrl + Shift + K` in Firefox).
2. Click the **Console** tab.
3. Paste this and press **Enter**:

```javascript
document.querySelectorAll('input[type="checkbox"]').forEach(cb => cb.checked = false);
```

---

## Sticky sites? (React, Vue, etc.)

If the checkboxes look unchecked but the site doesn’t “notice,” the page is probably waiting for a `change` event. Use this stronger version:

```javascript
document.querySelectorAll('input[type="checkbox"]').forEach(cb => {
    cb.checked = false;
    cb.dispatchEvent(new Event('change', { bubbles: true }));
});
```

---

## Narrow it down

Only want to untick checkboxes inside a specific table (e.g., skip the header row)?

```javascript
// Example: only rows in a table with class "attachments"
document.querySelectorAll('table.attachments input[type="checkbox"]').forEach(cb => {
    cb.checked = false;
});
```

Right-click a checkbox → **Inspect** to find the table’s class or ID, then swap `table.attachments` for the right selector.

---

## What this won’t do

- Custom “fake” checkboxes made from `<div>` or `<span>` tags won’t respond.
- Government/legacy sites like **PACER** use real HTML checkboxes, so this works perfectly there.
