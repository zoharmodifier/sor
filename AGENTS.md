# Agent Guide

## Project overview

This is a dependency-free static website that displays grouped Spotify track players. It is intended to work both from Azure Static Web Apps and when `index.html` is opened directly with a `file://` URL.

## Important files

- `index.html`: The rendered page and all Spotify iframe embeds.
- `styles.css`: Shared page styling.
- `spotify-playlist-115E3b7ibKCUOeXOQVNjZs.md`: Playlist metadata file that determines the content of the website.

## Updating the page

Future prompts may ask to load any existing playlist MD file, create a new playlist MD file, or import a Spotify playlist URL. Use the playlist file explicitly named in the prompt as the source of truth. If the prompt creates a new file and then asks to update the page, use that newly created file.

`index.html` must contain static iframe markup generated from the selected playlist file. Replace the existing groups and embeds rather than combining playlists unless the prompt explicitly requests a merge. Do not use browser-side `fetch()` to load JSON because browsers block local JSON requests when the page is opened through `file://`.

Each song should use this iframe structure:

```html
<iframe class="spotify-player" data-testid="embed-iframe" src="https://open.spotify.com/embed/track/SPOTIFY_TRACK_ID?utm_source=generator" title="SONG_NAME Spotify player" width="100%" height="152" frameborder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>
```

Escape characters such as double quotes in HTML attribute values. Keep an accessible `title` containing the song name.

## Importing a Spotify playlist

For a public Spotify playlist:

1. Retrieve the playlist page.
2. Read the ordered track IDs from its `music:song` metadata.
3. Resolve each track name through Spotify's public oEmbed endpoint:
   `https://open.spotify.com/oembed?url=https://open.spotify.com/track/TRACK_ID`
4. Write the names and IDs into the file requested by the user, or a clearly named new playlist JSON file if no destination is specified.
5. If requested, regenerate the static iframe sections in `index.html` from that file.

If meaningful groups are unknown, use neutral sequential names such as `Group1`, `Group2`, and so on.

## Validation

There is no build step or test suite. Before finishing:

- Parse the selected playlist file to confirm it is valid JSON.
- Confirm every `spotify hash` occurs exactly once in `index.html`.
- Confirm the embedded group and track order exactly matches the selected playlist file.
- Run `git diff --check`.
