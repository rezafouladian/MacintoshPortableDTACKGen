# /DTACK Generator for Macintosh Portable
This hardware is designed to generate the Data Transfer Acknowledge (DTACK) signal for the 68000 processor, specifically for the Macintosh Portable.

Typically the /DTACK signal is generated by the CPU GLU. In the case of the Backlit Macintosh Portable (Model M5126), /DTACK for the first 5MB of address space is generated by additional logic in order to get /DTACK sooner.

This card allows you to get faster /DTACK across the first 9MB of address space in similar fashion.

## Hardware versions
Multiple hardware variations are planned, both to provide different functionality as well to combat part shortages.

- DTACK16 - Designs using the ATF16V8
    - DTACK 16 DIP - This design uses the DIP version of the ATF16V8 and all through-hole components.
    - DTACK 16 SOIC (coming soon) - 
    - DTACK 16 PLCC (coming soon) -
- DTACK22 - Designs using the ATF22V10 (coming soon)

## Simplified CUPL example
This is simplified code to demonstrate how little is needed to generate the /DTACK signal.
```c
PIN 1   =   CLOCK;
PIN     =   [A23..20];  /* Address lines for decoding */
PIN     =   !AS;

PIN     =   !DTACK;
PIN     =   SEL;        /* Extra pin because of product term limits */
PIN     =   DELAY1;     /* D flip-flop 1 */
PIN     =   DELAY2;     /* D flip-flop 2 */

FIELD ADDRESS = [A23..20];

/* Define the address range we want to trigger from */
EXPRAM  =   ADDRESS:[100000..8FFFFF];

SEL = EXPRAM & AS;

DTACK.OE = SEL; /* Tri-state when our address range isn't active */

DELAY1.D = SEL; /* Delay for one clock pulse */
DELAY2.D = SEL & DELAY1; /* Delay for a second clock pulse */
DTACK = DELAY2;
```