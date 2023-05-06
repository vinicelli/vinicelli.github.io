## Analyzing and Interpreting Speech Patterns with Praat and Parselmouth

## Introduction
Praat is a popular software package mainly used for speech analysis. It has been around since 1995 and is still widely used in linguistics and NLP fields. While being consitently updated by its creators Paul Boersma and David Weenink of the University of Amsterdam, [its website](https://www.fon.hum.uva.nl/praat/) is a mid-90s time capsule. Praat has a broad functionality, and supports spectrogram visualization, pitch tracking, formant analysis, and speech synthesis.

[Parselmouth](https://parselmouth.readthedocs.io/en/stable/) is a Python library built specifically to interact with Praat and audio files, creating a seamless development environment to utilize the power of Python and Praat. The goal of Parselmouth is specifically to provide a "Pythonic" interface to Praat, rather than using the native C-based script that the software uses. 

In this tutorial I hope to give you a very brief look into the capabilities of Praat, and some strategies to use Parselmouth for NLP tasks.

## Prerequisites
In order to run the examples in this tutorial, you will need Python 3, Praat, and Parsertongue installed on your machine. These were run wothin Jupyter notebooks in a native Linux environent (Ubuntu 20.04.6).

## Loading a Sound file Using Parselmouth
Parselmouth makes it very easy to load a sound file for analysis.

``` 
import parselmouth as pm

sound = pm.Sound(Test.wav) 
```
From there, the file is ready to be read and manipulated using Praat.

The included test file includes a short clip of me saying "This is a sentence I am recording for Praat." Feel free to use any audio file you'd like, but know that these programs are happiest when working with .wav files.

We can use other common libraries like matplotlib to visualize the recording, with things like spectrograms:
```
import parselmouth
import matplotlib.pyplot as plt

voice = parselmouth.Sound("ShortenedTest.wav")

spectrogram = voice.to_spectrogram()

plt.imshow(spectrogram.values.T, aspect='auto', origin='lower', cmap='jet')
plt.xlabel("Time (s)")
plt.ylabel("Frequency (Hz)")
plt.show()
```
![Output](https://raw.githubusercontent.com/vinicelli/vinicelli.github.io/main/Spectrogram.png)

...or pitch contour plots:
```
pitch = voice.to_pitch()

# Get the pitch contour values and times
pitch_values = pitch.selected_array['frequency']
pitch_times = pitch.xs()

# Plot the pitch contour using matplotlib
plt.plot(pitch_times, pitch_values, linewidth=2, color='black')
plt.xlabel("Time (s)")
plt.ylabel("Pitch (Hz)")
plt.show()
```
![Output](https://raw.githubusercontent.com/vinicelli/vinicelli.github.io/main/Pitch_Contour.png)
