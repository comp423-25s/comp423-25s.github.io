# How To: Add a New Bio

## Guide

1. Under `people/profile_photos`, add a square profile photo titled with your ONYEN.
2. In `people/bios`, add a markdown file titled `<onyen>_about.md`. Add a title for the page:
```bash
# YOUR NAME
```
3. Go to `people/team.md`, and add the following lines inside the `<div id="TA-team">`, replacing YOUR NAME with your information:
```bash
-   ![YOUR NAME photo](../people/profile_photos/<filename from Step 1>)
    
    ---

    [__YOUR NAME__](../people/bios/<MD name from Step 2>)
```
4. Run `mkdocs serve` to confirm your info card and bio page have been added.
    * Navigate to the **People** tab and click on your card's name. You should be redirected to an empty bio page with your name.
5. Customize your bio page! Just make sure to include your name and profile photo at the top. You can search for other bios with the left navbar or using the Search bar at top right.

### [Example](./other_about.md)

