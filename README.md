# RC Stunt Car Hacking with Hackrf
A GNU Radio on-off keying transmitter for [RC car](https://www.target.com/p/sharper-image-remote-control-rc-flip-stunt-vehicle/-/A-52125747#ln)

## GNU Radio Flowgraph
The GNU radio flow graph is the initial thing that I got working with a single attack  
(major S/O to [jordib123](https://github.com/jordib123/ook-transmitter) for the original   
flow graph).   

## Real Magic Script
The ``hacker_trasmit.py`` is taken from the original GNU radio companion output but has  
 direct calculations for the packet being sent for the different directions. This  
allows for progamming actions such as forward with both wheels for ten seconds  
then backwards with both wheels for ten seconds. Pretty interesting!
  
GNU Radio with the Hackrf is real hard to setup. But, the [Instant Gnu RADIO](https://wiki.gnuradio.org/index.php/UbuntuVM) makes the ENV easy to use! 

## Encoding Scheme
The controller has three buttons/sticks that control directions: 
- The reverse button
- A left wheel stick
- A right wheel stick 

With backwards and forwards on both of the sticks, this allows for 16 possible encodings.   
But, the reverse button takes place at the controller level. So, there are 8   
possible codes that can be sent then.   

### Signal Analysis
- Runs at 49.86 MHz:
  - https://fccid.io/2ABYDJZH2017B49
- A single pulse is .2 milliseconds or 208 microseconds
- 1110 (long) and (1010) are the only patterns seen. 
- The AMOUNT of ``10``s is ths code being sent. Each packet is prefaced with ``1110`` 
4 times. This is when the RC car knows when to stop look for data: 
  - Signal is sent by a difference in the 'ground' signal in 12 pulse cycles. 
  - The table of these is shown at the bottom of this. 
- Fault tolerance: 
    - Sending 4 bits (two groups of 10) short still works.
    - Sending 6 bits (three groups of 10) long still works
    - Flipping any of the bits (0s or 1s) within the 'payload' (non header) section will result in the signal not goes through
    - Removing any of the 1110s in the header causes the code not to work.
    - Changing the header to '1100', '0001' and '1000' work fine as well. But, '1111', '1011' and '1010' does not. Appears to need something that has a pulse that is not always on and not a consistent set of '10'

The encoding of each of the movements
- Forward Left:
    - Has a sprinkle of longer pulses
    - 128 pulses between longer signals
    - The length between long pulses is DOUBLED from the right
    - Cycle is 144 pulses
    - 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 
- Forward Right: 
    - Consistent on-off pattern per pulse
    - Followed by a collection of shorter (1-0-1-0) 
    - 32 pulses before seeing the longer pulses
        - 20 works as well!
    - 48 pulses for a cycle
    - 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 
    - 111011101110111010101010101010101010101010101010
- Forward Both:
    - 68 pulses before seeing the longer pulses
    - Cycle is 84 pulses
    - 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 
- Back Right: 
    - 80 pulses before seeing the longer pulses
    - Cycle is 96 pulses
    - 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 
- Back Left: 
    - 116 Pulses before seeing the longer pulses
    - Cycle is 132 pulses
    - 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 
- Back Both: 
    - 104 pulses before seeing the longer pulses
    - Cycle is 120 pulses
    - 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 
- Left forward - Right Back: 
    - 92 pulses before seeing the longer pulses
    - Cycle is 108 pulses
    - 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 
- Right-Left: 
    - 56 pulses before seeing the longer pulses
    - Cycle is 72 pulses
    - 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 
- Both Forward with reverse button: 
    - Same as both backwards
- Both backwards with reverse button: 
    - Same as both forward signal
- Right forward with reverse button: 
    - Same as right backwards signal
- Left forward with reverse button: 
    - Same as left backwards signal 
- Right backwards with reverse button: 
    - Same as right forward signal 
- Left backwards with reverse button: 
    - Same as left forward signal
- Left forwards - right backwards with reverse button: 
    - Same as right forwards - left backwards signal
- Right forwards - left backwards with reverse button: 
    - Same as left forwards - right backwards signal
- How this works: 
    - Right Forward Only:        48
    - UNUSED!                    60 
    - Left Back - Right Forward: 72 
    - Both Forward:              84
    - Back Right Only            96
    - Left Forward - Right back: 108
    - Both backwards:            120
    - Back Left Only:            132
    - Left Forward Only:         144
