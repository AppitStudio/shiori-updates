You are helping to publish a new version of Shiori. Follow these steps carefully:

## Step 1: Read version.md
Read the `version.md` file to get:
- VERSION: The version number (e.g., 1.0.0)
- DETAILS: What's new in this version

## Step 2: Find the new appcast file
Look for a file named either:
- `appcast copy.xml`
- `appcast 2.xml`

This file is generated from the build process and contains the new version with the hashed signature.

## Step 3: Extract content from current appcast.xml
From the current `appcast.xml`, extract:
1. The `<description>` section (including full CDATA content) as a style reference

## Step 4: Update the new appcast file
Add the required content to the new appcast file:
1. Add the `<sparkle:fullReleaseNotesLink>` section after `<sparkle:minimumSystemVersion>`:
   ```xml
   <sparkle:fullReleaseNotesLink>
       https://appitstudio.github.io/shiori-updates/release-notes.html</sparkle:fullReleaseNotesLink>
   ```
2. **CRITICAL**: Update the `url` attribute in the `<enclosure>` tag to use the versioned GitHub releases URL:
   - Format: `https://github.com/AppitStudio/shiori-updates/releases/download/v{VERSION}/Shiori.dmg`
   - Example: For version 1.2.0, use `https://github.com/AppitStudio/shiori-updates/releases/download/v1.2.0/Shiori.dmg`
   - The version must include the `v` prefix in the URL path

## Step 5: Generate release notes
Using DETAILS from version.md, generate release notes following this format:
- Main title: `Shiori {VERSION} - [Creative Subtitle]`
- Section headings (example: `What's New`, `Improvements`, `Fixes`)
- Use bullet points with `<strong>Feature Name:</strong> description` format
- Keep tone clear, concise, and user-focused
- End with a short thank-you note

## Step 6: Update the new appcast file description
Replace `<description>` content in the new appcast file with the generated release notes wrapped in CDATA:
```xml
<description><![CDATA[
[Generated release notes HTML here]
]]></description>
```

## Step 7: Update release-notes.html
Add a new `<div data-sparkle-version="{VERSION}">` section at the top of `<body>` (right after `<body>`), containing the same release notes HTML.
- Use version number WITHOUT `v` in the data attribute (`1.2.0`, not `v1.2.0`)

## Step 8: Replace appcast.xml
Replace current `appcast.xml` with the updated new appcast file content.

## Step 9: Clean up
Delete the temporary appcast file (`appcast copy.xml` or `appcast 2.xml`).

## Important Notes
- Maintain exact XML structure and valid CDATA blocks
- Preserve EdDSA signature from generated appcast file
- Ensure enclosure URL points to versioned `Shiori.dmg` release asset
- Keep `data-sparkle-version` without the `v` prefix
