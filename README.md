# CookyTape VST



CookyTape is an analog **Saturation/Compression** emulation of a magnetic tape recorder built using [JUCE Framework](https://github.com/juce-framework/JUCE).

It recreates the behavior of the hardware circuitry when it is subjected to work in the non-linear or saturation zone.

On the one hand, an analog bus emulation, and on the other, the typical treble compression in the recording of magnetic tape.

In recording with tape recorders, a high-frequency compensating circuit is included in the recording step that amplifies them (pre-emphasis), to subsequently attenuate them in the reproduction (de-emphasis). For this reason when we force the circuitry with greater sound signal pitch, it creates a compression of the high frequencies from 2,250 Hz, creating the typical sound stuck and crushed, producing a greater sustain and a better insertion in the mix.

Its adjustment is always done with an incoming signal close to (0 dB), easy to get on any bus or group, such as; the group of percussion, of synths, or in the channel of any instrument.



## Building from source
To build from source, you must have CMake installed.
```
$ git clone --recursive https://github.com/cid2rrrr/CookyTape
$ cd CookyTape
$ cmake -Bbuild
$ cmake --build build --config Release
```


## Neural Network Modeling

Behind the scenes This Plug-in uses [RTNeural](https://github.com/jatinchowdhury18/RTNeural).

For the training data, A total of approximately 11 minutes of voices, ambient sounds, musical performances, and special sound effects were used. The audio was passed through the device at 11 steps for the full range of the fader (0 to 10), resulting in 11 samples of 1 min each. In the neural network version of the emulation, a recurrent neural network is used.



## License
The code in this repository is licensed under
the **BSD 3-clause** license.
