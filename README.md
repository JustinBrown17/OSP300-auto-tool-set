# OSP300_auto-tool-set
Okuma OPS300 control - auto tool setting with multi-insert/single insert

V2 branch is proven, though very ugly.

V3 when proven, will be the main branch


This program is great for automating tool setting in an Okuma CNC mill - using OSP300 control

    To adapt this for your machine:
        -Make sure you have this OTCH (uses CALL OO30) program mapped to G120 in the macros page:
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

        -Note: M203 is mapped as single point measure in my macro page, you can replace M203 line with:
            CALL OO30 VFST=#81H

        -Also add OMTM2 to any desired macro, I use G111.
            After macro is set, you can use this inside of a program after a tool change, so long as that tool # is in the machine, the tool will be set automatically

        -Adapt all tool numbers as you see fit, PY and RTS will vary by tool and must be calibrated for each tool
            note: PX must be 0, unless you change OTCH (be very careful changing OTCH)

        -Turn on [block skip] to only set with single insert (multi/single set only)
            block skip off for multi insert check

    The offset will be set in current tool