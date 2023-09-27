# Calibrator waveform for CS-4

The repository contains:
 * A calibrator waveform as per Appendix A requirement of the CLPS CS-4 mission
 * Software to reproducibibly re-create this waveform (or see how it was created!)

Any questions regarding this waveform should be addressed to Anže Slosar (anze@bnl.gov).

# Waveform

Waveforms are located in the `waveforms` directory. The latest waveform is in [waveforms/calibrator_231001.bin](waveforms/calibrator_231001.bin).
It was also uploaded to zenodo [here](https://zenodo.org/record/8381205).
It is stored as a set of little-endian 16 bit signed integers. It can be read into python as
```
import numpy as np
filename = 'waveforms/calibrator_231001.bin'
wave = np.fromfile(filename, dtype=np.int16)
```
The md5sum for this file is `b5f829b5e2dbef213030a1761fa85b62`.

# Generation script

The generation script is a jupyter notebook [generate_waveform.ipynb](./generate_waveform.ipynb). It contains a few self-checks (seed, md5sum, etc) to ensure the waveform generated is the same as in the tagged versions of this repo. 

It should run in any recent python environment, but to guarantee full reproducibility please use the python from [lusee/lusee-night-unity-luseepy:1.0](https://hub.docker.com/layers/lusee/lusee-night-unity-luseepy/1.0/images/sha256-a9fb9b47e1f300025995fc35c917ba865725285fa52a61c58920540a25439559?context=explore) docker image.

The notebook contains three section:
 1. *Waveform generation.* In this section we generate the form with a fixed random seed. The form is 2048 samples long with unit amplitude and random phase Fourier coefficients on odd frequencies (corresponding to 50kHz, 150kHz, 250kHz, etc at 102.4MS/s sampling rate) and 0 for even frequencies. Such field has a particular symmetry: samples 1024-2047 are the same as samples 0-1024 multiplyed with -1. Note that this is *not* a Gaussian random field, since amplitudes are fixed to unity. In this section will also save the waveform as 16 bit signed integer binary file.

 2. *Waveform check.* In this section we reload the waveform and confirm that it has the expected statistical properties.

 3. *Generation of oversampled waveforms.* In this section we generate x16 oversampled waveforms. These can be replayed at 16 times the sampling rate to emulate various high-frequency aliasing:
  Three waveforms are provided:
    * *Band limited*: No aliasing beyoned the first Nyquist zone for the original sampling. This corresponds to an idealised DAC.
    * *Nearest neighbor interpolation* (NNI): Instantaneous DAC voltages correspond to the closest sample: i.e the DAC replays the voltage perfectly for the sampling time. This is an idealised picture of a realistic DAC.
    * *SOS* filtered NNI: low-pass filtered version of the above signal. This is likely the most realistic waveform, but note that real DAC can act in various ways.

