## Getting Started (WIP)

First we want to run a simplified aspire/dispense cycle, with the following steps:

 1. Initialize the robot
 2. Move to washing location
 3. Wash the tips
 4. Move to vial 1 (absolute position)
 5. Aspirate from vial 1
 6. Move to vial 2 (absolute position)
 7. Dispense into vial 2
 8. Move to Washing location
 9. Wash the tips
 

The steps cannot be transferred to commands 1:1.
Therefore, we accumulated some command descriptions from different sources (TODO: where).

```
AFI - something related to washing?
BR
GFC - group feed command
GSC - group feed run (???)
MDT - Move to fluid height or error
MxIR
OV
PAA - position all axises
PAG
PAR
PAX/PAY/PAZ - position aboslute for axis of the LiHa arm
PIA
PIF - fake init
PIS - position warm init
PRG
PRX/PRY/PRZ - position relative for axis of the LiHa arm
PSY - y-spacing of tips
Q23
REE - report error
RSD1 - report system devices (1=num of deluters)
RPG
RPX/RPY/RPZ - report parameter on axis
RVF0
RVM - report volt meter
YIP
```

Before testing it is assumed that the following commands will implement the test plan.

```
# 1. Initialize the device
RFV - get firmware details
PIS - warm init
# 2. Move to Washing location
PAA - Position all axises
# 3. Wash the tips
# 4. Move to plate 1 vial 1
PAA - Position all axises
# 5. Aspirate from vial 1 on plate 1
# 6. Move to plate 1 vial 2
PAA - Position all axises
# 7. Dispense into vial 2
# 8. Move to Washing location
PAA - Position all axises
# 9. Wash the tips
```


The communication itself assumingly follows such a protocol:

 1. send command
 1. receive repeating response
 1. send acknowledge

A command is structured as:

 1. STX start byte
 1. channel
 1. device
 1. command string
 1. ETX end byte
 1. [LRC checksum](https://en.wikipedia.org/wiki/Longitudinal_redundancy_check)
    of everything

A command string is a short form as listed above, followed by possible
parameters.

The response is the device ID that will take on the command. It should be the same device the command was sent to.

The acknowledgement is an `@` character, followed again by the device ID.

We assume that after this exchange the tecan controller should be ready to receive the next message.
