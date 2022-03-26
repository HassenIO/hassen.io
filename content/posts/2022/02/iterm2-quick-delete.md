---
title: "Delete words and line in iTerm2"
date: 2022-02-18T21:56:00+01:00
draft: false
summary: A quick post showing my iTerm2 settings to delete words and a line
---

tl;dr: This is a short practical post on how to delete words and line in iTerm2.

Look at [the previous article](/2022/02/iterm2-quick-move.md) on how to find the key mappings settings.

# Delete a word

From the Key Mappings page, go back to General, then on the `Left Option Key`, check the `Esc+` checkbox.

This will allow you to delete the previous word by hitting `⌥ ⌫`.

![Delete a word in iTerm2](/2022/02/step-5.png "Delete a word in iTerm2")

# Delete the whole line

In the key mappings section, click on the `+` button like shown below, and set the following:

- Keyboard Shortcut: Keep `⌘ ⌫`
- Action: `Send Hex Code`
- Esc+: `0x15`

Click on OK. You can delete the line with `⌘ ⌫`

![Delete line in iTerm2](/2022/02/step-4.png "Delete line in iTerm2")
