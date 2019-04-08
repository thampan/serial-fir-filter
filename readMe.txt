1. Introduction
The primary goal of an FIR filter is to process an incoming signal and provide a conditioned output. The conditioned output could be obtained by either removing or enhancing the spectral components. Finite impulse response (FIR) filters find their applications in various digital signal processing applications. This is particularly due to their stability and linear phase characteristics. They perform signal conditioning, anti-aliasing, low pass filtering, band selection video convolution functions etc.
The implementation of FIR filters depends to a large extent on the platform and the capability/availability of the hardware units. It could be either Serial or Parallel.

a. Parallel implementation: This requires dedicated adder and multiplier units. This helps to generate faster output thereby increasing the overall performance of the System. Even though this would increase the performance, this scheme requires the presence of the dedicated multipliers/floating point units in the system which may not be feasible always since then the cost would be pretty high.
b. Serial Implementation: On the other hand Serial FIR filter implementation requires just a single accumulator and a multiplier. Of course this would result in
a performance degradation but the advantage here is that this is particularly suitable in systems that lack more than one multiplier units. If the cost factor is dominant compared to the performance aspect, then this could be a better choice. However it is not suitable for real-time systems.
In this project we focus on the Serial Implementation of FIR Filter.

2. DESIGN
For a Serial FIR Filter we use the following elements:
1. Registers-N
For an N-tap Serial FIR filter we would need N registers which would delay the input signal and finally feed it to the multiplier unit.
2. LUT
The LUT would store the coefficients needed for multiplication. An additional pointer to traverse the LUT may be also required. The multiplier unit would fetch the required coefficient from the LUT using this pointer.
3. Multiplier
The serial implementation of an FIR filter requires at least 1 Multiplier unit. The Multiplier unit is used to multiply the delayed input from the Registers with the filter coefficients stored in the LUTs.
4. Accumulator
Finally we need an accumulator which would accumulate all the data after the multiplication together with the previous output samples. This module also forms the output of the Serial FIR Filter.
