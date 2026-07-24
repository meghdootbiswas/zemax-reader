# Zemax Reader

A single-page web tool that reads Zemax OpticStudio sequential lens files (`.zmx`, `.seq`)
and displays the full prescription, system configuration, and a scale layout drawing.

**Everything runs in the browser.** There is no server and no upload — the file is read
locally with the `FileReader` API. This matters for optical design work, where lens
prescriptions are often proprietary.

## Accuracy warning

Ray diagrams produced by this tool are **approximate and can be wrong**. Layouts and rays use
first-order (paraxial) optics only — there is no real ray tracing. Folded systems are drawn
unfolded, and designs relying on solves, pickups or model glasses may render or compute
incorrectly. Treat the output as a reading aid, not as verified analysis, and always confirm
against OpticStudio before relying on it.

## Privacy

There is no server, no database, no analytics and no cookies. The file you choose is read
locally by the browser's `FileReader` API and never leaves your machine — it is not uploaded,
stored, logged or transmitted. The page works with the network disconnected, which is the
simplest way to confirm this for yourself.

## Features

- **Works on any screen** — desktop, laptop, tablet and phone, with a light and dark theme
  that follows your system preference and can be toggled.

- **Robust decoding** — `.zmx` files are usually UTF-16LE with a BOM, which breaks naive
  text readers. The parser sniffs UTF-16LE/BE and UTF-8 automatically.
- **Full surface prescription** — radius (from stored curvature), thickness, glass,
  semi-diameter, conic constant, comments, stop, and mirror flags.
- **System summary** — mode, units, entrance pupil, track length, wavelengths, surface counts.
- **Fields & wavelengths** — decodes `FTYP` field type (angle / object height / image height),
  `XFLN`/`YFLN` field points, and de-duplicates the 24 wavelength slots Zemax always writes.
- **Coordinate breaks** — decodes `PARM` tilt/decenter values, so folded systems
  (Newtonians, off-axis designs) read correctly.
- **First-order analysis** — paraxial ABCD matrix gives EFL, back focal distance and f/number
  for pure refractive systems, using real Sellmeier dispersion for common Schott glasses.
- **Consistency check** — compares the computed back focal distance against the image
  distance stored in the file and reports whether they agree.
- **Layout drawing** — elements drawn to scale from the actual sag of each surface.
- **Export** — prescription as CSV, or the whole parsed model as JSON.

## Deploying to GitHub Pages

1. Create a repository, e.g. `zemax-reader`.
2. Commit `index.html` (and this README) to the default branch.
3. Repo **Settings → Pages → Source: Deploy from a branch**, branch `main`, folder `/ (root)`.
4. The site appears at `https://<user>.github.io/zemax-reader/` within a minute or so.

No build step, no dependencies, no framework. `index.html` is the entire application.

## Known limits — please read

This tool is a **file reader and first-order calculator**, not a ray-tracing engine.
Being clear about what it cannot do matters more than the feature list:

- **No real ray tracing.** No spot diagrams, MTF, wavefront error, distortion, or tolerancing.
  Anything requiring finite (non-paraxial) rays is out of scope.
- **Solves and pickups are shown, not resolved.** Zemax curvature/thickness solves,
  pickup chains, and variable flags are read from the file but not evaluated. A design
  whose surfaces depend on solves may report first-order values that differ from
  OpticStudio's.
- **Model glasses are approximate.** Catalog glasses use proper Sellmeier coefficients,
  but model glasses (specified by index/Abbe rather than name) fall back to the stored
  index. If a design's computed back focus disagrees with its stored image distance,
  this is the usual cause — the tool flags it rather than silently fudging the numbers.
- **Paraxial analysis is skipped for mirrors and coordinate breaks.** Folded and
  catadioptric systems need a full 3-D trace. The prescription table is still exact;
  the tool declines to print a first-order number it cannot compute honestly.
- **The layout is unfolded.** Tilts and decenters are listed in the table but the drawing
  lays surfaces along a straight axis, so a Newtonian will *not* show its 90° fold.
- **Only text-based sequential files.** Binary `.zos` and zipped `.zar` archives are not
  supported. Non-sequential (`MODE NSC`) files will parse partially at best.
- **Glass catalog is a subset.** Roughly a dozen common Schott glasses have Sellmeier data.
  Unknown glasses are displayed by name with no index computed.

## Extending it

The parser is one function (`parseZMX`) returning a plain object, so adding keywords is easy.
Useful next steps: more glass catalogs (Ohara, Hoya, CDGM), aspheric coefficient display for
`EVENASPH` surfaces, a real folded layout that honours coordinate breaks, and a finite
ray trace for spot diagrams.

## License

MIT — do what you like with it.
