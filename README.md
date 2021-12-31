## Sears Garage Door Openers
- 389.4MHz-390.5MHz on the frequency
- Appears to have a preamble: 
    - Starts & ends with a zero
- The clicker has three types of signals (+,0,-):
    - https://www.eevblog.com/forum/projects/dip-switches/
- Compatability chart: 
    - https://www.northshorecommercialdoor.com/sears.html
- Links for the parts: 
    - Amazon Sears Craftsman Remake: 
        - https://www.amazon.com/Sears-Craftsman-139-53708-Remote-Equivalent/dp/B00283OFRG
    - FCC ID information but does not exist:
        - https://fccid.io/BYF8S513953706S
    - Ebay link to the device: 
        - https://www.ebay.com/itm/275033640139
- Datasheet: 
    - https://titan-interconnect.com/product/p8416-2/
- Encoding Scheme: 
    - Clock cycle of 500 microseconds 
    - 3 bits (+,0,-): 
        - +: 1001
        - -: 1011
        - 0: 1000
    - On the clicker that I have, the 7 & 9 are switched!
- 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 
- 100110001011100010111001100010001000 - 507ms (real) 
- 1001 1000 1011 1000 1011 1001 1000 1000 1000 1000 1000
- Works :) "100110001011100010111001100010001000100010000000000000000000000000" 
- Has a two groups of 0's at the end (10001000) <-- required! and 21 pulses of OFF at the end ()
- The code must be recieved FIVE times in a row in order to work
- Outputs: 
    - ALL +s: 
        - 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 
    - ALl 0s: 
        - 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 
    - All -s: 
        - 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 
    - Six +, -, +, 0
        - 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 
    - +,0,-,0,+,0,-,0,+:
        - 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 
    - +,0,-,0,0,0,0,0,0: 
        - 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 
    - +,0,-,0,0,0,0,0,1: 
        - 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 
    - 0,0,0,+,+,+,-,-,-: 
        - 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 
    - +,0,0,+,+,+,-,-,-: 
        - 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 
    - -,0,0,+,+,+,-,-,-:  
        - 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 
    - -,+,0,+,0,+,-,+,0: 
        - 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 
    - +,-,0,+,+,-,0,+,-
        1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 
    - Math: 
        - 1 millisecocond per symbol
        - A full send is 36 pulses of data, 8 for the trailer and 20 blanks for the reset. For a total of 64 pulses total
        - 19683 possible codes to send
        - 5 attempts per 
        - 104.96 minutes = 1 millisecond per bit * 64 bits * 19683 attempts * 5 correct guesses 

+,0,-,0,+,0,-,0,+
1001-1000-1011-1000-1001-1000-1011-1000-1001

## Random Articles on this
- https://www.andrewmohawk.com/2012/09/06/hacking-fixed-key-remotes/
- Attacking rolling codes: 
    - https://rolling.pandwarf.com/
