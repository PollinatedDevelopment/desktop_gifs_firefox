# Desktop GIFs for Firefox

Post GIFs in YouTube comments and replies from desktop Firefox.

YouTube's mobile app can attach a GIF to a comment; the desktop site has no button for it. Desktop GIFs adds one. The GIFs it posts are ordinary YouTube GIF comments, so everyone sees them, with or without the extension.

This is a Firefox port of [Desktop GIFs](https://github.com/rayytor/desktop_gifs) by rayytor, which targets Chrome.

## How to install

> **The `.xpi` file is the extension.** This repository contains only the `.xpi` and the `LICENSE`. There is nothing else to import.

1. Click the green **Code** button at the top of this page, then click **Download ZIP**.
2. Unzip the downloaded file.
3. Find the file ending in **`.xpi`** (currently `5f75c9ca2a5e48abb3c3-0.1.0.xpi`).
4. Open Firefox and drag the `.xpi` file into the window, then click **Add**.
   Or open `about:addons`, click the gear icon, choose **Install Add-on From File…** and select the `.xpi`.
5. Open `about:addons`, select Desktop GIFs, go to **Permissions** and make sure access to `tenor.com` is on.
6. Reload any YouTube tabs that were already open.

It needs Firefox 140 or newer (Firefox for Android 142 or newer). The `.xpi` is signed by Mozilla as an unlisted add-on, so it installs permanently and survives restarts. It is not listed on addons.mozilla.org.

## Use

1. Sign in to YouTube and open a video or a Short.
2. Click **Add a comment…**, or **Reply** under a comment. A **GIF** button appears next to the emoji button.
3. Click it. The picker opens with Tenor's featured GIFs.
4. Find a GIF:
   - type to search Tenor, or
   - paste a Tenor link (`tenor.com/view/…`, a `media.tenor.com` file link, or a bare Tenor ID).
5. Click a GIF. **It is posted straight away**, there is no preview step.

Anything you typed in the comment box is posted together with the GIF.

| Key | What it does |
| --- | --- |
| <kbd>Enter</kbd> | Posts the GIF when the picker shows exactly one result, as it does after pasting a link |
| <kbd>Esc</kbd> | Closes the picker |

If YouTube does not send the comment by itself, the picker asks you to press the Comment button. If the post fails, nothing is published and your text stays in the box.

## Limits

- **Unofficial.** This project is not affiliated with YouTube, Google, Tenor or Mozilla. It sends a field that YouTube's desktop site does not use itself, so it can stop working whenever YouTube changes something, and using it is at your own risk under YouTube's Terms of Service.
- **Tenor only.** YouTube's GIF comments take Tenor GIFs. Tenor closed its public API in June 2026, so search results are read from Tenor's web pages and will break if those pages change. Pasting a link keeps working without search.
- **GIFs only.** Stickers and still images are left out of the results.
- **New comments and replies only.** You cannot add a GIF while editing a comment or writing a channel post.

## How it works

An `.xpi` file is a zip archive, and this one contains the extension's full, unminified source. To read it, make a copy, rename the copy to end in `.zip` and open it.

| File inside the `.xpi` | Runs in | Job |
| --- | --- | --- |
| `src/content.js`, `src/content.css` | YouTube page, isolated world | Adds the GIF button and the picker |
| `src/main.js` | YouTube page, main world | Rewrites the comment request so it carries the GIF |
| `src/sw.js` | Background script | Fetches search results from tenor.com |

When you pick a GIF, the content script adds a one-time marker to the comment box and presses YouTube's own Comment button. YouTube builds and signs the request as usual. The main-world script catches that request, removes the marker and adds the GIF's Tenor ID. A marker that cannot be turned into a GIF is blocked, so it never appears as comment text.

### Differences from the Chrome version

- The manifest uses `background.scripts` instead of a service worker, and declares a Firefox add-on ID, a minimum Firefox version and `data_collection_permissions`.
- The button icon is inlined in `content.css` as a data URI, because Firefox extension URLs are different for every install.
- `content.js` and `sw.js` use `browser` when it exists and fall back to `chrome`.

## Privacy

The extension has one permission: access to `tenor.com`, used for search. It collects nothing, stores nothing and has no analytics. Search requests go to Tenor without your cookies, and GIF previews load from Tenor's media servers. It never reads your YouTube credentials; YouTube's own page sends the comment.

## License

Copyright (C) 2026 rayytor

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, version 3. It is distributed without any warranty. See [LICENSE](LICENSE).

Firefox port modifications are released under the same license.
