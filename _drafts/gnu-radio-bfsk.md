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

## BFSK Modulation Part 1: A Closed Loop System

A Closed system is an important step in the design and implementation of a software-designed radio communication system. It allows the engineer to test the system in a closed environment, eliminating many of the environmental variables that come with the actual implemenation like channel noise and frequency drift. In this part we will be developing a BFSK modulator and demodulator with configurable additive AWGN for the channel.

1. Create the variable blocks for this design.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Variable_Blocks.png" alt="BFSK Variable Blocks">
      <figcaption><b>Figure 1.1</b> BFSK Variable Blocks for a Closed Loop System</figcaption>
    </figure>
   </center>

1. Create the flow diagram for the bit source.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Source_Flow_Diagram.png" alt="BFSK Source Flow Diagram">
      <figcaption><b>Figure 1.2</b> BFSK Source Flow Diagram for a Closed Loop System</figcaption>
    </figure>
   </center>

1. Create the flow diagram for the modulator.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Modulator_Flow_Diagram.png" alt="BFSK Modulator Flow Diagram">
      <figcaption><b>Figure 1.3</b> BFSK Modulator Flow Diagram for a Closed System</figcaption>
    </figure>
   </center>

1. Create the flow diagram for the channel.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Channel_Flow_Diagram.png" alt="BFSK Channel Flow Diagram">
      <figcaption><b>Figure 1.4</b> BFSK Channel Flow Diagram for a Closed System</figcaption>
    </figure>
   </center>

1. Create the flow diagram for the demodulator and comparator.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Demodulator_Flow_Diagram.png" alt="BFSK Demodulator And Decoder Flow Diagram">
      <figcaption><b>Figure 1.5</b> BFSK Demodulator and Decoder for a Closed System</figcaption>
    </figure>
   </center>

## BFSK Modulation (Open System)

### Block based modulation

1. Construct the flow diagram shown below to generate a test series of bits.

   <center>
     <figure>
       <img src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Source_Flow_Diagram.png" alt="BFSK Source Flow Diagram">
       <figcaption><b>Figure 1.</b> BFSK Source Flow Diagram</figcaption>
     </figure>
   </center>

1. Construct the BFSK modulator.

   <center>
     <figure>
       <img src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Modulator_Flow_Diagram.png" alt="BFSK Modulator Flow Diagram">
       <figcaption><b>Figure 2.</b> BFSK Modulator Flow Diagram</figcaption>
     </figure>
   </center>

1. Add the transmitter sink.

   <center>
     <figure>
       <img alt="BFSK Transmitter Flow Diagram" src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Transmitter_Flow_Diagram.png">
       <figcaption><b>Figure 3.</b> BFSK Transmitter Flow Diagram</figcaption>
     </figure>
   </center>

## FSK Demodulation

1. Add the receiver source block and a frequency sink

   <center>
     <figure>
       <img alt="BFSK Receiver Flow Diagram" src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Receiver_Flow_Diagram.png">
       <figcaption><b>Figure 4.</b> BFSK Receiver Flow Diagram</figcaption>
     </figure>
   </center>

1. Add the demodulator blocks

   <center>
     <figure>
       <img alt="BFSK Demodulator Flow Diagram" src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Demodulator_Flow_Diagram.png">
       <figcaption><b>Figure 5.</b> BFSK Demodulator Flow Diagram</figcaption>
     </figure>
   </center>

## Sending Data

### Encoding



### Decoding

1. Add the decoder blocks

   <center>
     <figure>
       <img alt="BFSK Decoder Flow Diagram" src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Decoder_Flow_Diagram.png">
       <figcaption><b>Figure 6.</b> BFSK Decoder Flow Diagram</figcaption>
     </figure>
   </center>

### Synchronization Sequence (Training Sequence)

This is sent to differentiate the message from noise and provides a signal for the receiver to "lock onto" and recognize the start of a new message.

Some general rules:

1. Shouldn't simply repeat

   Ex. 10101010

   If the first two bits of the training sequence are sampled from noise immediately prior to the start of the actual training sequence, the received message will start two bits earlier than it actually should.