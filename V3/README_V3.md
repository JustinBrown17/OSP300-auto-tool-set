# OSP300_auto-tool-set
Okuma OPS300 control - auto tool setting with multi-insert/single insert

V2 branch is proven, not scalable for more inserts.

V3 is proven: 
 - OINS2 + OMTM3 parametric in main branch
   - up to 16 insert measuring capability


##### This program is great for automating tool setting in an Okuma CNC mill - w/ OSP300 control

#### \*\*\* **This has only been tested on an OKUMA MA500HII (horizontal machining center) with Renishaw tool setter** \*\*\*
    
#### \*\*\*\* **USE AT YOUR OWN RISK** \*\*\*\*

## To adapt this for your machine:

- Make sure you have OTCH (uses CALL OO30) program mapped to G120 within G/M code macro page

- Adapt all tool numbers and variables as you see fit, within TOOL LIBRARY

- PY and RTS will vary by tool and must be calibrated for each tool
  - Note: **PX must be 0**, unless you change OTCH (be *very careful* changing OTCH, but this *may* be useful for VMC's)

#### Additional macros involved in OMTM3/OMTM2: 

- M203 = single point measure
  -Replace M203 line (in OMTM3), if desired, with: CALL OO30 VFST=#81H

- G30 P1 = safe tool change rapid positioning.
  -Pxx are set within HOME POSITIONS page, can also add order to always retract Z first
  -\*\* *recommend not changing postions or orders that are set* \*\*
  -Change to XYZ moves if desired



## To use: 
- Make sure tool is setup with correct PX, PY, RTS and INRT within TOOL LIBRARY in MTM3 program

- ADD needed macros into G/M code macro page:
  - Add OMTM3 to any desired macro, I use G111

### Multi insert check/set:
- Turn on block skip

### Manual operation:
- TxxM06 (desired tool to be checked)
- G111 
- Cycle start

### Auto operation:
- Add G111 after tool change within program


## New features to implement:

- OMTM3 contains a TOOL LIBRARY, in later revision this may be separated from OMTM3
- Delete M1 and replace with dwell so operator doesn't need to hit cycle start
