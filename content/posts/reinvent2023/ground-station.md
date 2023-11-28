---
title: "AES303 - Ground Station SDRs in the Cloud"
date: 2023-11-27T10:00:00-07:00
draft: false
---

## Software Defined Radios

Components conventially implemented in hardware, now in software, including things like modems.
* demodulation
* decoding
* frame and packet processing
* reporting and analytics

SDRs require hardware acceleration because signal processing is embarassingly parallel, e.g. FGPAs, GPUs, or specialized instruction sets. EC2 accelerated instances are available for this.

## FPGAs for SDR

After an ADC converts an analog signal to digital, the signal needs to be processed (highly parallelizable):
* Enter FPGAs
* Custom programmable
* Highly parallelized compute
* High bus bandwidth

...transitioned to a demo. This stuff is a bit over my head and out of my wheelhouse.