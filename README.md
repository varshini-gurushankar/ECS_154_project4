# Project 4 README

* Student 1: Varshini Gurushankar (922249127)
* Student 2: Sravya Kota (919987625)

## Project Status
09/6/25 - Finished VPWBArbiter, VPWBObserver, and VPWBSRO

VPWBArbiter - Did according to lecture, confirmed that it works
VPWBObserver - Did according to lecture, confirmed that it works
VPWBSRO - Tried to simulate it, don't understand simulation style, but logic makes sense 

9/6/25 - Finished VPWBSRI - Tested works
Stuck on VPWBFrameQueue

09/7/25 11:54pm : Finished VPWBFrameQueue, debugged

9/8/25 : Finished VPWBRX and VPWBTX, Not sure if VPWBRX is right

9/9/25: Finished VPWBController, 

9/9/25: Finished VPWBRX, VPWBTX, and VPWController according to how the prof showed it to us in class 

tried to do the extra credit with the help of the hints provided by the professor in class 

Need to figure out how to simulate

## Known Issues
The project fails to function properly in ...
Without simulating, everything in our project should work

## References
* [Build an XOR gate from AND/NOT](https://electronics.stackexchange.com/questions/487529/build-an-xor-gate-from-and-not)
* [Some other link](https://stackoverflow.com)
* [Video on what shift registers are and how to use in logism](https://www.youtube.com/watch?v=IeeUSGXutTo&ab_channel=ShazShoaib)
* [Video on how to use counters with classic logism](https://youtu.be/I0ohV3kFtBw)

## Generative AI Use
I used ChatGPT for Prompt 1, and ZZZZZ for Prompt 2....

### Prompt 1
How do you create an XOR from only and/or/not gates?

### Response 1
To create an XOR (exclusive OR) gate using only AND, OR, and NOT gates, you can follow these steps. The XOR gate outputs true only when the inputs are different.

The truth table for XOR is:

| A | B | A XOR B |
|---|---|---------|
| 0 | 0 |    0    |
| 0 | 1 |    1    |
| 1 | 0 |    1    |
| 1 | 1 |    0    |

The Boolean expression for XOR can be derived as:\
A &oplus; B = (A &and; &not; B) &or; (&not; A &and; B)

This can be broken down into the following steps:

1. Create \(&not; A\) and \(&not; B\) using NOT gates.
1. Create \(A &and; &not; B\) and \(&not; A &and; B\) using AND gates.
1. Combine the results of the two AND gates using an OR gate.

Here is the step-by-step logic construction:

1. **NOT gates** to get \(&not; A\) and \(&not; B\):\
   NOT(A) &rarr; &not; A\
   NOT(B) &rarr; &not; B

1. **AND gates** to get \(A &and; &not; B\) and \(&not; A &and; B\):\
   A &and; &not; B &rarr; AND(A, &not; B)\
   &not; A &and; B &rarr; AND(&not; A, B)

1. **OR gate** to get the final result:\
   (A &and; &not; B) &or; (&not; A &and; B) &rarr; OR(AND(A, &not; B), AND(&not; A, B))

Putting it all together in a circuit diagram:

1. Input \(A\) goes to a NOT gate to produce \(&not; A\).
1. Input \(B\) goes to a NOT gate to produce \(&not; B\).
1. Input \(A\) and the output of the NOT gate for \(B\) go to an AND gate to produce \(A &and; &not; B\).
1. Input \(B\) and the output of the NOT gate for \(A\) go to an AND gate to produce \(&not; A &and; B\).
1. The outputs of the two AND gates go to an OR gate to produce the final output \(A &oplus; B\).

In terms of Boolean logic:\
XOR(A, B) = \(A &and; &not; B\) &or; \(&not; A &and; B\) 

This way, you have constructed an XOR gate using only AND, OR, and NOT gates.

### Prompt 2
Walk me through the eight states (IDLE, B0, B00, B1, B11, E1, E0, E00) and what transitions them. Don’t design logic, give me a state transition diagram.

### Response 2
VPWBObserver: 8-state narrative + transition table
Glossary (from the handout)

IDLE – seen after ≥3 zeros in a row.

B0 / B00 – 1 or 2 zeros seen since a 1 (separator run of zeros).

B1 / B11 – 1 or 2 ones seen since a 0 (pulse of ones).

E1 / E0 / E00 – error tracking: hit E1 on three 1s in a row (or any 1 while already in an E* state); remain in the E* family until IDLE is re-detected; no NEWBITs in E*. 



Observer outputs (naming from handout):
IDLE is true only in the IDLE state. NEWBIT pulses when a new data bit is recognized; DIN holds that bit. (Handout defines the signals, not their exact timing.) 



Decoding convention used here (you can swap if your course uses the opposite):
When a ones-run ends, B1→0 means a short pulse ⇒ DIN=1; B11→0 means a long pulse ⇒ DIN=0.
(This matches common VPW practice: short=1, long=0.)

State-transition table (inputs are sampled RX∈{0,1})
Current	On RX=0 → Next	On RX=1 → Next	When does NEWBIT pulse?	What is DIN when it pulses?	Notes
IDLE	IDLE (stay idle)	B1	—	—	IDLE holds on further zeros; first 1 starts a ones-run. 




B0	B00	B1	—	—	Counting zeros (1 so far) since last 1.
B00	IDLE (3rd zero)	B1	—	—	Third zero promotes to IDLE. 




B1	B0	B11	Yes, on RX=0	DIN=1 (short ones-run)	1 one followed by 0 → bit recognized.
B11	B0	E1	Yes, on RX=0	DIN=0 (long ones-run)	2 ones followed by 0 → bit recognized; 3rd 1 is an error. 




E1	E0	E1	Never in E*	—	In error: ones keep you in E1; zeros count toward exit. 




E0	E00	E1	Never in E*	—	Zeros progress E0→E00; any 1 snaps back to E1. 




E00	IDLE	E1	Never in E*	—	Third consecutive 0 from error returns to IDLE. 


### Changes 1
I needed XNOR, so changed it to:\
&not;\(\(A &and; &not; B\) &or; \(&not; A &and; B\)\)

### Changes 2 

From the state transition table, I was able to understand how the states work and implement the logic myself.



