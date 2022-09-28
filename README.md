# OSP300_auto-tool-set
Okuma OPS300 control - auto tool setting with multi-insert/single insert

V2 branch is proven, not scalable for more inserts.

V3 when proven, will be the main branch, added scalablility and easier to read


This program is great for automating tool setting in an Okuma CNC mill - w/ OSP300 control

    *** This has only been tested on an OKUMA MA500HII (horizontal machining center) with Renishaw tool setter***
    
    **** USE AT YOUR OWN RISK ****

    To adapt this for your machine:
    
        -Make sure you have OTCH (uses CALL OO30) program mapped to G120 within G/M code macro page:
        
            OTCH
            IF[PX NE 0] NALMX
            IF[PY LT .5] NALMY
            IF[PX EQ EMPTY] NALMX1
            IF[PY EQ EMPTY] NALMY1
            IF[PRS EQ EMPTY] NALMP1
            CALL OO30 VFST=#81H PLI=-2 PY=PY PRS=PRS
            GOTO NBTM
            NALMX
            VUACM[1]='PX MUST BE ZERO'
            VDOUT[992]=1234
            NALMY
            VUACM[1]='PY TO LOW'
            VDOUT[992]=1234
            NALMP
            VUACM[1]='PRS CANT BE ZERO'
            VDOUT[992]=1234
            NALMX1
            VUACM[1]='PX IS MISSING'
            VDOUT[992]=1234
            NALMY1
            VUACM[1]='PY IS MISSING'
            VDOUT[992]=1234
            NALMP1
            VUACM[1]='PRS IS MISSING'
            VDOUT[992]=1234
            NBTM
            G30 P1
            RTS

        -Additional macros involved in 0MTM2: 
            M203 = single point measure in this machines macro page
                -Replace M203, if desired, with: CALL OO30 VFST=#81H

            G30 P1 = safe tool change rapid positioning.
                -Change to XYZ moves if desired

        -Add OMTM2 to any desired macro, I use G111.

        -Adapt all tool numbers as you see fit, PY and RTS will vary by tool and must be calibrated for each tool
            Note: PX must be 0, unless you change OTCH (be very careful changing OTCH, may be useful for VMC's)

        -This program has tool list, in later revision this may be separated from OMTM2

        -Turn on [block skip] to check all inserts of tool (multi/single set only)
            Block skip off for single insert check


    To use: 
        -Make sure tool is setup in MTM2 program, if T# not in MTM2 program will alarm out
        
        -Manual operation:
            TxxM06 (desired tool to be checked)
            G111 - Cycle start
            
        -Auto operation:
            Add G111 after tool change within program
