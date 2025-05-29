# Day 2: Timing libs, hierarchical vs flat synthesis and efficient flop coding styles
Welcome to the day 2 of this workshop! This day of the  workshop first explains  the `.lib` file named as `sky130_fd_sc_hd__tt_025C_1v80.lib` which is an open-source PDK for making Integrated Circuits.
Then explains the difference between hierarchical and flat synthesis and finally tells about the various styles of Flip-Flop coding.

## Timing libs
### SKY130 PDK

The SKY130 PDK is an open-source Process Design Kit (PDK) based on SkyWater Technology's 130nm CMOS technology. It's a crucial resource for designing integrated circuits (ICs) using this specific technology node. If you have cloned the repository discussed in day 1 you must have the Sky130 PDK liberty downloaded named as `sky130_fd_sc_hd__tt_025C_1v80.lib`. 

### What is the meaning and use of tt_025C_1v80 in the sky130 PDK?
You would have this question in your mind when you saw the file name. Actually the tt stands for typical process, and 025C is the temperarure in the chip because CMOS technique is too sensitiive on temperatures, and the 1v80 is the voltage.

