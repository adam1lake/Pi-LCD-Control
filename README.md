# Pi-LCD
A simple interface to common 16x2 Raspberry Pi LCD screens (HD44780).
Includes useful features such as:
- Text display
- Timeouts and delays
- Text scrolling
- Threading to allow for control of both lines separately
- Logic to prevent display "glitches" when performing multiple operations at once

## Installation
With **pip**:  
`python3 -m pip install --upgrade PiLCDControl`

Without **pip**:  
```wget https://github.com/adam1lake/Pi-LCD-Control/tarball/master -O - | tar -xz```  
```cd adam1lake*```  
```sudo python3 setup.py install```

## Getting Started
Ensure that you have wired the LCD screen correctly. Be sure to check the datasheet for your specific screen, but for 
the typical LCD1602 screen, this will be as follows:   
LCD1602 pinout:  
1 : Power Supply Ground       - Ground  
2 : Power Supply              - +5V  
3 : LCD Contrast              - 0-5V  
4 : RS (Register Select)      - GPIO pin **LCD_RS**  
5 : R/W (Read/Write)          - Ground  
6 : Enable                    - GPIO pin **LCD_E**  
7 : Data Bit 0                - Not used  
8 : Data Bit 1                - Not used  
9 : Data Bit 2                - Not used  
10: Data Bit 3                - Not used  
11: Data Bit 4                - GPIO pin **LCD_D4**  
12: Data Bit 5                - GPIO pin **LCD_D5**  
13: Data Bit 6                - GPIO pin **LCD_D6**  
14: Data Bit 7                - GPIO pin **LCD_D7**  
15: LCD Backlight Brightness  - 0-5V, 5V is full brightness  
16: LCD Backlight Ground      - Ground
#
You **_must_** know the GPIO BCM numbers for each of the GPIO pins specified above. Substitute them into the code below.


### Code:
```
from PiLCDControl import LCD

# Initialises the screen
screen = LCD.LCD(LCD_RS, LCD_E, LCD_D4, LCD_D5, LCD_D6, LCD_D7)

# Displays text on line 1
screen.display_text("Hello world!", 1) 

# Performs GPIO cleanup and clears the screen
screen.cleanup()
```

### Things to remember: 
Always call `screen.cleanup()` on exit of your program. It's a good idea to execute code within a try/except block like 
so:
```
try:
    screen.display("Interrupt me...", 1, timeout=5)
except KeyboardInterrupt:
    screen.cleanup()
```
This will ensure that if CTRL+C is used to exit the program, GPIO cleanup will still occur.


## Credits
Thanks to Raspberry Pi Spy for the inspiration -
https://www.raspberrypi-spy.co.uk/2012/07/16x2-lcd-module-control-using-python/
