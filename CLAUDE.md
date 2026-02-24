# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository manages the Sparkle auto-update feed for Shiori, a macOS app by AppIt Studio. The repository's purpose is to distribute app updates and release notes through Sparkle's RSS-based update system via GitHub Pages.

## Architecture

### Core Files
- **`appcast.xml`** - Production Sparkle update feed in RSS format
  - Contains a single `<item>` element with the latest version information
  - Critical fields: `sparkle:version` (build number), `sparkle:shortVersionString` (display version), `enclosure url` (DMG download link), `sparkle:edSignature` (EdDSA security signature)
  - Links to `release-notes.html` via `sparkle:fullReleaseNotesLink`
  - Hosted URL: `https://appitstudio.github.io/shiori-updates/appcast.xml`

- **`release-notes.html`** - HTML page displayed in the Sparkle updater window
  - Each version is wrapped in a `<div data-sparkle-version="X.Y.Z">` container
  - New versions are added at the top, older versions below

- **`appcast-example.xml`** - Reference template kept for structural reference

- **`version.md`** - Scratch file used during release prep
  - Stores `VERSION` and `DETAILS` used to generate release notes

### Sparkle Integration
- **Minimum System Version:** macOS 12.4+
- **Security:** Uses EdDSA signatures (`sparkle:edSignature`) for secure update verification
- **File Hosting:** DMG files hosted on GitHub Releases using versioned tags
- **Distribution:** GitHub Pages serves the static XML/HTML files

## Common Development Tasks

### Publishing a New Release

When publishing a new version of Shiori:

1. **Update `appcast.xml`:**
   - Increment `sparkle:version` (integer build number, e.g., 1 -> 2)
   - Update `sparkle:shortVersionString` (semantic version, e.g., "0.9" -> "1.0")
   - Update `pubDate` to current timestamp in RFC 2822 format
   - Update `enclosure url` to versioned release URL
   - Update `sparkle:edSignature` with the new EdDSA signature from Sparkle tools
   - Update the inline `<description>` CDATA section with release notes HTML

2. **Update `release-notes.html`:**
   - Add a new `<div data-sparkle-version="X.Y.Z">` section at the top of `<body>`
   - Keep release notes concise, user-facing, and readable in Sparkle

3. **Commit:**
   - Use a clear commit message like: `bump v1.0.0`

### Release Asset URL Format
Use this format in `appcast.xml` enclosure URL:
- `https://github.com/AppitStudio/shiori-updates/releases/download/v{VERSION}/Shiori.dmg`

Example:
- `https://github.com/AppitStudio/shiori-updates/releases/download/v1.0.0/Shiori.dmg`

## Technical Notes

- Static repository: direct XML/HTML/Markdown editing
- No compile/build process in this repo
- Validate update flow from the Shiori app after publishing XML + release asset
