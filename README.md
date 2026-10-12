# fluentbird-tb157-patches

A small `custom.css` patch file for [FluentBird](https://github.com/Deathbyteacup/fluentbird)
(a Fluent Design `userChrome.css` theme for Mozilla Thunderbird), fixing a few
issues found specifically on **Thunderbird 157**.

FluentBird itself is unmodified — this only adds a `custom.css` override,
which is the standard extension point FluentBird's own `userChrome.css`
already `@import`s.

## What this fixes

1. **Message subject wraps to 2 lines when it should fit on 1.**
   Stock Thunderbird 157 (`messageHeader.css`) bumps the subject's font size
   by 35% via `.message-header-large-subject`, and that row shares space
   with the date/crypto/security buttons. Short subjects — especially ones
   with CJK text — can wrap unnecessarily as a result. Not a FluentBird bug,
   a stock layout issue the theme happens to make more noticeable.

2. **Message list cards have asymmetric left/right padding and no rounding.**
   Stock Thunderbird 157 (`threadCard.css`) sets `padding-inline-start` on
   `.card-container` with no matching `padding-inline-end`, so cards sit
   flush against the right edge. Also trims the vertical padding a bit and
   adds a small border-radius.

3. **Tabs have no visual distinction between active/inactive, and are all
   forced to the same width.**
   FluentBird styles the selected tab with a faint translucent background
   and explicitly removes Thunderbird's stock active-tab indicator line
   (`.tab-line { height: 0px }`) without adding a replacement, so unselected
   tabs are indistinguishable from the window background. It also leaves
   Thunderbird's stock `flex: 100 100; inline-size: 0` tab-width rule in
   place, which forces every open tab to the same width regardless of how
   short its label is. This patch gives active/inactive tabs distinct
   backgrounds and borders, restores an accent-colored indicator line on the
   active tab, and switches tab width to size-to-content (with sane
   min/max bounds) instead of forced-equal.

Each fix's exact root cause (file + rule) is documented inline in
[`custom.css`](./custom.css).

## Install

1. Install FluentBird first, following its own
   [installation instructions](https://github.com/Deathbyteacup/fluentbird).
2. Copy this repo's `custom.css` into your Thunderbird profile's `chrome/`
   folder, replacing FluentBird's own `custom.css` (or merging into it, if
   you've already made your own changes there).
3. Fully quit and reopen Thunderbird.

## Compatibility

Verified against Thunderbird 157.0.1 on macOS. These are stock-Thunderbird
layout rules being patched, not FluentBird-internal ones, so they should
hold up across Thunderbird point releases until Mozilla changes the
underlying CSS — but no promises beyond what's been tested.

## License

MIT, same as FluentBird itself.
