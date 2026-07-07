# NODE 0448 Update — 1992 Build Installation

This update reframes the terminal as what it fictionally *is*: an unregistered node hiding inside DSNET-3 — the Top Secret segment of the Defense Data Network, the real 1992-era predecessor to SIPRNET — which A-Cell uses to pass traffic to cell leaders, and where field cells file After Action Reports. It presents as a legitimate DoD system (period-accurate monitoring-consent warning banner, 18 U.S.C. 1030 citation, VT220/9600-baud session line) because that's the camouflage: an illegal conspiracy squatting on the government's own infrastructure.

## What changed

The DELTA GREEN text masthead is replaced by a **shaded ASCII delta** in classic ANSI-art block characters (░▒▓█ — authentic CP437 BBS style), glowing green. Above it sits a **login banner**: government warning text, node ident, `SYSTEM BUILD 1992.07.05`, a last-login line from a redacted node, and a blinking cursor. Navigation is now a **numbered BBS menu** — `[1] BRIEFING  [2] AAR TRAFFIC  [3] PERSONNEL  [4] REMOTE UPLINK`. The briefing page is an A-Cell **MESSAGE OF THE DAY** with FROM/TO/RE headers, closing with the traditional summons: *"You have been invited to a night at the opera."* AAR posts render as **intercepted message traffic** — FROM: FIELD CELL / TO: A-CELL / SUBJ / HANDLING: DESTROY AFTER READING — and post navigation reads `<< PREV MSG / NEXT MSG >>`. The Foundry link is the **Remote Uplink**. CRT effects, audio hum, toggle, and redaction bars all carry over unchanged.

Nothing here references Impossible Landscapes content — all copy is generic Delta Green tradecraft, safe for player eyes.

## Files in this package

```
_layouts/default.html    (replace — banner, delta, menu, session block)
_layouts/post.html       (replace — message-traffic AAR header)
index.html               (replace — A-Cell MOTD briefing)
reports.html             (replace — traffic archive)
assets/css/dossier.css   (replace — restyle)
```

Deliberately NOT included: `_config.yml`, `roster.md`, your `_posts`, and the audio — your customizations and content are untouched. The new pages still read your operation/handler/cell/schedule values from `_config.yml`.

## Install

1. Unzip. On your repo's main page: **Add file → Upload files**.
2. Drag in the `_layouts` and `assets` folders **plus** the loose `index.html` and `reports.html`.
3. Commit. Same-named files are replaced; ~2 minutes later it's live. Hard-refresh (Ctrl+Shift+R / Cmd+Shift+R).

## Optional touches for full immersion

**Date your AARs in-world.** Jekyll post dates come from the filename, and past dates are fine — name a recap `1992-07-10-cold-harvest.md` and the archive will show `RECEIVED 10 JUL 1992`. If your table's fiction runs in 1992, this sells it completely. (Only *future* dates get skipped by Jekyll.)

**Retheme the password screen.** In `.github/workflows/deploy.yml`, the StatiCrypt block near the bottom has template strings you can edit in place (single-line value edits — no indentation risk). Suggested 1992 flavor:

```
--template-title "DDN GATEWAY 0448"
--template-instructions "This is a U.S. Government computer system. Use constitutes consent to monitoring. Enter access code."
--template-button "TRANSMIT"
--template-error "ACCESS DENIED. INCIDENT LOGGED."
--template-color-primary "#33ff66"
--template-color-secondary "#030905"
```

**The road ahead (your era-shift plan).** The architecture already supports it: all theming lives in one CSS file plus two layouts, and every page hook is a stable class name. The 2015 SIPRNET refresh will be a same-shaped drop-in (flat-panel look, modern classification banners, the delta rendered cleaner). The "corruption" phase can layer on top of whatever theme is live — CSS/JS that misbehaves in small, deniable ways. When your table gets there, come back with this repo and we'll build the next mask.
