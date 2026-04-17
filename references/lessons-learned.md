# Lessons Learned

From the dopeskills.ai landing page build (2026-04-16).

## AMP Boilerplate Must Be Exact

**Problem:** Page failed AMP validation with "amp-boilerplate tags malformed." The boilerplate was missing `-ms-animation`, `-ms-keyframes`, and `-o-keyframes` vendor prefixes.

**Fix:** Copy the boilerplate from the official AMP docs character-for-character. Do not write it from memory. Do not simplify it. The required prefixes are: `-webkit-`, `-moz-`, `-ms-`, `-o-`, and unprefixed. The `<noscript>` block must also include `-ms-animation:none`.

## @import Kills AMP Validation

**Problem:** Used `@import url('https://fonts.googleapis.com/...')` inside `<style amp-custom>`. This caused a "CSS syntax error" that cascaded and also corrupted the boilerplate detection.

**Fix:** Replace `@import` with `<link rel="stylesheet" href="...">` in the `<head>`. AMP allows external Google Fonts via link tags but prohibits @import inside the custom style block.

## Glow Orbs Cause Mobile Horizontal Scroll

**Problem:** Decorative background elements (blurred circles for ambient glow effect) were positioned with negative offsets (`right: -200px`, `left: -250px`). On mobile, these extended the document width beyond the viewport, causing horizontal scroll even with `overflow-x: hidden` on the body.

**Fix:** Either remove decorative absolute-positioned elements on mobile via media query, or remove them entirely. `overflow: hidden` on the body is not sufficient because the elements create document width before the overflow is clipped. Adding `overflow: hidden` to every parent section also helps but is fragile.

## Grid minmax Causes Mobile Overflow

**Problem:** `grid-template-columns: repeat(auto-fit, minmax(320px, 1fr))` forces a minimum column width of 320px. On screens narrower than 320px + padding, this overflows.

**Fix:** Use `min()` function: `minmax(min(320px, 100%), 1fr)`. This caps the minimum at either 320px or 100% of the container, whichever is smaller. Works on all screen sizes.

## WebP Logo from Telegram

**Problem:** Sending a PNG logo through Telegram as a photo converts it to JPEG, destroying transparency. Sending as a sticker converts to WebP but preserves transparency.

**Fix:** Use WebP directly -- AMP supports it natively via `<amp-img>`. If you need PNG, ask the user to send as a document/file attachment, not as a photo.

## AMP Shimmer Animation

**Problem:** Wanted a shimmer/gleam effect on CTA buttons but AMP doesn't allow JavaScript.

**Fix:** Pure CSS `@keyframes` animation using a pseudo-element. A `::after` element with a diagonal white gradient slides across the button. Animation timing uses a long cycle (6s) with the actual sweep in the first 16% (~1s), creating a natural "gleam then rest" pattern. Fully AMP-compatible.
