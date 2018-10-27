---
layout: post
author: joshua
categories: [tutorials]
tags: [gnu radio, sdr]
excerpt_separator: <!--more-->
---

In this tutorial, a binary frequency shift keyed modulator and demodulator using GNU radio companion.

<!--more-->

# Binary Frequency Shift Keyed Modulation Using GNU Radio

## TODO:

1. Specify which values should be specified as variables
1. Specify any equations for, and involving, the variables

## Helpful Tips

1. Increase the working space in GNU Radio Companion
   1. Double click on the **Options** block
   1. Enter a new value for the **Canvas Size** option (e.g. *[2000,2000]*)

## FSK Modulation

### Block based modulation

1. Construct the flow diagram shown below to generate a test series of bits.
   ![BFSK Source Flow Diagram](/assets/img/BFSK_GNU_Radio_Source_Flow_Diagram.png)

   <center><b>Figure 1.</b> BFSK Source Flow Diagram</center>

1. Construct the BFSK modulator.
   ![BFSK Modulator Flow Diagram](/assets/img/BFSK_GNU_Radio_Modulator_Flow_Diagram.png)

   <center><b>Figure 2.</b> BFSK Modulator Flow Diagram</center>

1. Add the transmitter sink.
   ![BFSK Transmitter Flow Diagram](/assets/img/BFSK_GNU_Radio_Transmitter_Flow_Diagram.png)

   <center><b>Figure 3.</b> BFSK Transmitter Flow Diagram</center>

## FSK Demodulation

1. Add the receiver source block and a frequency sink
   ![BFSK Receiver Flow Diagram](/assets/img/BFSK_GNU_Radio_Receiver_Flow_Diagram.png)

   <center><b>Figure 4.</b> BFSK Receiver Flow Diagram</center>

1. Add the demodulator blocks
   ![BFSK Demodulator Flow Diagram](/assets/img/BFSK_GNU_Radio_Demodulator_Flow_Diagram.png)

   <center><b>Figure 5.</b> BFSK Demodulator Flow Diagram</center>

1. Add the decoder blocks
   ![BFSK Decoder Flow Diagram](/assets/img/BFSK_GNU_Radio_Decoder_Flow_Diagram.png)

   <center><b>Figure 6.</b> BFSK Decoder Flow Diagram</center>

## Sending Data

### Synchronization Sequence (Training Sequence)

This is sent to differentiate the message from noise and provides a signal for the receiver to "lock onto" and recognize the start of a new message.

Some general rules:

1. Shouldn't simply repeat

   Ex. 10101010

   If the first two bits of the training sequence are sampled from noise immediately prior to the start of the actual training sequence, the received message will start two bits earlier than it actually should.