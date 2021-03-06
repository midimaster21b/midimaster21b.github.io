---
layout: post
author: joshua
categories: [tutorials]
tags: [gnu radio, sdr]
excerpt_separator: <!--more-->
---

In this tutorial, a binary frequency shift keyed modulator and demodulator using GNU radio companion.

<!--more-->

## Binary Frequency Shift Keyed Modulation Using GNU Radio

## TODO:

1. Specify which values should be specified as variables
1. Specify any equations for, and involving, the variables
1. Fix Part 2, Figure 1 (BPSK -> BFSK)
1. Remove unnecessary/unused blocks (especially GUI elements)
1. Add variable blocks for open loop system Tx and Rx
1. Create a table of values for figure 1.1

## Helpful Tips

1. Increase the working space in GNU Radio Companion
   1. Double click on the **Options** block
   1. Enter a new value for the **Canvas Size** option as *[width, height]* (e.g. *[2000,2000]* )
1. Sometimes a parameter requires an int to be supplied you need to perform division in the equation
   1. Cast the value to an int (e.g. `10 / 3` => `int(10 / 3)` or `YOUR EQUATION HERE` => `int(YOUR EQUATION HERE)`)

## BFSK Modulation Part 1: A Closed Loop System

A Closed system is an important step in the design and implementation of a software-designed radio communication system. It allows the engineer to test the system in a closed environment, eliminating many of the environmental variables that come with the actual implemenation like channel noise and frequency drift. In this part we will be developing a BFSK modulator and demodulator with configurable additive AWGN for the channel.

1. Create the variable blocks for this design.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Variable_Blocks.png" alt="BFSK Variable Blocks">
      <figcaption><b>Figure 1.1</b> BFSK Variable Blocks for a Closed Loop System</figcaption>
    </figure>
   </center>

   The value for the variable *quad_demod_gain* should be **-samp_rate/(2*math.pi*bandwidth/8.0)**. All other variables are values that can be copied from the figure above.

1. Create the flow diagram for the bit source.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Source_Flow_Diagram.png" alt="BFSK Source Flow Diagram">
      <figcaption><b>Figure 1.2</b> BFSK Source Flow Diagram for a Closed Loop System</figcaption>
    </figure>
   </center>

   The repeat block repeats the byte received from the source for *interpolation* samples. **Determine an appropriate equation for the interpolation value of the repeat block using the variables you established in the previous step.**

   **A)** The interpolation value of the repeat block should be **samp_rate / bits_per_second**

1. Create the flow diagram for the modulator.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Modulator_Flow_Diagram.png" alt="BFSK Modulator Flow Diagram">
      <figcaption><b>Figure 1.3</b> BFSK Modulator Flow Diagram for a Closed System</figcaption>
    </figure>
   </center>

   The frequencies of the top and bottom signal sources should be **fsk_center_freq - (bandwidth / 2)** and **fsk_center_freq + (bandwidth / 2)** accordingly. Using this relationship is important for allowing the values of the variable blocks to be changed in a way that affects the whole system equally. This will make more sense when you build the demodulator.

   Recognizing that the byte value from the source will have an amplitude of 1:
   1. **Why are the constant source and subtract blocks necessary for the modulator to function correctly?**
   1. **What functional block do they correspond to in our understanding of a BFSK modulator?**

1. Create the flow diagram for the channel.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Channel_Flow_Diagram.png" alt="BFSK Channel Flow Diagram">
      <figcaption><b>Figure 1.4</b> BFSK Channel Flow Diagram for a Closed System</figcaption>
    </figure>
   </center>

   It's important in this implementation to include a throttle block with it's sample rate appropriately set because there is no hardware limiting the rate of bit processing. Again recognizing that the amplitude of the signal supplied from the modulator is 1:

   1. **Determine an appropriate value for the Amplitude parameter of the Noise Source block such that when you enter a value in the SNR variable block you are entering the value in dB.**

