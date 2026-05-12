# October Books Mesh, Pre-deployment Documentation

A follow-along record of preparing a four-node Meshtastic mesh deployment between October Books and Highfield in Southampton, May 2026.

**Live site:** https://yadirasanchezbenitez.github.io/october-books-mesh/

## What's here

- The Site Planner walkthrough, with the actual coverage map produced
- The printable A4 survey sheet used on the walk (Word + PDF)
- The findings from the walk (to be added after 14 May 2026)

## Hardware

Four [SenseCAP Solar Node P1-Pro for Meshtastic](https://www.seeedstudio.com/SenseCAP-Solar-Node-P1-Pro-for-Meshtastic-LoRa-p-6412.html) units from Seeed Studio, mounted in planters with hand-woven wigwam structures along the 1.2 km route from October Books to Highfield campus.

## Build locally

```bash
pip install mkdocs-material pymdown-extensions
mkdocs serve
```

Open http://127.0.0.1:8000.

## Deploy

Pushing to `main` triggers GitHub Actions to build and publish to GitHub Pages. In repo Settings → Pages, set the source to "GitHub Actions."

## License

Text and images CC BY-SA 4.0. Code MIT.

## Citation

```
October Books Mesh: pre-deployment documentation.
Winchester School of Art, University of Southampton.
```
