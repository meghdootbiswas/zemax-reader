
Readme · MD
# Zemax Reader
 
Read Zemax lens files in your browser. Drop a `.zmx` file and see its full prescription, system data and layout.
 
**Live:** https://meghdootbiswas.github.io/zemax-reader/
 
## What it does
 
Parses sequential Zemax files and displays:
 
- Surface prescription — radius, thickness, glass, semi-diameter, conic, stop and mirror flags
- System summary — units, entrance pupil, track length, surface counts
- Fields and wavelengths
- First-order results (EFL, back focal distance, f/number) for refractive systems
- A to-scale layout drawing
- Export to CSV or JSON
Works on desktop, tablet and phone. Light and dark themes.
 
## Privacy
 
No server, no database, no analytics, no cookies. Your file is read locally by the browser and never uploaded or transmitted. The page works offline — disconnect from the internet and it still runs.
 
## Warnings
 
**Ray diagrams are approximate and can be wrong.** Rays are first-order (paraxial) only — there is no real ray tracing.
 
Other limits worth knowing:
 
- No spot diagrams, MTF, wavefront error or tolerancing
- Solves and pickups are displayed but not evaluated
- Model glasses fall back to the stored index, so computed values may differ from OpticStudio
- First-order analysis is skipped for mirrors and coordinate breaks
- Layouts are drawn unfolded — tilts and decenters are listed but not shown in position
- Only text-based sequential `.zmx` files; binary `.zos` and `.zar` archives are not supported
Treat the output as a reading aid, not verified analysis. Check anything important against OpticStudio.
 
## Usage
 
Open the [live page](https://meghdootbiswas.github.io/zemax-reader/) and drop in a `.zmx` file.
 
To run it locally or offline, download `index.html` and open it in any browser. That single file is the entire application — no build step, no dependencies, no install.
 
## Author
 
Meghdoot Biswas — Wyant College of Optical Sciences, University of Arizona.
 
## Acknowledgements
 
Built with the assistance of Claude (Opus 4.8) by Anthropic, which contributed to the parser, the first-order calculations and the interface. All output was reviewed and tested against real Zemax files.
 
## License
 
MIT — see [LICENSE](LICENSE).
 
Not affiliated with or endorsed by Ansys or Zemax. "Zemax" and "OpticStudio" are trademarks of their respective owners.
