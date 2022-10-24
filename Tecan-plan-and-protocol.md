## Getting Started

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

Most of the descriptions where taken from the source code of the Perl library
Robotics-0.23 by Jonathan Cline.

```
AAA - drive start
ACX - calibrate ROMA X
AFI - Something related to washing (???)
ADT - discard disposable tip on tipmask @ current xyz
AGR - grip distance
AGT - get disposable tip on tipmask
ALO - doorlock
ARP - postion angle r
ARS - scan IO RS 485 (???)
APT - pierce tipmask, start to max
BMA - stop immediately all axis xyzrg
BMX/BMY/BMZ/BMG- stop immediately X/Y/Z/G axis
BR
GFC - Group command load (batch commands)
GSC - Group command start (loaded commands)
MAX/MAY/MAZ/MAG/MAR - position absolute for axis, 0.1mm
MCT - detect clot on tipmask
MDT - Move to fluid height or error
MET - like MDT + remain at z-max on error
MHZ - move to z-travel
MRX/MRY/MRZ/MRG/MRR - position relative speed for axis, 0.1mm
MxIR
OV
PAA - position all axises
PAG
PAR
PAX/PAY/PAZ - position aboslute for axis
PIA - position init x/y/z
PIF - Fake init x/y/z
PIS - Position warm init
PIX/PIY/PIZ/PIG/PIR - position init for one axis only
PAX/PAY/PAZ/PAG/PAR - position absolute for one axis only
PRX/PRY/PRZ/PRG/PRR - position relative for axis
PSY - Y-spacing of tips, 0.1mm unit
Q23 - 
RAA - Report coord programmed with SAA?
RAE - Report current index of AAA?
RBL - Report liquid search submerge
RCL - report collision avoidance liha-posid
RDF - report diag functions
RDM - report liquid detect mode
RDI - read i/o digital input
RDL - Report liquid search retract
RDR - Report clot detect retract
RDX/RDY/RDZ - Report z diagnostic
RDO - read i/o digital out
REE - Report extended axis error
RFP - Report piezo module firmware version
RFV - Report firmware version
RGD - Report g diagnostic
RGG - Report g parameter
RGZ - Report global parameter on z for SPS, SSP, SSD
RLI - read door lock input
RLO - read door lock output
RLR - Report clot detect retract limit
RMA - get machine limit x-axis
RML - report liquid search zmax for MDT, MET
RNT - report number of tips on arm
RNV - report new device detected
ROW - read eeprom into RAM
RIZ - Report z-force, set by SIZ
ROD - Report g outrigger distnace for arm avoidance
RRS - report i/o rs483 address
RRX - Report absolute rest movement distance
RSD1 - Report system devices (1=num of deluters)
RSR - report clot detect retract speed
RPG - Report current g paramter
RPR - Report current r parameter
RPX/RPY/RPZ - Report parameter on axis
RTL - Report liquid search zstart for MDT, MET
RTS - report tip status mounted
RVF0
RVM - Report volt meter
RVZ - report parameter on z
RYS - report min yspacing
RYV - Report syringe volume in microliters
SAX/SAY - Set scale adjust for axis
SBL - set liquid search submerge for MDT, MET
SCL - set collision avoidance liha-posid
SFG - Set ramp parameter g
SFR - Set ramp parameter r
SFX/SFY/SFZ - Set ramp parameter for axis
SAA - set position into index table
SDO - set i/o digital out
SDL - set liquid search retract for MDT, MET
SDM - set liquid detection mode
SDR - set clot detection retract for MCT
SGG - Set parameter g
SHB - set comm rate
SHZ - set ztravles (? z-travel height?)
SIZ - set force z
SLR - set clot detect limit
SMA - set soft x limit in 0.1mm
SML - set liquid search zmax
SMX - set gear type x (also might be dangerous)
SNV - set node address
SOD - set outrigger distance g
SOG - set init offset g
SOF - firmware download
SOR - set init offset r
SOX/SOY/SOZ - set init offset for axis
SOW - write to eeprom non-volatile permanent
SPS - set pierce speed
SRA - Set absolute axis range
SSD - set discard speed when moving zstart to zmax
SSG - set slow speed g
SSL - set search speed for liquid commands, 0.1mm/s unit
SSR - set clot detection retract speed for MCT
SSP - set pick speed when moving zstart to zmax
SSS - set slow speed z, 0.1mm/s unit
SSX/SSY/SSZ - set slow speed x/y/z, 0.1mm/s unit
STL - set liquid search zstart for MDT, MET
SYS - set min y distance
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
