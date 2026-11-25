---
name: edge-browser
description: Use Microsoft Edge, not Chrome, as the browser on this machine. Use whenever a script, tool or config picks a browser (Playwright, Puppeteer, Lighthouse, Selenium, launch configs, docs telling the user what to open), and whenever a task needs to drive a browser or reach a signed-in site.
---

# Microsoft Edge as the browser

Edge is the default browser here. Chrome is a fallback only when Edge cannot do the job.

## Picking a browser in code and config

Whenever something chooses a browser, choose Edge:

| Tool | Setting |
|------|---------|
| Playwright | `channel: 'msedge'` (Python: `browser_type.launch(channel="msedge")`) |
| Puppeteer | `executablePath` pointing at `msedge.exe`, or `channel: 'msedge'` with `puppeteer-core` |
| Lighthouse / chrome-launcher | `CHROME_PATH` set to the Edge executable |
| Selenium | `webdriver.Edge()` / `EdgeOptions` |
| VS Code launch.json | `"type": "msedge"` |
| Docs and instructions to the user | say "open in Edge" |

Default Edge path: `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`.

Fall back to Chrome only when Edge cannot do it (a tool with no Edge support, a Chrome-only API), and say so.

## Driving the browser

Use the running Edge that has the Claude extension (its tools are named `mcp__claude-in-chrome__*`; the extension works in Edge despite the name), not a fresh browser an automation library launches. If those tools are not present in the session, tell the user the extension is not connected instead of switching to another browser.

- That Edge already holds the user's signed-in sessions. A fresh Playwright or Puppeteer profile starts signed out and forces a login the user does not want to repeat.
- Do not use Claude's built-in browser (the `mcp__Claude_Browser__*` tools) unless no alternative exists. This device is managed by Microsoft Intune: Conditional Access policies restrict most Microsoft 365 content to registered, compliant browsers, and many applications authenticate through Entra ID single sign-on. The built-in browser is neither registered nor signed in, so it cannot access these applications or network resources.
- So a task that needs a signed-in site is a step Claude does through the extension, not something a script does on its own.
- Headless automation libraries are fine for public pages, local dev servers and test suites that need no real sign-in; still launch Edge (`msedge` channel) there.

## Pin the session to the Edge on this PC

The extension is signed in to the same Claude account on several PCs. Every one of those browsers stays connected, and until a session picks one, a new tab can open on whichever PC answers first, not the one running this session. So before the first browser action in a session (opening a tab, navigating, reading tabs):

1. Call `mcp__claude-in-chrome__list_connected_browsers`.
2. If exactly one browser is listed, or the one with `inUse` already has `onThisComputer: true`, carry on.
3. Otherwise select the browser with `onThisComputer: true` via `mcp__claude-in-chrome__select_browser` and its `deviceId`. `isLocal` only means the OS matches; on a fleet of Windows PCs it is true for all of them, so never pick by `isLocal` alone. If a browser's display name contains this computer's name (`$env:COMPUTERNAME`), that is a match too.
4. If no browser is marked `onThisComputer` and no name matches, do not guess. Show the user the list and ask which one is on this PC, or call `mcp__claude-in-chrome__switch_browser` so they can click **Connect** in the Edge in front of them.
5. Call `list_connected_browsers` again and confirm the selected one is `inUse` before opening tabs.

The choice lasts for the session only, so repeat this in every new session. If these tools are missing (older extension), tell the user that tabs may open on another PC and ask them to confirm the right Edge is in use.
