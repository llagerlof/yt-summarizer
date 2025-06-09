# yt-summarizer
YouTube Summarizer

This is a shell script (bash) that generates a summarized version of the transcription of an YouTube video.

## How it works

It uses the cli program yt-dlp to download the transcription and use the OpenAI API to generate the summary.

## Configuration file

The configuration file 'yt-summarizer.conf' contains the:

- OpenAIAPIKey
- OpenAIModel
- DefaultPrompt

Example (the key here is an invalid key, for example purposes):
```
OpenAIAPIKey=sk-proj-N4TEodyvIqw7tFy3SnGeT3BlbkFJUDY4Te3pWuM9XCHYyxLe
OpenAIModel=gpt-4.1-nano
DefaultPrompt=This is a YouTube transcription text. Create a summary of this transcription in the language of the transcription. Make the text look like it was written by a human. Be serious, but use a casual tone, focusing on the important aspects of the text. Add timestamps and a brief title (between parenthesis) about the paragraph subject before each paragraph.
```

## First time run

When executing the script for the first time, create the configuration file above in ~/.config directory with the OpenAIModel and DefaultPrompt (leaving OpenAIAPIKey blank) and tell the user he must configure at least the OpenAIAPIKey.

## Usage (after fully configured)

```
$ yt-summarizer https://www.youtube.com/watch?v=7bgQcvXT9uc
```

The final result is the generated summary in text format: <video title>_YYYY-MM-DD_hh-mm-ss.txt
