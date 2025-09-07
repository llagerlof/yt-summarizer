# yt-summarizer

A tiny Bash tool that downloads YouTube subtitles and generates a high‑quality summary using the OpenAI API.

## Features

- **Smart subtitle retrieval**: Prefers official subtitles; falls back to auto‑generated captions.
- **Language handling**: Uses the detected video language; falls back to English when unknown.
- **VTT cleaning**: Strips tags and cue settings to reduce noise and token usage.
- **Saved artifacts**: Writes both the original and cleaned VTT files alongside the summary.
- **Simple configuration**: Set your API key, model (default `gpt-4o-mini`), and prompt once.
- **Convenient output**: Timestamped filenames with a sanitized video title.
- **Optional editor opening**: Add `-e` to open the summary right away.
- **Logging**: Detailed logs written to `./yt-summarizer.log`.

## Prerequisites

- Bash (tested on Linux; macOS should work as well)
- `yt-dlp`
- `jq`
- `curl`
- An OpenAI API key

## Installation

1. Make the script executable:
   ```bash
   chmod +x yt-summarizer
   ```
2. Place it somewhere on your `PATH` (for example):
   ```bash
   sudo cp yt-summarizer /usr/local/bin/
   ```

## Configuration

On the first run, the script creates `~/.config/yt-summarizer.conf` if it does not exist. It contains:

- `OpenAIAPIKey` – your OpenAI API key
- `OpenAIModel` – model name used in the API call (default `gpt-4o-mini`)
- `DefaultPrompt` – prompt prepended to the transcript before sending it to the API

Example configuration (replace the example key with your own):

```ini
OpenAIAPIKey=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
OpenAIModel=gpt-4o-mini
DefaultPrompt="This is a YouTube transcription. Create a detailed, well‑explained summary. Make it read naturally, with a serious yet conversational tone, focusing on the important topics. If the transcription explains how to do something, include those explanations, organized by topic. Favor clarity over brevity. Use bullet points when helpful. Organize the summary into sections with brief titles. Each section can have one or more paragraphs. If people’s names appear, include the name (and the person’s role/task if available). Write the summary in English using Markdown."
```

If the file is created automatically, `OpenAIAPIKey` is left blank. Edit the file and add your key before running the script again.

## Usage

```bash
yt-summarizer [-e] <youtube-url>
```

The summary is written to:

```
<sanitized video title>_YYYY-MM-DD_hh-mm-ss_summary.txt
```

### Options

- `-e` – open the summary file with an editor after generation
- `-h` – show a short help message
- `-v` – print the script version

## Example

```bash
yt-summarizer https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

Outputs (filenames include the sanitized title and a timestamp):

```
<title>_YYYY-MM-DD_hh-mm-ss_summary.txt
<title>_YYYY-MM-DD_hh-mm-ss_original.vtt
<title>_YYYY-MM-DD_hh-mm-ss_clean.vtt
```

## How it works

1. Uses `yt-dlp` to fetch official subtitles in the video’s language; if unavailable, falls back to auto‑generated captions. If the language cannot be detected, English is used as a fallback.
2. Cleans the downloaded VTT to remove tags and cue settings, reducing noise and token usage.
3. Sends the cleaned transcript to the OpenAI API along with your configured prompt.
4. Saves the returned summary as a timestamped text file next to the VTT files.

## Troubleshooting

- **“Failed to download any subtitles”**: The video may not have subtitles. Check what’s available:
  ```bash
  yt-dlp --list-subs <youtube-url>
  ```
- **API errors or empty responses**: Ensure your `OpenAIAPIKey` is set correctly and that your account has quota.
- **Missing commands**: Install `yt-dlp`, `jq`, and `curl` and make sure they’re on your `PATH`.
- **No editor opens with `-e`**: The script tries several GUI and terminal editors, then falls back to `xdg-open`. Install or configure your preferred editor.

## Privacy

The transcript text is sent to OpenAI to generate a summary. Avoid using this tool with sensitive content if that is a concern. Your API key is read from your local config file; it is not logged, and the config file’s permissions are restricted when created.
