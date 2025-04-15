#Digitisation

## Audio CDS

To ensure accurate copies of audio CDs, even older ones with errors, we use `cdparanoia`.
CD Paranoia can be downloaded from here - https://xiph.org/paranoia/

### Audio Ripping Process

1. Insert Audio CD into the drive.
2. Execute the command `cdparanoia -B` - this will extract the data and attempt to correct errors. The files will be saved as a WAV file in directory where the command was executed.
3. We store the files in a directory that links it back to the CD - For example, `DNPASA-1401`.
4. cdparanoia will adjust the speed of the ripping process depending on the condition of the disk. If it is in a poor condition it can take several hours. The software will attempt to repair any data errors it finds and report on this in the terminal as it is running. You will see an output similar to this `(== PROGRESS == [    +eeee>                    | 009870 00 ] == :-P 0 ==)` The icons are explained [here](https://xiph.org/paranoia/manual.html).
5. To control file sizes and cost, we compress the WAV files into FLAC files. This reduced the size without removing any of the audio detail.
6. A subtitle file is created for audio files assuming there are spoken words.
7. Chapters may be created where desired.

# Transcription and Captions
We store our transcription and chapter information in [WebVTT files](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).

## Captions Example
````
WEBVTT

00:00.000 --> 00:00.900
Hildy!

00:01.000 --> 00:01.400
How are you?

00:01.500 --> 00:02.900
Tell me, is the lord of the universe in?

00:03.000 --> 00:04.200
Yes, he's in - in a bad humor

00:04.300 --> 00:06.000
Somebody must've stolen the crown jewels
````


## Chapter Example
Chapters allow for a way to link an audio timestamp to a description to enable the user to find content of interest. They also use the WebVTT format but are tagged as chapters in the HTML.

````
WEBVTT

00:01.000 --> 00:04.000
Chapter 1 - Title One

00:05.000 --> 00:09.000
Chapter 2 - Title 2
````
Chapters are maintained in our database as follows
Note: Chapter number may be redundent as they would follow sequentially when ordered by start_time.
|file_id|chapter_number|chapter_title|start_time|
|-------|---------|---------|------|
|jshdfkjfhsdjfhsd|1|A chat about trains|00:01:00|
