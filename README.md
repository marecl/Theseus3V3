# Voron Theseus 3 V3

Modifications to accomodate Creality's Ender 3 V3 components.  

**[CLICK HERE TO DOWNLOAD](https://voron.zip/done/VSW.zip)**

The above link includes the stealthburner toolhead files which are compatible with several of the printers in the Voron lineup. 
To keep things organized, StealthBurner’s files are maintained separately. 
the Stealthburner Repository can be browsed by clicking [HERE](https://github.com/VoronDesign/Voron-Stealthburner)

![Image of Voron Switchwire](http://vorondesign.com/images/voron_switchwire.jpg)

The official release of the Voron Switchwire 3d printer.  You can find the BOM and any other relevant information at the [Voron Design]( http://vorondesign.com/voron_switchwire) website.

![Voron Logo](http://vorondesign.com/images/voron_design_logo.png)

## Software

### (WIP) Klipper configs
Placeholder.

### Misc
It feels like Creality really needed to push the software out _fast_. They promised Creality OS in E3V3 marketing, but they're not planning on releasing it. Don't take my word for it, ask them yourself.  
No wonder, it's a disaster. Either that or I'm detached from what embedded software should look like.  

More information can be found in [the hardening guide](https://github.com/marecl/Theseus3V3/tree/master/Hardening).

## Custom parts

Obviously.

Directories are organized in the following way:  
* Custom - Made from scratch to fit the hardware
* Modified - Modified Voron models
* Native - Parts modeled from original hardware
* Primitives - Generalized Voron models
* Pictures

### Custom
* LCD Case Rear
* (WIP) Y Carriage Drill Guide

### Modified
* Rear Left Grill (power inlet with integrated filter)
* (WIP) Front Middle Grill
* (WIP) Y Bed Anchor

### Native
* LCD Case Front
* USB Port PCB
* (WIP) Bed Frame

### Primitives
* Grill Front (unmodified)
* Grill Rear L/R - cleaned up side grill

### Rear Side Grill
Cleaned up rear side grill.  
They differ from front grills which bugs me, but 

### Rear Left Grill
This part is not going to work with E3's stock power inlet.  
I'm using [DBI5-10A-S](https://www.emcdorexs.com/product/dbi5.html) which has built-in EMI filter.

<img src="https://github.com/marecl/Theseus3V3/blob/master/STL%203V3/pictures/3V3_grill_rear_left_integrated_filter_front.jpeg" alt="front" style="width:40%; height=auto;"><img src="https://github.com/marecl/Theseus3V3/blob/master/STL%203V3/pictures/3V3_grill_rear_left_integrated_filter_back.jpeg" alt="back" style="width:40%; height=auto;">

Used hardware:
1. 4x M3 threaded brass insert
2. 2x M3x8 FHCS (ISO 10642 / DIN 7991)

M3x6 FHCS may not thread fully into the insert.

### Front Left Grill
Stock grill. Copied only because original file name requested 2 pieces, while this project requires just 1.

### (WIP) Front Middle Grill
Front Middle Grill adapted for stock display (with case).  
Includes partial slit for LCD cable and USB port.

### (WIP) Front Right Grill
Front Side Grill adapted for stock display (with case).  
Includes mounting extrusion and slit for LCD cable.

### (WIP) LCD Case

#### Front
There's no front case, it's the entire display assembly. Though i made a mockup for development.

#### (WIP) Rear
Mounts LCD to the grills.

### (WIP) Bed Drill Guide
Stock E3V3 bed doesn't have enough meat to properly mount the anchor. Yet to level-up from brief look at it.

### (WIP) Y Bed Anchor
Stock E3V3 bed doesn't have enough meat to properly mount the anchor. Yet to level-up from brief look at it.