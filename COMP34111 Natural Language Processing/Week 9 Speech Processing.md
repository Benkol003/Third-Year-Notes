Speech Processing tasks:

- Speech Coding - i.e. taking a raw waveform and encoding it as binary data, e.g. compressing to MP3
- Speech Synthesis - i.e. text-to-speech - have a model to make synthesized speech sound more natural / less robotic

![](misc/Pasted%20image%2020240118181508.png)

- Speech Recognition - Converting encoded audio into text (will be covering this here). Also called automatic speech recognition, computer speech recognition, Speech-To-Text

## Speech Recognition

Stages:
- Audio input (i.e. speech encoding from microphone input)
- Feature extraction: analysing audio for relevant data / features e.g. pitch.
- Pattern Matching: an *acoustic model*, i.e. trained machine learning model, is used to identify phonemes (or words) spoken from the audio
- Language Modelling: 
1. Naïve construction - acoustic model gives a deterministic answer for what was spoken
2. Use probabilistic techniques to identify what words most likely to have been spoken from the output of the acoustic model - there may be ambiguity in audio especially with noise. Typically use a hidden markov model, each speech 'unit' is a hidden state.
- Output Generation: i.e. construct the sequence of words, including inserting punctuation; or may pass off to an AI e.g. in the case of Siri/google assistant to perform actions. Or for Duolingo, check that the spoken audio matches the expected words.

Deep learning has been increasingly used for all of these steps, & for more realistic speech synthesis aswell.



### Human Speech Production

![](misc/Pasted%20image%2020240118183040.png)

Speech is produced by vocal cords vibrating from exhalation of air - vocal chord flaps repeatedly open/close at the audio frequency to release *glottal pulses* of air to create the periodic pressure difference for sound; muscular control of the vocal chords changes sound that is produced.


#### Voice Parameters for feature Extraction

- Pitch-frequency (of the vocal chords)
- Jitter / variation of the frequency
- Shimmer: Variation in audio volume / glottal pulse amplitude
- Tremor: rhythmic combined jitter and/or shimmer
- Harmonic noise ratio (HNR): Amount of noise relative to the harmonic components (i.e. signal noise), a person who has lost their voice / hoarse voice, e.g. damage to vocal chords, will have a higher HNR.

#### Pulse-code modulation
The amplitude axis is quantised with a uniform scale (linear for LPCM, regular PCM uses A-law or $\mu$-law - we typically expect signals to be rapidly changing at origin 0 but not at peak amplitudes), both both positive/negative amplitudes / pressures. The level is then bit-encoded.

![](misc/Pasted%20image%2020240118201343.png)

Here level 7 would be amplitude 0.

A sample rate, e.g. 48Khz determines the time interval between samples to use before we read the PCM level for the signal.

The highest possible reproducible frequency, known as *Nyquist frequency*, is half the sample frequency e.g. 24Khz.


### Voiced / Unvoiced Phone

![](misc/Pasted%20image%2020240118201801.png)

- Voiced phones are produced by vocal chords, e.g. *aaaaaa*.
- Unvoiced phone are produced by the mouth / lips (i.e. not the vocal chords), such as *ssssssss*.

You can see that unvoiced phones are higher frequency & more noisy.

### GRBAS voice quality assessment

Medical scale to assess audibility of voice:

![](misc/Pasted%20image%2020240118202338.png)

With following scale:

![](misc/Pasted%20image%2020240118202403.png)

#### Machine Learning model for GRBAS

General model:

Extract data features that can be used in a machine learning regression model to produce a GRBAS score.

![](misc/Pasted%20image%2020240118202605.png)

The dataset will be pre-recorded voice samples with GRBAS scores from clinicians using GRBAS presentation & scoring package (GPSP).

Extracting the data features is done using *digital signal processing* techniques (i.e. can be calculated from audio waveform). Namely pitch-frequency, jitter & shimmer.

- [Praat](https://www.fon.hum.uva.nl/praat/) - free phonetics & speech analysis software.
- ADSV - Commercial software that calculates its own features/parameters - *Cepstral/Spectral Index of Dysphonia*; extracts up to 11 parameters such as these:

![](misc/Pasted%20image%2020240118203319.png)


#### Cepstral Measures
Time based measures such as jitter, shimmer not that accurate for measuring voice quality; unvoiced segments & intonation can inflate these values, & voiced segments are usually short in connected speech.

cepstral analysis analyses periodic structures of frequency spectra by performing inverse fourier transform, to calculate the *cepstrum*.

 ADSV uses both spectral & cepstral analysis to generate a score.
