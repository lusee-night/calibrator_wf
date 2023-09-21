# Calibrator waveform for CS-4

The repository contains:
 * A calibrator waveform as per Appendix A requirement of the CLPS CS-4 mission
 * Software to reproducibibly create this waveform

# Waveform

Waveforms are located in the `waveforms` directory. The latest waveform is `waveforms/calibrator_231001.bin`.
It can also be found at zenodo [here](https://zenodo.org/record/).
It is stored as a set of small endian 16 bit signed integers. It can be read into python as
```
(fix)
```


# Generation script

The generation script is a jupyter notebook `generate_waveform.ipynb`. It contains a few self-checks to ensure the waveform generated is the same as in the tagged versions of this repo. 
It should run in any recent python environemnt, but to guarantee full reproducibility please use the python from [lusee/lusee-night-unity-luseepy:1.0](https://hub.docker.com/layers/lusee/lusee-night-unity-luseepy/1.0/images/sha256-a9fb9b47e1f300025995fc35c917ba865725285fa52a61c58920540a25439559?context=explore) Docker distribution.

