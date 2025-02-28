# Digitising

## Source media and outputs

| Medium             | Master   | Web  | Derived   |
| ------------------ | -------- | ---- | --------- |
| Audio CD           | flac     | mp3  | subtitles |
| Audio Digital      | Existing | mp3  | subtitles |
| Printed Photograph | tiff     | jpeg | none      |
| Digital Photograph | Existing | jpeg | none      |
| Printed Document   | TBD      | pdf  | none      |
| Digital Document   | TBC      | pdf  | none      |

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
