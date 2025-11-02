# 02-APIBasicUtils.j - Line by Line Explanation

## Overview
This file contains a JASS library that provides basic utility functions for mathematical operations, type conversions, and data manipulation in Warcraft III maps.

---

## Line-by-Line Breakdown

### Lines 1-3: Editor Metadata
```jass
//TESH.scrollpos=0
//TESH.alwaysfold=0
//! nocjass
```
- **Line 1**: TESH (Trigger Editor Syntax Highlighter) setting - sets scroll position to 0
- **Line 2**: TESH setting - disables auto-folding of code blocks
- **Line 3**: JassHelper preprocessor directive - tells the compiler not to use cJASS features (uses standard JASS only)

### Line 4: Library Declaration
```jass
library APIBasicUtils
```
- Declares a JASS library named `APIBasicUtils` that can be required by other scripts

### Lines 5-8: Global Variables
```jass
globals
    boolean IsPrint = false
    constant string sLetters = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz"
endglobals
```
- **Line 5**: Begins global variable declarations
- **Line 6**: `IsPrint` - Boolean flag to track if printing/preloading is active (used for debug output)
- **Line 7**: `sLetters` - A constant string containing all alphanumeric characters for character-to-ID conversions
- **Line 8**: Ends global variable declarations

---

## Mathematical Functions

### Lines 10-16: absI (Absolute Value for Integers)
```jass
function absI takes integer number returns integer
    if number < 0 then
        return -number
    endif
    
    return number
endfunction
```
- **Line 10**: Function declaration - takes an integer, returns its absolute value
- **Line 11**: Checks if the number is negative
- **Line 12**: If negative, returns the negation (making it positive)
- **Line 13**: Ends the if statement
- **Line 14**: Empty line for readability
- **Line 15**: If the number was already positive, returns it unchanged
- **Line 16**: Ends the function

### Lines 18-24: absF (Absolute Value for Reals/Floats)
```jass
function absF takes real r returns real
    if r < 0. then
        return -r
    endif
    
    return r
endfunction
```
- **Line 18**: Function declaration - takes a real (floating-point) number
- **Line 19**: Checks if the real number is negative (note the `.` after 0 indicating float)
- **Line 20**: If negative, returns the negation
- **Line 21**: Ends the if statement
- **Line 22**: Empty line
- **Line 23**: Returns the number unchanged if positive
- **Line 24**: Ends the function

### Lines 26-32: floorF (Floor Function for Reals)
```jass
function floorF takes real r returns real
    if r < 0. then
        return -I2R( R2I( -r ) )
    endif
    
    return I2R( R2I( r ) )
endfunction
```
- **Line 26**: Function to round down a real number to the nearest integer value
- **Line 27**: Checks if the number is negative
- **Line 28**: For negatives: negates, converts to int (truncating), converts back to real, then negates again
  - `R2I(-r)` - Converts negative real to integer (truncates towards zero)
  - `I2R(...)` - Converts back to real
  - `-I2R(...)` - Negates to get proper floor value
- **Line 29**: Ends if
- **Line 30**: Empty line
- **Line 31**: For positive numbers: converts to int (truncates) then back to real
- **Line 32**: Ends the function

### Lines 34-36: floorI (Floor Function for Integers)
```jass
function floorI takes integer i returns integer
    return R2I( floorF( I2R( i ) ) )
endfunction
```
- **Line 34**: Floor function for integers (somewhat redundant since integers are already whole numbers)
- **Line 35**: Converts integer to real, applies floorF, converts back to integer
- **Line 36**: Ends the function

### Lines 38-46: ceilF (Ceiling Function for Reals)
```jass
function ceilF takes real r returns real
    if floorF( r ) == r then
        return r
    elseif r < 0. then
        return -( I2R( R2I( -r ) ) + 1. )
    endif
    
    return I2R( R2I( r ) ) + 1.
endfunction
```
- **Line 38**: Function to round up a real number to the nearest integer value
- **Line 39**: Checks if the number is already a whole number (floor equals original)
- **Line 40**: If already whole, returns it unchanged
- **Line 41**: Else if the number is negative
- **Line 42**: For negatives: negates, converts to int, converts to real, adds 1, then negates
- **Line 43**: Ends if
- **Line 44**: Empty line
- **Line 45**: For positive non-whole numbers: truncates to integer, converts to real, adds 1
- **Line 46**: Ends the function

### Lines 48-50: ceilI (Ceiling Function for Integers)
```jass
function ceilI takes integer i returns integer
    return R2I( ceilF( I2R( i ) ) )
endfunction
```
- **Line 48**: Ceiling function for integers (redundant for whole numbers)
- **Line 49**: Converts integer to real, applies ceilF, converts back to integer
- **Line 50**: Ends the function