1. Create the flow diagram for the demodulator and comparator.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/Closed_System/BFSK_Demodulator_Flow_Diagram.png" alt="BFSK Demodulator And Decoder Flow Diagram">
      <figcaption><b>Figure 1.5</b> BFSK Demodulator and Decoder for a Closed System</figcaption>
    </figure>
   </center>

   1. The frequency translating FIR filter block should be configured with parameters:
      - taps = `firdes.low_pass(1.0,samp_rate, bandwidth, bandwidth / 2)`
      - center frequency = `fsk_center_freq`
      - sample rate = `samp_rate`

   1. The quadrature demod block should be configured with parameter:
      - gain = `quad_demod_gain`

   1. The QT GUI Time Sink block should be configured with parameters:
      - Number of points: Some clever equation (I just used `2 * fsk_center_freq + bandwidth`)
      - Sample rate: `samp_rate`

   The frequency translating FIR filter block shifts the frequency located at the specified frequency down to baseband without causing aliasing to occur in addition to low pass filtering the shifted frequencies. **How do you think this block accomplishes this shift without causing aliasing to occur?**

   The quadrature demodulation block basically determines what Theta value of the peak frequency supplied to it and multiplies that value by the supplied gain parameter. More information on why this works can be found [here](https://www.gnuradio.org/doc/doxygen/classgr_1_1analog_1_1quadrature__demod__cf.html).

1. Run the system and verify that the bit source output and received signal line up in the GUI Time Sink block with both signals.

1. Modify the amplitude of the Noise Source block. Recognizing that the modulated signal has an amplitude of 1 in the channel, **at what SNR do you start noticing bits being dropped? Compare this value with the theoretical value you calculated earlier. Do these values line up? If not, why not?**

## BFSK Modulation Part 2: An Open Loop System

### BFSK Modulation

1. Create the variable blocks for this design.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/BFSK_Tx_Variable_Blocks.png" alt="BFSK Transmitter Variable Blocks">
      <figcaption><b>Figure 2.1</b> BFSK Variable Blocks for an Open Loop Transmitter System</figcaption>
    </figure>
   </center>

   All values can be used directly from the figure above except for:
   - bandwidth = `samp_rate / 50`

1. Construct the flow diagram shown below to generate a test series of bits.

   <center>
     <figure>
       <img src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Source_Flow_Diagram.png" alt="BFSK Source Flow Diagram">
       <figcaption><b>Figure 2.2</b> BFSK Source Flow Diagram</figcaption>
     </figure>
   </center>

   All values can be used directly as shown in the figure above except:
   - The repeat interpolation value: `int(samp_rate / bits_per_second)`

1. Construct the BFSK modulator.

   <center>
     <figure>
       <img src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Modulator_Flow_Diagram.png" alt="BFSK Modulator Flow Diagram">
       <figcaption><b>Figure 2.3</b> BFSK Modulator Flow Diagram</figcaption>
     </figure>
   </center>

   Use the same variable representations as you used in part 1 for these blocks.

1. Add the transmitter sink.

   <center>
     <figure>
       <img alt="BFSK Transmitter Flow Diagram" src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Transmitter_Flow_Diagram.png">
       <figcaption><b>Figure 2.4</b> BFSK Transmitter Flow Diagram</figcaption>
     </figure>
   </center>

   Set the **Ch0: Frequency (Hz)** parameter of the osmocom sink to `tx_center_freq`

   The osmocom source block mixes the baseband signal you supply to it up to the center frequency you supplied in the **Ch0: Frequency (Hz)** parameter. Note that you could use a file sink block in place of the osmocom sink block to save the output exactly as it would be seen prior to mixing up to the center frequency you supplied for the SDR. This can be helpful when debugging a GNU radio SDR system if you would like to eliminate the possibility of errors being introduced by the SDR or the channel.

## BFSK Demodulation

1. Create the variable blocks for this design.

   <center>
    <figure>
      <img src="/assets/img/GNU_Radio/BFSK/BFSK_Rx_Variable_Blocks.png" alt="BFSK Receiver Variable Blocks">
      <figcaption><b>Figure 2.5</b> BFSK Variable Blocks for an Open Loop Receiver System</figcaption>
    </figure>
   </center>

   - The sample rate(samp_rate) should be set to 4MHz because that is the appropriate sample rate for the rtl-sdr.
   - bandwidth = `samp_rate / 50`
   - bit_interpolation = `int(samp_rate/bits_per_second)`
   - symbol_decimation = `int(bit_interpolation / 4)`
   - quad_demod_gain is the same value as in part 1.

1. Add the receiver source block and a frequency sink

   <center>
     <figure>
       <img alt="BFSK Receiver Flow Diagram" src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Receiver_Flow_Diagram.png">
       <figcaption><b>Figure 2.6</b> BFSK Receiver Flow Diagram</figcaption>
     </figure>
   </center>

   Where Ch0. Frequency (Hz) = `rx_center_freq`. The osmocom source block acts in the reverse manner as the osmocom sink block and passes the received signal through a high quality bandpass filter with a bandpass region of 4 MHz centered at your provided center frequency. The resulting signal that the osmocom source block provides is a baseband representation of the sampled signal. Similar to the osmocom sink, a file source can be used to replace the baseband representation provided by the osmocom source with an arbitrary signal. If this file is from the file sink output of a modulator, the file can be used to simulate ideal, reproducible, communication between the transmitter and receiver.

1. Add the demodulator blocks

   <center>
     <figure>
       <img alt="BFSK Demodulator Flow Diagram" src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Demodulator_Flow_Diagram.png">
       <figcaption><b>Figure 2.7</b> BFSK Demodulator Flow Diagram</figcaption>
     </figure>
   </center>

   Where the frequency translating taps are `firdes.low_pass(1.0,samp_rate, 4*bandwidth, bandwidth/2)` and the center frequency is `fsk_center_freq`.

   The gain on the quadrature_demod block is `quad_demod_gain`.

## BFSK Modulation Part 3: Sending Data

### Packetization Basics

In order to send information across a channel digitally the information is generally packetized. There are number of methods available to packetize data. Generally a packet consists of three parts, a synchronization or training sequence, a header, and a payload.

#### Packet Synchronization Sequence (Training Sequence)

This is sent to differentiate the message from noise and provides a signal for the receiver to "lock onto" and recognize the start of a new message.

Some general rules:

1. Shouldn't simply repeat

   Ex. 10101010

   If the first two bits of the training sequence are sampled from noise immediately prior to the start of the actual training sequence, the received message will start two bits earlier than it actually should.

#### Packet Header

A packet header can be as simple or extensive as the protocol developer desires. A simple packet header may contain just the size of the payload that is contained in the packet while a more extensive packet header may contain information regarding the type of content, source address, destination address, and any other type of metadata desired.

### Encoding



### Decoding

1. Add the decoder blocks

   <center>
     <figure>
       <img alt="BFSK Decoder Flow Diagram" src="/assets/img/GNU_Radio/BFSK/BFSK_GNU_Radio_Decoder_Flow_Diagram.png">
       <figcaption><b>Figure 3.1</b> BFSK Decoder Flow Diagram</figcaption>
     </figure>
   </center>

# Questions

1. Compare the advantages and disadvantages of FSK and CPFSK.