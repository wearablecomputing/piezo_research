# Piezo Electric Research

## Impedance Matching Theory

As a piezo element acts like a capacitor in an electronic circuit, a typical recording setup acts like a highpass filter with the cutoff frequency determined by the capacitance of the piezo element and the resistance of the input. The formula for the cutoff frequency ($f$) is the following: 

$$
f = \frac{1}{2 \pi R C}
$$

where $R$ is the Restistance at the input, and $C$ the capacitance of the piezo element. Using typical values of
$R = 10.000\, \Omega$
and
$C = 15\,\mathrm{nF}$
the cutoff frequency is 
$\approx 1061\, \mathrm{Hz}$

To counteract the filtering of low frequency content while recording with piezo elements, it is required to match the input resistance to the capacitance of the piezo. Having a large enough input resistance, the cutoff frequency is lowered until irrelevant for the individual setup. A input impedance of $1\mathrm{M\Omega}$ results in a cutoff frequency of

$$
f = \frac{1}{2 pi (1 \times 10^{6}\,\Omega) (15 \times 10^{-9}\,\mathrm{F})} \approx 11\, \mathrm{Hz}
$$

which is below human hearing threshhold. To match the input impedance to the load of the piezo a pre-amplifying circuit is needed.

## Impedance Matching Circuit
| Circuit Schematic by Richard Mudhar. See [here](https://www.richardmudhar.com/piezo-contact-microphone-hi-z-amplifier-low-noise-version/) | Self-built Circuit |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| ![Impedance Matching Circuit Design by Richard Mudhar](https://i0.wp.com/www.richardmudhar.com/blog/wp-content/uploads/2018/12/RM_piezo_amp-1.png)                  | ![Picture of self-built Circuit](./assets/images/impedancematchingCircuit.webp) |
                                                                                                                                                    
### Usage of circuit

#### Connecting the Circuit
The input signal is connected on the left mini-jack (connected to the yellow wire).
The output signal is connected on the right mini-jack.
Power is connected through the blue terminal block, according to the polarity signs written on the terminal block. (Only 2 of 3 inputs are used).

Connect the piezo element(-s) to a stereo aux cable (TRS) and the cable to the input of the circuit. Be aware of [the following chapter](#important-signal-characteristics) while connecting the piezos.
Connect a stereo aux cable from the output of the circuit to any type of stereo line-level input of your device or split the stereo signal into two mono line-level inputs.

#### **IMPORTANT** Signal Characteristics

The circuit is built with a comparison between impedance-matched and not impedance-matched signals in mind. Therefore it accepts a stereo signal, where the left input of the signal is routed directly to the output and therefore **not amplified**, while the right signal goes through the circuit and is therefore **amplified**. Be aware to use a stereo (TRS) cable and connect the piezo to the right channel (Ring) for the impedance matched input. In hindsight it would have been smarter to have the left input connected to the circuit and the right input as throughput, so mono cables (TS) would also work, but that will probably be changed in the future.

## Measurements

|                       | Absolute dB Reference | Relative dB Reference (individual normalization) |
|-----------------------|-----------------------|--------------------------------------------------|
| Not impedance matched | ![Spectrogram of not impedance matched input with absolute values](./assets/images/NoAmpPreFilterAbsolute.webp)                      | ![Spectrogram of not impedance matched input with normalizedValues](./assets/images/NoAmpPreFilterNormalized.webp)                                                 |
| impedance matched | ![Spectrogram of impedance matched input with absolute values](./assets/images/AmpPreFilterAbsolute.webp)                      | ![Spectrogram of impedance matched input with normalizedValues](./assets/images/AmpPreFilterNormalized.webp)                                                 |

### Measurement Setup
The measurements where done with piezo elements with about $20\, \mathrm{nF}$ capacitance on inputs 0 and 1 on the [bela gem multi](https://bela.io/products/bela-gem-stereo-and-multi/) which have an adjustable input impedance between $2.5\, \mathrm{k\Omega}$ and $20\, \mathrm{k\Omega}$ according to [here](https://forum.bela.io/d/7307-audio-input-impedance).
The left column shows absolute values of the two signals (with the same dB reference) to compare overall signal strength while the right column shows the normalized signals (with individual dB references) to compare the signal-to-noise ratio.

The signal source comes from manual mechanical excitement of the piezo elements by hitting a wodden plate, with the piezos mounted on.

### Comparison

#### Signal Strength
It is clearly visible how the impedance matched input delivers far more overall signal-strength than without impedance matching, which is probably based on the amplifying function of the impedance matching circuit. This ultimately results in a better signal-to-noise ratio for the impedance-matched signal, as analog amplification only amplifies the signal while digital amplification everything that the input picks up.

#### 50 Hz Bump

However due to the high impedance of the circuit, the piezo element and especially the cabling from the piezo element to the amplification circuit is very susceptible to electromagnetic fields and basically acts as an antenna. This can be seen in the spectrogram at the $50\, \mathrm{Hz}$ mark which is picked up strongly by the circuit (especially compared to the actual signal).

#### Expected High Pass Filtering

It is really odd, that there is only a slight increase in lower frequency content with the impedance matched input. The absolute values show a significant difference in strength between the matched and non-matched signal, but looking at the right column, which shows the normalized signals, a visible decrease in lower frequency content for the not impedance matched signal is expected. A slight decrease can only be seen at around $70\, \mathrm{Hz}$, which does not align with the expected $\approx 1000\, \mathrm{Hz}$ cutoff. This will require further investigation.


## Multi-Piezo Setup

### Theoretical Considerations

Using multiple piezo-elements as *one* sensor runs the risk of interference. This is due to the different traveling times of the mechanical force in the specific medium. The travel time of the force is probably a characteristic of the specific material used as the surface for the piezos. Using multiple piezos therefore makes it more challenging regarding exact on-set detection.

If all piezos are connected in parrallel to the input, the detected signal resembles the sum of the individual piezos. This sum is susceptible to interference. If for example two piezos are placed in such a way, that the latency between the piezos (determined by the distance between them relative to the point of excitement) is exactly half the duration of the period of the frequency of the excitement, the individual piezos detect the signal exactly half a phase apart. Summing them at the input then results in massive phase cancelation and no reliable detection of the specific frequency. Therefore it exists a relationship between the placements of the piezos, the material of the surface and the frequencies that get canceled or amplified by the phase interference. To test this, following measurements where made.

### Measurement Setup

![Image of Measurement Setup with four piezos, labeled C1 to C4 and one Exciter, labeled E1](./assets/images/latencyMeasurementSetup.webp)

To measure an examplatory latency as a proof-of-theory 4 piezos labeled *C1** to *C4* and one Exciter labeled *E1* were placed on a wooden board. The piezos are roughly arranged to resemble the following 5 cm grid:

| *C1* |      | *C4* |
|------|------|------|
|      | ***C2*** | ***C3*** |
|      | ***E1*** |      |

To test the latency, the exciter played Impulses spaced 1 second apart. Each piezo was recorded as a seperate audio channel to mitigate any interference between the piezo signals.
The following image shows the latency between the individual channels. The top-most waveform resembles the recording of *C1* and then continuing down so that the lowest resemembles *C4*.

### Latency

![Waveform of four-channel piezo recording showing the latency between each channel](./assets/images/latencyMeasurementRecording.webp)

The Image shows that *C2* is the first piezo to detect the Impulse, followed by *C3*, *C4* and almost at the same time*C1*. This order proofs that the latency is dependent on the distance of the piezos to the exciter as *C2* is closest to *E1* and *C1* furthest apart. The recordings were done at a Samplerate of $f = 44100\, \mathrm{Hz}$ and the dots in the waveforms resemble the individual sample. To get a grasp of the amount of latency we can count the samples between the waveforms. *C3* detects the impulse $\approx 5$ after *C2* which results in a latency of $\Delta t \approx \frac{5}{44100 \, \mathrm{Hz}} \approx 1.13 \times 10^{-4}$ so about $\frac{1}{10}$ of a Millisecond.

### Interference

More importantly to consider is the characteristic of each waveform. As shown in the figure above, the waveform looks quite differently between the individual channels. This is probably due to force reflections inside of the wooden plate. For example *C2* and *C3* start with a small dip before the on-set rise of the signal while *C1* and *C4* behave almost exactly opposite, starting with a small rise before a big dip. It almost looks like the piezos *C1* and *C4* were differently polarized to *C2* and *C3* which should not have been the case.

![Waveform of four-channel piezo recording with the summed waveform included](./assets/images/latencyMeasurementRecordingSum.webp)

This figure includes the digital sum of the 4 piezo channels, which should approximately resemble the analog sum, if the piezos would have been connected in parallel at one input. The summed signal clearly has its one unique waveform compared to the individual piezos.

### Reflections

The [latency](#latency) of $\frac{1}{10}$ of a Millisecond is for most use-cases probably irrelevant, but still puts a limit on the precision of on-set-detection times using a piezo-electric sensing setup. The more important aspect to consider is the interference. If the presumed reason for the different wave form characteristics of the piezos formulated in Chapter [Interference](#interference) is correct, the risk of interference is twofold. On the one hand, there is a risk of interference in the electrical domain, when summing individual piezos with specific latencies (as described in Chapter [Theoretical Considerations](#theoretical-considerations)). On the other hand there is a risk of interference inside of the surface material itself. If the force reflects at the edges of the excited surface there will be a multitude of interference patterns influencing the force that each piezo element detects.
Still the question remains, if this is relevant for using piezo-elements to detect physical excitement, as the signal source there resembles a noise burst without any specific important frequency which one might want to detect. Having such a noisy (desired) signal relieves a lot of the pressure coming from interference considerations.

## TODO

- [ ] Add sound-recordings to repo 