### Lines 52-58: roundF (Rounding Function for Reals)
```jass
function roundF takes real r returns real
    if r > 0. then
        return I2R( R2I( r + .5 ) )
    endif
    
    return I2R( R2I( r - .5 ) )
endfunction
```
- **Line 52**: Function to round a real number to the nearest integer value
- **Line 53**: Checks if the number is positive
- **Line 54**: For positive: adds 0.5 then truncates (standard rounding technique)
- **Line 55**: Ends if
- **Line 56**: Empty line
- **Line 57**: For negative: subtracts 0.5 then truncates
- **Line 58**: Ends the function

### Lines 60-62: roundI (Rounding Function for Integers)
```jass
function roundI takes integer i returns integer
    return R2I( roundF( I2R( i ) ) )
endfunction
```
- **Line 60**: Rounding function for integers (redundant for whole numbers)
- **Line 61**: Converts integer to real, applies roundF, converts back to integer
- **Line 62**: Ends the function

### Lines 64-78: log (Logarithm Function)
```jass
function log takes integer number, integer base returns integer
    local integer id = 1
    
    if number > 0 then
        loop
            exitwhen number / base <= 1
            set id = id + 1
            set number = number / base
        endloop
        
        return id
    endif
    
    return 0
endfunction
```
- **Line 64**: Function to calculate logarithm of a number with a given base
- **Line 65**: Declares local variable `id` initialized to 1 (the result counter)
- **Line 66**: Empty line
- **Line 67**: Checks if the number is positive (log only defined for positive numbers)
- **Line 68**: Begins a loop
- **Line 69**: Exits loop when number divided by base is less than or equal to 1
- **Line 70**: Increments the counter
- **Line 71**: Divides the number by the base
- **Line 72**: Ends the loop
- **Line 73**: Empty line
- **Line 74**: Returns the calculated logarithm
- **Line 75**: Ends if
- **Line 76**: Empty line
- **Line 77**: Returns 0 if the number was not positive
- **Line 78**: Ends the function

### Lines 80-94: PowI (Power/Exponentiation Function)
```jass
function PowI takes integer x, integer power returns integer
    local integer y = x
    
    if power == 0 then
        set x = 1
elseif power > 1 then
        loop
            set power = power - 1
            exitwhen power == 0
            set x = x * y
        endloop
    endif
    
    return x
endfunction
```
- **Line 80**: Function to calculate x raised to the power of `power`
- **Line 81**: Stores the original value of x in y (for multiplication)
- **Line 82**: Empty line
- **Line 83**: Checks if power is 0
- **Line 84**: Any number to the power of 0 is 1
- **Line 85**: Else if power is greater than 1
- **Line 86**: Begins a loop for multiplication
- **Line 87**: Decrements the power counter
- **Line 88**: Exits when power reaches 0
- **Line 89**: Multiplies x by the original value (y)
- **Line 90**: Ends loop
- **Line 91**: Ends if
- **Line 92**: Empty line
- **Line 93**: Returns the result (x^power)
- **Line 94**: Ends the function

---

## Type Conversion Functions

### Lines 96-102: B2S (Boolean to String)
```jass
function B2S takes boolean flag returns string
    if flag then
        return "yes"
    endif
    
    return "no"
endfunction
```
- **Line 96**: Function to convert a boolean value to a readable string
- **Line 97**: Checks if the boolean is true
- **Line 98**: Returns "yes" for true
- **Line 99**: Ends if
- **Line 100**: Empty line
- **Line 101**: Returns "no" for false
- **Line 102**: Ends the function

---

## Character and String ID Conversion Functions

### Lines 104-121: CharToId (Character to ASCII ID)
```jass
function CharToId takes string input returns integer
    local integer pos = 0
    local string char
    
    loop
        set char = SubString( sLetters, pos, pos + 1 )
        exitwhen char == null or char == input
        set pos = pos + 1
    endloop
    
    if pos < 10 then
        return pos + 48
    elseif pos < 36 then
        return pos + 65 - 10
    endif
    
    return pos + 97 - 36
endfunction
```
- **Line 104**: Converts a single character string to its ASCII code equivalent
- **Line 105**: Initializes position counter to 0
- **Line 106**: Declares a local variable for character comparison
- **Line 107**: Empty line
- **Line 108**: Begins loop to find the character in `sLetters`
- **Line 109**: Extracts one character from `sLetters` at position `pos`
- **Line 110**: Exits when character is null or matches the input
- **Line 111**: Increments position to check next character
- **Line 112**: Ends loop
- **Line 113**: Empty line
- **Line 114**: If position is less than 10, it's a digit (0-9)
- **Line 115**: Returns ASCII code for digits: position + 48 (ASCII '0' is 48)
- **Line 116**: Else if position is less than 36, it's uppercase (A-Z)
- **Line 117**: Returns ASCII code for uppercase: position + 65 - 10 (ASCII 'A' is 65)
- **Line 118**: Ends if
- **Line 119**: Empty line
- **Line 120**: Otherwise it's lowercase (a-z), returns position + 97 - 36 (ASCII 'a' is 97)
- **Line 121**: Ends the function

