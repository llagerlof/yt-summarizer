# yt-summarizer

`yt-summarizer` is a small Bash script that downloads the transcript of a YouTube video and creates a concise summary using the OpenAI API.

## How it works

1. The script uses `yt-dlp` to download the official subtitles in the video's language if available, otherwise it falls back to the auto-generated captions.
2. The subtitle file (VTT) content is used as-is.
3. The transcript is sent to the OpenAI API together with a prompt stored in the configuration file.
4. The returned summary is saved as a time stamped text file next to the downloaded subtitle.

## Configuration file

On the first run the script creates a configuration file at `~/.config/yt-summarizer.conf` if it does not exist. The file contains the following variables:

- `OpenAIAPIKey` – your OpenAI API key
- `OpenAIModel` – model name used in the API call (default `gpt-4o-mini`)
- `DefaultPrompt` – default prompt prepended to the transcript before sending it to the API

Example configuration (replace the example key with your own):

```ini
OpenAIAPIKey=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
OpenAIModel=gpt-4o-mini
DefaultPrompt="This is a YouTube transcription text. Create a detailed, well explained summary of this transcription. Make the text look like it was written by a human. Be serious but use a casual tone, focus on the important aspects of the subjects. If the transcription has explanations on how to do something, include these explanations in the summary organized by topics. A better explained text must be prioritized over shortness. You can use bullet points in some parts if the context if it is relevant. Organize the summary in sections and add a brief title before each section. Each section can have one or more paragraphs. If people's names are mentioned in the text you must also mention the name (and the person's role/task if available) in the summary. Write the summary in english in markdown format."
```

If the file is created automatically the `OpenAIAPIKey` value is left blank. Edit the file and add your key before running the script again.

## Usage

```bash
$ yt-summarizer [-e] <youtube-url>
```

The summary is written to `<sanitized video title>_YYYY-MM-DD_hh-mm-ss.txt`.

Options:
- `-e`   Open the summary file with an editor after generation
- `-h`   Show a short help message
- `-v`   Print the script version

Run `yt-summarizer -h` to show a short help message or `yt-summarizer -v` to print the script version.

