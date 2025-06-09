# yt-summarizer

`yt-summarizer` is a small Bash script that downloads the transcript of a YouTube video and creates a concise summary using the OpenAI API.

## How it works

1. The script uses `yt-dlp` to download the official subtitles in the video's language if available, otherwise it falls back to the auto-generated captions.
2. The subtitle file is converted to plain text.
3. The text is sent to the OpenAI API together with a prompt stored in the configuration file.
4. The returned summary is saved as a time stamped text file next to the downloaded subtitle.

## Configuration file

On the first run the script creates a configuration file at `~/.config/yt-summarizer.conf` if it does not exist. The file contains the following variables:

- `OpenAIAPIKey` – your OpenAI API key
- `OpenAIModel` – model name used in the API call (e.g. `gpt-3.5-turbo`)
- `DefaultPrompt` – default prompt prepended to the transcript before sending it to the API

Example configuration (the key is invalid and shown only for reference):

```ini
OpenAIAPIKey=sk-proj-N4TEodyvIqw7tFy3SnGeT3BlbkFJUDY4Te3pWuM9XCHYyxLe
OpenAIModel=gpt-4.1-nano
DefaultPrompt=This is a YouTube transcription text. Create a summary of this transcription in the language of the transcription. Make the text look like it was written by a human. Be serious, but use a casual tone, focusing on the important aspects of the text. Add timestamps and a brief title (between parenthesis) about the paragraph subject before each paragraph.
```

If the file is created automatically the `OpenAIAPIKey` value is left blank. Edit the file and add your key before running the script again.

## Usage

```bash
$ yt-summarizer <youtube-url>
```

The summary is written to `<video title>_YYYY-MM-DD_hh-mm-ss.txt`.

Run `yt-summarizer -h` to show a short help message or `yt-summarizer -v` to print the script version.