### Lines 123-125: StringToId (4-Character String to Integer ID)
```jass
function StringToId takes string input returns integer
    return ( ( CharToId( SubString( input, 0, 1 ) ) * 256 + CharToId( SubString( input, 1, 2 ) ) ) * 256 + CharToId( SubString( input, 2, 3 ) ) ) * 256 + CharToId( SubString( input, 3, 4 ) )
endfunction
```
- **Line 123**: Converts a 4-character string to a 32-bit integer ID (used in WC3 for object IDs)
- **Line 124**: Complex nested calculation:
  - Extracts 4 characters from positions 0-1, 1-2, 2-3, 3-4
  - Converts each character to ASCII code
  - Combines them into a 32-bit integer by multiplying by 256 (8 bits per character)
  - Formula: `((char1 * 256 + char2) * 256 + char3) * 256 + char4`
- **Line 125**: Ends the function

### Lines 127-137: IdToChar (ASCII ID to Character)
```jass
function IdToChar takes integer input returns string
    local integer pos = input - 48
    
    if input >= 97 then
        set pos = input - 97 + 36
    elseif input >= 65 then
        set pos = input - 65 + 10
    endif
    
    return SubString( sLetters, pos, pos + 1 )
endfunction
```
- **Line 127**: Converts an ASCII code to its character representation
- **Line 128**: Assumes input is a digit, calculates position in `sLetters` (input - 48)
- **Line 129**: Empty line
- **Line 130**: Checks if input is lowercase letter (ASCII >= 97)
- **Line 131**: Adjusts position for lowercase: input - 97 + 36 (36 positions after digits and uppercase)
- **Line 132**: Else if input is uppercase letter (ASCII >= 65)
- **Line 133**: Adjusts position for uppercase: input - 65 + 10 (10 positions after digits)
- **Line 134**: Ends if
- **Line 135**: Empty line
- **Line 136**: Returns the character at the calculated position from `sLetters`
- **Line 137**: Ends the function

### Lines 139-148: IdToString (Integer ID to 4-Character String)
```jass
function IdToString takes integer input returns string
    local integer result = input / 256
    local string char    = IdToChar( input - 256 * result )
    
    set input  = result / 256
    set char   = IdToChar( result - 256 * input ) + char
    set result = input / 256
    
    return IdToChar( result ) + IdToChar( input - 256 * result ) + char
endfunction
```
- **Line 139**: Converts a 32-bit integer ID back to a 4-character string (reverse of StringToId)
- **Line 140**: Divides input by 256 to get the upper 24 bits
- **Line 141**: Extracts the 4th character (lowest byte) and converts to char
- **Line 142**: Empty line
- **Line 143**: Continues extracting bytes by dividing by 256
- **Line 144**: Extracts 3rd character and prepends it to the result string
- **Line 145**: Continues dividing to get upper bytes
- **Line 146**: Empty line
- **Line 147**: Extracts 1st and 2nd characters and prepends them to form complete 4-char string
- **Line 148**: Ends the function

---

## Hexadecimal Conversion Functions

### Lines 150-173: GetIntHex (Single Digit Integer to Hex Character)
```jass
function GetIntHex takes integer i returns string
    local string result = ""
    local integer numb  = absI( i )
    
    if numb >= 0 and numb <= 15 then
        if numb <= 9 then
            set result = I2S( numb )
        elseif numb == 10 then
            set result = "A"
        elseif numb == 11 then
            set result = "B"
        elseif numb == 12 then
            set result = "C"
        elseif numb == 13 then
            set result = "D"
        elseif numb == 14 then
            set result = "E"
        elseif numb == 15 then
            set result = "F"
        endif
    endif
    
    return result
endfunction
```
- **Line 150**: Converts a single digit (0-15) to its hexadecimal character representation
- **Line 151**: Initializes result string as empty
- **Line 152**: Gets absolute value of input
- **Line 153**: Empty line
- **Line 154**: Checks if number is in valid range (0-15) for a single hex digit
- **Line 155**: If 9 or less, it's a decimal digit
- **Line 156**: Converts integer to string directly (0-9)
- **Lines 157-168**: Series of if-else statements mapping 10-15 to A-F
- **Line 169**: Ends inner if
- **Line 170**: Ends outer if
- **Line 171**: Empty line
- **Line 172**: Returns the hex character
- **Line 173**: Ends the function

