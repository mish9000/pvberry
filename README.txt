Detail for my Mk2_bothDisplays_4.ino sketch

This is an updated version of the original sketch for use with my 
PCB-based hardware.  It supports two current sensors.  CT1 is 
for monitoring the flow of energy at the supply point.  CT2 is 
available for monitoring the flow of current to the dump-load.  

The display can be driven in either of two ways.  The #define statement 
PIN_SAVING_HARDWARE near the top of the sketch can be either 
included or commented out to achieve this selection.  If no display is in 
use, it doesn't matter whether this line is included or not.

The two powerCal variables provides a convenient means of calibrating 
hardware for use with a Mk2 Router.  CT1 and CT2 each have separate 
powerCal variables, with the suffices _grid and _diverted respectively. 


Changes for version _2:
- for compatibility with other versions of the Mk2 code, the variable cycleCount 
has been removed.  This variable would have eventually overflowed which 
could have caused unpredictable effects with other versions of the Mk2 code.

- improved description of the display code initialisation in setup() for the 
pin-saving hardware option.

- removal of some unhelpful comments in the IO pin declaration section. 


Changes for version _3:
- a persistence check for the zero-crossing detection has been added.  This 
is to remove any false detections of zero-crossings.  This effect is seen more 
with some types of transformer than others.

- a mechanism has been added to monitor and display the minimum number
of sample sets that occur each mains cycle.  With a 125us timebase, and three
ADC samples per set, the expected number of sample sets per 20ms mains cycle 
is  20 / (3 * 0.125) = 53.33.   Any value less than 53 would indicate a loss 
of data. The measured value is sent to the Serial port every 5 seconds.


Changes for version _3a:
- typographical changes only.


Changes for version _3b:
- A minor change has been made to the function timerIsr() so as to resolve
a timing anomaly that has previously existed.  With the new arrangement, a 
complete set of data samples is made available by the ISR for use by the
main code.  There is no longer any possibility of these values being 
overwritten before they are processed.

- the display timeout period has been reduced to 8 hours instead of 10.


Changes for version _3c:
- The variables to store ADC data are now declared as "volatile" to remove 
any possibility of incorrect operation due to optimisation by the compiler.

Changes for version _4:
 - improvements to the start-up logic.  The start of normal operation is now 
    synchronised with the start of a new mains cycle.
 - reduce the amount of feedback in the Low Pass Filter for removing the DC content
     from the Vsample stream. This resolves an anomaly which has been present since 
     the start of this project.  Although the amount of feedback has previously been 
     excessive, this anomaly has had minimal effect on the system's overall behaviour.
 - removal of the unhelpful "triggerNeedsToBeArmed" mechanism
 - tidying of the "confirmPolarity" logic to make its behaviour more clear
 - SWEETZONE_IN_JOULES changed to WORKING_RANGE_IN_JOULES 
 - change "triac" to "load" wherever appropriate




