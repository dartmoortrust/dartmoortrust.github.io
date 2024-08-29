# dartmoortrust.github.io



# Digitising
## Audio CDS

To ensure accurate copies of audio CDs, even older ones with errors, we use `cdparanoia`.
CD Paranoia can be downloaded from here - https://xiph.org/paranoia/

### Audio Ripping Process
1. Insert Audio CD into the drive.
2. Execute the command `cdparanoia -XBZ` - this will extract the data and attempt to correct errors. The files will be saved as a WAV file in directory where the command was executed.
3. We store the files in a directory that links it back to the CD - For example, `DNPASA-1401`.
4. To control file sizes and cost, we compress the WAV files into FLAC files. This reduced the size without removing any of the audio detail.