### Lines 175-199: IntToHex (Integer to Hexadecimal String)
```jass
function IntToHex takes integer i returns string
    local string result = ""
    local boolean ispos = i >= 0
    local integer numb  = absI( i )
    local integer j     = 0
    
    if numb != 0 then
        loop
            exitwhen numb == 0
            set j = numb - ( numb / 16 ) * 16
            set result = GetIntHex( j ) + result
            set numb = ( numb - j ) / 16
        endloop
        
        set result = "0x" + result
        
        if not ispos then
            set result = "-" + result
        endif
    else
        set result = "0x00"
    endif
    
    return result
endfunction
```
- **Line 175**: Converts a full integer to hexadecimal string representation
- **Line 176**: Initializes result string
- **Line 177**: Stores whether the number is positive
- **Line 178**: Gets absolute value
- **Line 179**: Declares counter variable
- **Line 180**: Empty line
- **Line 181**: Checks if number is not zero
- **Line 182**: Begins loop to extract hex digits
- **Line 183**: Exits when all digits processed
- **Line 184**: Calculates remainder when divided by 16 (gets rightmost hex digit)
- **Line 185**: Converts digit to hex character and prepends to result
- **Line 186**: Divides number by 16 to process next digit
- **Line 187**: Ends loop
- **Line 188**: Empty line
- **Line 189**: Prepends "0x" prefix to indicate hexadecimal
- **Line 190**: Empty line
- **Line 191**: Checks if original number was negative
- **Line 192**: Adds negative sign if needed
- **Line 193**: Ends if
- **Line 194**: Else (if number was 0)
- **Line 195**: Returns "0x00" for zero
- **Line 196**: Ends if
- **Line 197**: Empty line
- **Line 198**: Returns the formatted hex string
- **Line 199**: Ends the function

---

## Debug/Output Functions

### Lines 201-216: PrintData (Data Preloading for Debug Output)
```jass
function PrintData takes string path, string s, boolean flag returns nothing
    if not IsPrint then
        call PreloadGenClear( )
        call PreloadGenStart( )
        set IsPrint = true
    endif
    
    if IsPrint then
        call Preload( s )
        
        if flag then
            call PreloadGenEnd( path )
            set IsPrint = false
        endif
    endif
endfunction
```
- **Line 201**: Function to write debug data to a file using WC3's preload system
- **Line 202**: Checks if printing is not currently active
- **Line 203**: Clears any previous preload generation
- **Line 204**: Starts a new preload generation session
- **Line 205**: Sets flag to indicate printing is active
- **Line 206**: Ends if
- **Line 207**: Empty line
- **Line 208**: If printing is active
- **Line 209**: Preloads (writes) the string data
- **Line 210**: Empty line
- **Line 211**: If flag is true (indicating this is the last write)
- **Line 212**: Ends preload generation and writes to file at specified path
- **Line 213**: Resets the printing flag
- **Line 214**: Ends if
- **Line 215**: Ends if
- **Line 216**: Ends the function

### Line 217: Library End
```jass
endlibrary
```
- **Line 217**: Ends the `APIBasicUtils` library declaration

---

## Initialization Functions

### Lines 219-222: Standard WC3 Trigger Initialization
```jass
//===========================================================================
function InitTrig_APIBasicUtils takes nothing returns nothing
    //set gg_trg_APIBasicUtils = CreateTrigger(  )
endfunction
```
- **Line 219**: Separator comment
- **Line 220**: Standard WC3 initialization function (required by the game engine)
- **Line 221**: Commented out trigger creation (not needed for a library)
- **Line 222**: Ends the initialization function

### Lines 223-224: Preprocessor End
```jass
//! endnocjass
```
- **Line 223**: JassHelper preprocessor directive - ends the `nocjass` block started at line 3
- **Line 224**: Empty line (end of file)

---

## Summary

This utility library provides essential functions for:
1. **Math Operations**: Absolute value, floor, ceiling, rounding, logarithm, and power functions
2. **Type Conversions**: Boolean to string conversion
3. **Character/String ID Conversions**: Converting between characters and ASCII codes, and between 4-character strings and 32-bit integer IDs (crucial for WC3 object identification)
4. **Hexadecimal Conversions**: Converting integers to hexadecimal string representation
5. **Debug Output**: Writing debug data to files using the preload system

These functions form the foundation for more complex memory manipulation and API operations used throughout the MemHack API system.

