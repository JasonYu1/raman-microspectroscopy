# Spontaneous Raman Microspectroscopy

A platform for combining spontaneous Raman spectroscopy with automated optical microscopy. Built on top of [`pymmcore-plus`](https://pymmcore-plus.github.io/pymmcore-plus/), this project extends conventional multi-dimensional acquisition (MDA) workflows with synchronized Raman acquisition, enabling label-free chemical imaging alongside standard fluorescence and brightfield microscopy.

## Overview

Spontaneous Raman microspectroscopy provides label-free, chemically specific information about a sample, but integrating it into a modern automated microscopy workflow is non-trivial: the Raman acquisition has to be coordinated with stage movement, illumination, shutters, and the rest of the imaging pipeline. This project provides a set of Python packages that handle that coordination, so Raman measurements can be acquired as part of an MDA experiment in the same way one would acquire a z-stack or a multi-channel image.

Beyond basic acquisition, the platform supports user-defined autofocusing metrics, allowing the focus criterion to be tailored to the sample and modality at hand rather than relying on a fixed algorithm. It also performs on-the-fly segmentation and tracking of objects in the field of view, and uses the resulting positions to update the Raman laser aiming in real time � making it possible to follow and interrogate moving or growing targets, such as live cells, throughout the course of an experiment.

The project is organized as three packages, each handling a separate concern.

## Components

### [cns_control](https://github.com/JasonYu1/cns-control)
Hardware-specific control code for the spontaneous Raman microscope at the Harvard Center for Nanoscale Systems (CNS) facility. This package wraps the particular lasers, shutters, detectors, and DAQ devices on that instrument. **Users running on different hardware should adapt or replace this package to match their own setup** � it is intended as a reference implementation rather than a general-purpose driver layer.

### [raman_control](https://github.com/JasonYu1/raman-control)
General-purpose control of Raman-related devices during a microscopy experiment, independent of any specific facility. Handles device abstraction, acquisition timing, and the lower-level coordination needed to fire a Raman measurement at the right moment in an imaging sequence. Forked from [ianhi/raman-control](https://github.com/ianhi/raman-control).

### [raman_mda_engine](https://github.com/JasonYu1/raman-mda-engine)
A custom MDA engine for `pymmcore-plus` that incorporates Raman acquisition alongside conventional MDA dimensions (time, position, channel, z). This is what makes Raman a first-class citizen in an automated experiment: you define an MDA sequence the usual way, and the engine takes care of interleaving Raman measurements at each step. Forked from [ianhi/raman-mda-engine](https://github.com/ianhi/raman-mda-engine).

## Installation

We recommend a fresh conda environment:

```bash
conda create -n micro-control python=3.10
conda activate micro-control

pip install git+https://github.com/JasonYu1/cns_control.git
pip install git+https://github.com/JasonYu1/raman-control.git
pip install git+https://github.com/JasonYu1/raman-mda-engine.git
```

Note that `cns_control` is tailored to the Harvard CNS instrument. If you are running on different hardware, you will likely want to fork it and modify the device definitions to match your own system, rather than installing it as-is.

## Example notebook

See [`example_notebook.ipynb`](notebooks/example_notebook.ipynb) for a walkthrough of a typical acquisition.

## License

Each component is licensed under BSD-3-Clause. See the individual repositories for details.