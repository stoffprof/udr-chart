# Univariate Distribution Relationships chart (patched)

A copy of Larry Leemis's interactive
[Univariate Distribution Relationships chart](https://www.math.wm.edu/~leemis/chart/UDR/UDR.html)
(William & Mary) with a fix for misaligned hotspots. Only the page itself lives here. The
chart images, PDFs and stylesheet still load from the original site through a `<base href>` tag.

## The bug

The chart is one large image with 287 clickable regions (an HTML image map). When you zoom,
JavaScript rescales each region relative to the image's "original" width. The original page
recorded that width in `window.onload`. That event waits for every image and for the analytics
and visitor-counter scripts. If you hovered a name in the distribution list, or clicked zoom,
before then, the chart had already been resized, so the wrong width was recorded. From that point
every region was scaled by the wrong factor (about 1.23×), and hovering highlighted the wrong
distribution until you reloaded the page.

## The fix

- Use the image's native width (1892 px) as the reference instead of measuring it at load time.
- Capture the original region coordinates the first time any scaling happens, so they are always
  the unscaled ones.
- Initialise on `DOMContentLoaded` rather than `window.onload`.
- Fix `newwidth=+1` (sets the width to 1) to `newwidth+=1`.
- Remove the original site's Google Analytics tag and visitor counter, so this copy doesn't report
  visits to the original author's accounts.

The chart and its content are Larry Leemis's work.
