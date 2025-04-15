# Digitisation

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
