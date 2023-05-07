## Analyzing and Interpreting Speech Patterns with Praat and Parselmouth

## Introduction
Praat is a popular software package mainly used for speech analysis. It has been around since 1995 and is still widely used in linguistics and NLP fields. While being consitently updated by its creators Paul Boersma and David Weenink of the University of Amsterdam, [its website](https://www.fon.hum.uva.nl/praat/) is a mid-90s time capsule. Praat has a broad functionality, and supports spectrogram visualization, pitch tracking, formant analysis, and speech synthesis.

[Parselmouth](https://parselmouth.readthedocs.io/en/stable/) is a Python library built specifically to interact with Praat and audio files, creating a seamless development environment to utilize the power of Python and Praat. The goal of Parselmouth is specifically to provide a "Pythonic" interface to Praat, rather than using the native C-based script that the software uses. 

In this tutorial I hope to give you a very brief look into the capabilities of Praat, and some strategies to use Parselmouth for NLP tasks.

## Requirements
In order to run the examples in this tutorial, you will need Python 3 or higher, Praat Version 6.03.09, and the Parsertongue Python Library installed on your machine. These were run within Jupyter notebooks in a native Linux environent (Ubuntu 20.04.6). All supplemental software referenced here is licensed under the GNU General Public License V3, and is open source. 

## Loading a Sound file Using Parselmouth
Parselmouth makes it very easy to load a sound file for analysis.

``` 
import parselmouth as pm

sound = pm.Sound(Test.wav) 
```
From there, the file is ready to be read and manipulated using Praat.

The included test file includes a short clip of me saying "This is a sentence I am recording for Praat." Feel free to use any audio file you'd like, but know that these programs are happiest when working with .wav files.

Here is an example of a variable made using Parselmouth's pitch method, which tracks information about the pitch over the duration of the audio file.
```
pitch = sound.to_pitch()
print(pitch)
```
```
Object type: Pitch
Object name: <no name>
Date: Sat May  6 13:22:20 2023

Time domain:
   Start time: 0 seconds
   End time: 2.72115625 seconds
   Total duration: 2.72115625 seconds
Time sampling:
   Number of frames: 269 (189 voiced)
   Time step: 0.01 seconds
   First frame centred at: 0.020578125 seconds
Ceiling at: 600 Hz

Estimated quantiles:
   10% = 83.4895308 Hz = 77.7288574 Mel = -3.12399353 semitones above 100 Hz = 2.56696611 ERB
   16% = 84.669073 Hz = 78.751992 Mel = -2.88111601 semitones above 100 Hz = 2.59933851 ERB
   50% = 96.1593296 Hz = 88.6203095 Mel = -0.67801508 semitones above 100 Hz = 2.90961122 ERB
   84% = 503.023015 Hz = 357.22615 Mel = 27.9674929 semitones above 100 Hz = 10.3309131 ERB
   90% = 549.888459 Hz = 381.175176 Mel = 29.5096681 semitones above 100 Hz = 10.920997 ERB
Estimated spreading:
   84%-median = 407.9 Hz = 269.3 Mel = 28.72 semitones = 7.441 ERB
   median-16% = 11.52 Hz = 9.895 Mel = 2.209 semitones = 0.3111 ERB
   90%-10% = 467.6 Hz = 304.3 Mel = 32.72 semitones = 8.376 ERB

Minimum 75.236856 Hz = 70.5167481 Mel = -4.92586238 semitones above 100 Hz = 2.33766131 ERB
Maximum 592.814486 Hz = 402.232088 Mel = 30.8109684 semitones above 100 Hz = 11.4324657 ERB
Range 517.6 Hz = 331.715339 Mel = 35.74 semitones = 9.095 ERB
Average: 214.312673 Hz = 165.69488 Mel = 7.19169562 semitones above 100 Hz = 5.02663409 ERB
Standard deviation: 191.9 Hz = 125.9 Mel = 13.61 semitones = 3.478 ERB

Mean absolute slope: 2470 Hz/s = 1580 Mel/s = 170.6 semitones/s = 43.31 ERB/s
Mean absolute slope without octave jumps: 51.9 semitones/s
```
## Simple plots Using Matplotlib

We can use other common libraries like Matplotlib to visualize the recording, with things like spectrograms:
```
import parselmouth
import matplotlib.pyplot as plt

voice = parselmouth.Sound("Test.wav")

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

plt.plot(pitch_times, pitch_values, linewidth=2, color='black')
plt.xlabel("Time (s)")
plt.ylabel("Pitch (Hz)")
plt.show()
```
![Output](https://raw.githubusercontent.com/vinicelli/vinicelli.github.io/main/Pitch_Contour.png)

...or simple waveform displays:
```
waveform = sound.values.T

duration = sound.duration

time = sound.xmin + np.arange(len(waveform)) / float(sound.sampling_frequency)

plt.plot(time, waveform, color='black')
plt.xlabel('Time (s)')
plt.ylabel('Amplitude')
plt.xlim([0, duration])
plt.show()
```
![Output](https://raw.githubusercontent.com/vinicelli/vinicelli.github.io/main/Waveform.png)

We can also average measurements like pitch and formants across the span of a sound file. 
```
import numpy as np
pitch_values = pitch.selected_array['frequency']
mean_pitch = np.mean(pitch_values)

print(mean_pitch)
```
```
150.57656235439302
```
While averaging an entire sentence may not be very useful, averaging many individual waveform files split into single words or phonemes can be used as features for classifying the demographics of the speaker. The resulting values can be used for recognizing speech emotion, the identity of the speaker, or even regional accents.

 

