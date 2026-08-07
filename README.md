# Piezo Electric Research

## Impedance Matching Theory

As a piezo element acts like a capacitor in an electronic circuit, a typical recording setup acts like a highpass filter with the cutoff frequency determined by the capacitance of the piezo element and the resistance of the input. The formula for the cutoff frequency ($f$) is the following: 

$$
f = \frac{1/}{2 * pi * R * C}
$$

where $R$ is the Restistance at the input, and $C$ the capacitance of the piezo element. Using typical values of
$R = 10.000\, \Omega$
and
$C = 15\,\mathrm{nF}$
the cutoff frequency is about $1061\, \mathrm{Hz}$.

## Impedance Matching Circuit

|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| ![Impedance Matching Circuit Design by Richard Mudhar] (https://i0.wp.com/www.richardmudhar.com/blog/wp-content/uploads/2018/12/RM_piezo_amp-1.png)                  | ![Picture of self-built Circuit] (./assets/images/impedancematchingCircuit.jpg) |
| Impedance Matching Circuit Design by Richard Mudhar. See [here](https://www.richardmudhar.com/piezo-contact-microphone-hi-z-amplifier-low-noise-version/) [07.08.26] |
| Self-built circuit                                                                                                                                                   |
                                                                                                                                                    
                                                                                                                                                    
