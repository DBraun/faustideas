
## Submitting a Google Summer of Code project

The application process for GSoC consists of next steps:

- become acquainted with the [Faust language and ecosystem](https://faust.grame.fr)
- join to [Faust Slack channel](https://faustaudio.slack.com) and/or #faust channel on [The Audio Programmer](https://theaudioprogrammer.com/community) discord
- search mentor for chosen project in mailing list discuss
- get invite to chosen project in Faust github organization
- submit the application/proposal including all requirements at the Google Summer of Code Site

# Faust ideas

This repository hosts the "TODO/ideas list" for the [Faust programming 
language](http://faust.grame.fr).

## Conventions

* Items are placed at the bottom of the file and separated by `---`. 
* Items can have a person associated to them by declaring `* Currently 
addressed by: xxx`. If no-one is currently working on the item, replace 
`xxx` by `nil`. 
* Items are ordered by priority and are also listed in the [List](#list) 
section below. 
* Items can be commented by adding subsections, pictures, etc. 

## List

- [Implement Jonathan Abel's Modal Reverb](#implement*jonathan-abels*modal-reverb)
- [Improved UI Declarations](#improved-ui-declarations)
- [TensorFlow Support](#tensorflow-support)
- [Improved Linear Algebra Support](#improved-linear-algebra-support)
- [Finish the DX7 Implementation](#finish-the-dx7-implementation)
- [Trigonometric simplifications](#trigonometric-simplifications)
- [WebAssembly specific optimisations](#webassembly-specific-optimisations)
- [Testing tools on the Web](#testing-tools-on-the-web)
- [Integration in Bespoke](#integration-in-bespoke)
- [Integration in Cables.jl](#integration-in-cablesjl)
- [Integration in HISE](#integration-in-hise)
- [Improve faust2audiokit](#improve-faust2audiokit)
- [Faust programming by examples](#faust-programming-by-examples)
- [Graphical language built on top of the signal API](#graphical-language-built-on-top-of-the-signal-api)


## Implement Jonathan Abel's Modal Reverb

* Currently addressed by: Romain and Yann

This should be done as part of the Longyou grottoes project. The "final goal" would be to create an interactive website where users can process the sound of
their microphone to apply the acoustics of this ancient space. Modal reverb would allow to interpolate between IRs and change some of the parameters of
the space in real-time. It'd be nice if this could be reproducible so we need to think about a way to nicely generate these reverbs from an impulse response.
This tool could be similar to `mesh2faust` or could come as part of a toolkit in matlab/octave/pyhton, etc.  


## Improved UI Declarations

* Currently addressed by: Romain and Yann

Essentially allow for specific UI elements to have metadatas associated to them outside of their declaration. As part of that, we want to implement a system to further customize UI elements. 

### Potential Implementation

Several approaches are being considered to further customize UI elements. The first one would consist of being able to declare a "CSS" allowing for the use
of CSS code. Another approach (more generic and not limited to the web) would allow for the declaration of UI-specific metadata inspired by CSS. 

```
declare UI "
  synth{
    background-color: blue;
  }
  synth/freq{
    tooltip: Frequency parameter of the synth;
    width: 70%;
  }
  synth/gain{
    style: knob;
    tooltip: Gain parameter of the synth;
    width: 70%;
  }
";

f = hslider("freq",400,50,1000,0.1);
g = hslider("gain",0.5,0,1,0.01);
process = hgroup("synth",os.sawtooth(f)*g);

```

Of course, it would still be possible to declare metadatas within the UI declaration (this system would be fully backward compatible). Internally, we'd have to parse the metadata and create a corpus of supported CSS metadatas knowing that interfaces would be based on a specific kind of layout (e.g., grid layout). Once again, another option would be to allow to specify "pure CSS" giving access to all the CSS features without having to do some reformatting. 

---

## TensorFlow Support

* Currently addressed by: nil

Be able to generate TensorFlow code with Faust.

---

## Improved Linear Algebra Support

* Currently addressed by: nil

Linear algebra operations are currently poorly supported in Faust. Having a way to conveniently express matrices would improvement. As part of that, linear algebra/matrix operations (e.g., inversion, multiplication, determinant, etc.) primitives could be added to the language.

### Potential Implementation

Matrices could be expressed using the Faust-multirate `vectorize` primitives
by creating vectors of vectors.

It would be interesting to try to implement matrix operations from scratch in  Faust. Although it might be hard and not so optimized, thus a more pragmatic solution would be to implement them as primitives. That would be a fair amount of work as this would imply that the corresponding code for each language supported by Faust would have to be supported. 

---

## Finish the DX7 Implementation

Essentially, finish `dx7.lib`. It might be worth looking at these elements to
make this happen:

* <https://webaudiomodules.org/demos/wasm/dx7.html>
* <https://github.com/everythingwillbetakenaway/DX7-Supercollider>

---

## Trigonometric simplifications

* Currently addressed by: nil (suggested by [Pierre Leconte](https://github.com/grame-cncm/faust/issues/59))

For some applications, trigonometric functions (spherical harmonics) are used, and depending on the algorithm, the output formula could be very complicated. However, in a lot of cases, trigonometric identities could help to simplify drastically the expressions.

---

## WebAssembly specific optimisations

* Currently addressed by: Stéphane and Yann

To run as fast as possible and approch native code performances as much as possible, WebAssembly code requires some specific optimisations, like: memory access (index precomputation as much as possible...), delay lines handling, struct/stack variables access...etc. We have started an informal collaboration with Mozilla engineers (Benjamin Bouvier) to work on this subject.

---

## Testing tools on the Web

* Currently addressed by: nil

Faust distribution already contains some testing tools, like `faust2plot` or `faust2octave`.etc. It would be great to have them running in a Web page (or some extension of the same idea). For signal generators/processors, several output formats (oscilloscope, spectrogramme...), and for processors several calibrated input signals (dirac impulse, ramp, sinusoide..) would be available.

---

## Integration in Bespoke

* Currently addressed by: nil

[Bespoke](https://www.bespokesynth.com) is a modular DAW for Mac, Windows, and Linux. Bespoke is a software modular synthesizer. It contains a bunch of modules, which you can connect together to create sounds. 
Benedict Gaster work (https://github.com/bgaster/BespokeSynth) uses WebAssembly as the compilation target language, and has already done the BespokeSynth integration. So possibly this work could be directly merged.
An more ambitious approach would be to directly embed the Faust compiler (using the libfaust + LLVM JIT way) with would even produce faster code. This is currently [discussed here](https://github.com/BespokeSynth/BespokeSynth/issues/317).

---

## Integration in Cables.jl

[Cables](https://cables.gl) is a tool for creating beautiful interactive content. With an easy to navigate interface and real time visuals, it allows for rapid prototyping and fast adjustments. You are provided with a set of operators, such as mathematical functions, shapes, materials and post processing effects. Connect these to each other with virtual cables to create the experience you have in mind.
Easily export your piece of work at any time. Embed it into your website or use it for any kind of creative installation.

The project would be to integrate the [Faust Web Audio Library](https://www.npmjs.com/package/@grame/libfaust) to dynamically compile and run Faust DSP programs in Cables.jl.

---

## Integration in HISE

[HISE](http://hise.audio) is a cross-platform open source audio application for building virtual instruments. It emphasizes on sampling, but includes some basic synthesis features for making hybrid instruments as well as audio effects. You can export the instruments as VST/AU/AAX plugins or as standalone application for Windows / macOS or iOS.

The project would be to integrate the Faust compiler (using the libfaust + LLVM JIT way) into HISE for live editing and then used to generate C++ at compile time. This would allow for much more complex effects development without need to delve into C++ DSP.
This is currently [discussed here](https://github.com/christophhart/HISE/issues/224).

---

## Improve faust2audiokit

The [faust2audiokit](https://github.com/grame-cncm/faust/tree/master-dev/architecture/audiokit) tool transforms a Faust DSP program into a fully working AudioKit node. The result can be a monophonic DSP or a MIDI controllable polyphonic one (when the DSP describes an instrument, following the freq, gain, gate parameter naming convention).

The project consist in improving and finishing the tool.

---

## Faust programming by examples

The objective is to develop a new approach to Faust programming, not textual or graphical, but based on DAW-like examples. This programming principle is analogue to the one described in the article *"real time composition in Elody"* (https://hal.archives-ouvertes.fr/hal-02158910/document). This approach is based on the idea of manipulating and editing virtual "audio files" which represent the real time audio inputs and outputs. 

To take a simple monophonic example, let's call these two virtual audio files `INPUT` and `OUTPUT`. Let's note `t:file` the fact of placing in the DAW a file `file`at time `t` in seconds and `t:file*0.75` the fact of placing in the DAW a file at time `t` but also controlling its sound level. So the DAW construction `{0:INPUT, 1:OUTPUT*0.75}` corresponds to a realtime echo whose Faust translation is `process = + ~ (@(ma.SR):*(0.75));`. The project consists in exploring this model and see how standard DAW editing actions can be translated in Faust DSP programs. 

---

## Graphical language built on top of the signal API

The [signal API](https://faustdoc.grame.fr/tutorials/signal-api/) opens an intermediate access inside the Faust compilation chain. Generating complex expressions by directly using it can quickly become really tricky and unpracticable. So a language created on top of the signal API is usually needed. This is exactly what the Block Diagram Algebra is all about, and the entire Faust language itself.

But some other approaches can possibly be tested. The [Elementary audio language](https://www.elementary.audio) for instance is built over a [similar signal](https://docs.elementary.audio/guides/making_sound) language and uses JavaScript as the upper layer language to help create complex signal graphs programmatically. The project consits in exploring how a graphical language could be built on top of the signal API and be compiled to efficient code using the lower part of the Faust compilation chain. 
