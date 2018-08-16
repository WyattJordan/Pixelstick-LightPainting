# Arduino-Pixelstick-4.0
Code for building your own pixelstick for light painting (long exposure photography). What is a pixelstick? See this: http://www.thepixelstick.com/

# Examples:
![Demo](/imgs/pxl_stick.jpg)  

![Red Times New Roman Text](/imgs/pxl_gcc.jpg)  

# Documentation Roadmap  
1. Refactor and comment Code (add general program flow to ReadME)
2. Add dependent libraries to this repo
3. Add circuitry info (schematics, PCB design + parts)
4. Physical construction imgs
5. Software modification instructions (if using different pins)
6. Menu demo of some sort? 

# Capabilites

  Display any custom color on any portion of the stick (a wand).  
  Display any pattern of colors or their repetition over the length of the whole stick (a band pattern).  
  Add any custom string and display the text as the pixelstick is moved at a constant rate.  
      Multiple fonts supported as well as bold and italizied versions of those fonts.  
  Coming soon! (hopefully) Upload any image and have the pixelstick display that image as it is moved at a constant rate.  

# I Want One
  Contact me (wyattsjordan@gmail.com). Seriously, the documentation on this is clearly not done and if you're really interested in constructing one yourself you're gonna need some guidance. I squeezed the maximum capability out of an arduino possible for his project which led to a pretty unintuitive UI.  

  Caveat: (some time and $)  
  Here are the parts and total cost:  
  Schematic design:  
  General construction notes: (power considerations!)  
  Usage tips: (quit to main ^, general navigation rules, send me feedback!)  

# Program Implementation Overview:  

## Menu flow  
  See Menus.h, each menu has a corresponding menu struct organized in a large menu_table. Each struct contains an array of other indices in the menu table corresponding to it's parent menu and submenus as well as a list of 3 function pointers. The list contains a function to execute continually while that menu is being displayed, a function that runs when that menu is entered, and function to run when that menu's back button is hit.  

  Functions that run continually would be things such as updating the LED string when making the general shape odf a new wand or band pattern.  

  The function that executes when the user enters a menu often adjusts the encoder counts so that they will have certain default or saved value.  
 
  An example of the back button is when the user goes from HSV color selection back a menu to select RGB color selection. In this case the HSV values are converted to RGB before entering the RGB menu to preserve the color.


## Menu File Explanation  
Each menu file contains a list of numbers and the strings that should be displayed on the LCD. The numbers indicate the cursor locations and dynamic information locations. The specific meaning of each number is explained by using m/9.txt as an example:  

3 //       number of cursor locations (in this case in front of Continue, Back, and one one more at the very end to quit to main            
1 // 	   row of cursor 1  
0 // 	   col of cursor 1  
1 // 	   row of cursor 2  
10// 	   col of cursor 2  
1 // 	   row of cursor 3  
15// 	   col of cursor 3  
2 // 	   number of dynamic strings to display (in this case start and length)  
0 // 	   row of dynamic string 1  
6 // 	   col of dynamic string 1  
3 // 	   character length of dynamic string 1  
0 // 	   row of dynamic string 2  
13// 	   col of dynamic string 2  
3 // 	   character length of dynamic string 2  
Start:---Len:--- // first line of actual text to display on lcd ( - = space)  
--Continue-Back- // second line of actual text to display on lcd  

## Wand and Band Files Explanation  
A wand file contains 5 numbers:  
1. start index on brush  
2. length of wand  
3. R  
4. G  
5. B  

A Band file contains 2 + 3*(num bands) numbers:
1. number of repitions of the pattern (evenly over the stick)
2. number of different colors
3. R1
4. G1
5. B1
6. R2
7. G2
8. B2  
etc.  

## Font File System  
Fonts are organized in the following fashion  

f/  
----/FontName1  
--------/FontSpecific1 (Ex: 144r.txt)  
----------------/sizes.txt  
----------------/A.txt  
----------------/B.txt  
----------------/C.txt  
----------------etc. for all characters  
--------/FontSpecific2  
--------/FontSpecific3  
--------/FontSpecific4  
----/FontName2  
----/FontName3  

FontNames are things like "Times New Roman" or "Agency".  
    FontSpecifics are the number of LEDs to use (72, 144, 216, or 288) and a character to switch the font type (r - regular, i - italicized, b - bold, c - both italicized and bolded)  
        sizes.txt contains a list of characters and then their width in pixels  
	A.txt (or [character].txt) is a raw byte file of NUM_LEDS * 3 * character width size. These files were generated by using the dot factory: http://www.eran.io/the-dot-factory-an-lcd-font-and-image-generator/.  


