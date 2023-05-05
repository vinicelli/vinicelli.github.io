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