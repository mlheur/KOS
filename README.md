kOS Mod Overview
================

kOS is a scriptable autopilot Mod for Kerbal Space Program. It allows you write small programs that automate specific tasks. 

Installation
------------

Like other mods, simply merge the contents of the zip file into your Kerbal Space Program folder.

Usage
-----

Add the Compotronix SCS part to your vessel; it’s under the “Control” category in the Vehicle Assembly Building or Space Plane Hanger. After hitting launch, you can right-click on the part and select the “Open Terminal” option. This will give you access to the KerboScript interface where you can begin issuing commands and writing programs.

KerboScript
===========

KerboScript is a programming language that is derived from the language of planet Kerbin, which _sounds_ like gibberish to non-native speakers but for some reason is _written_ exactly like English. As a result, KerboScript is very English-like in its syntax. For example, it uses periods as statement terminators.

The language is designed to be easily accessible to novice programmers, therefore it is case-insensitive, and types are cast automatically whenever possible.

A typical command in KerboScript might look like this:

    PRINT “Hello World”.

Expressions
-----------

KerboScript uses an expression evaluation system that allows you to perform math operations on variables. Some variables are defined by you. Others are defined by the system. 

There are three basic types:

### Numbers

You can use mathematical operations on numbers, like this:

    SET X TO 4 + 2.5. 
    PRINT X.             // Outputs 6.5

The system follows the order of operations, but currently the implementation is imperfect. For example, multiplication will always be performed before division, regardless of the order they come in. This will be fixed in a future release.

Resource tags allow you to quickly look up the amount of a resource your ship has. Any resource that appears at the top right resource panel can be queried.

    PRINT <LiquidFuel>. // Print the total liquid fuel in all tanks.

### Strings

Strings are pieces of text that are generally meant to be printed to the screen. For example:

    PRINT “Hello World!”.

To concatenate strings, you can use the + operator. This works with mixtures of numbers and strings as well.

    PRINT “4 plus 3 is: “ + (4+3).

### Directions

Directions exist primarily to enable automated steering. You can initialize a direction using a vector or a rotation.

    SET Direction TO V(0,1,0).         // Set a direction by vector
    SET Direction TO R(0,90,0).        // Set by a rotation in degrees
 
You can use math operations on Directions as well. The next example uses a rotation of “UP” which is a system variable describing a vector directly away from the celestial body you are under the influence of.

    SET Direction TO UP + R(0,-45,0).  // Set direction 45 degress west of “UP”.

Command Reference
-----------------

### CLEARSCREEN

Clears the screen and places the cursor at the top left.
Example:

    CLEARSCREEN.

### COPY

Copies a file to or from another volume. Volumes can be referenced by their ID numbers or their names if they’ve been given one. See LIST, SWITCH and RENAME.
Example:

    SWITCH TO 1.       // Makes volume 1 the active volume
    COPY file1 FROM 0. // Copies a file called file1 from volume 0 to volume 1
    COPY file2 TO 0.   // Copies a file called file1 from volume 1 to volume 0

### DECLARE

Declares a variable at the current context level. Alternatively, a variable can be implicitly declared by a SET or LOCK statement.
Example:

    DECLARE X.

### EDIT

Edits a program on the currently selected volume.
Example:

    EDIT filename.

### LIST

Lists the files on the current volume, or lists the currently available volumes. Lists files by default.
Example:

    LIST.           // Lists files on the active volume
    LIST FILES.     // Lists files on the active volume
    LIST VOLUMES.   // Lists all volumes, with their numbers and names

### LOCK

Locks a variable to an expression. On each cycle, the target variable will be freshly updated with the latest value from expression.
Example:

    SET X TO 1.
    LOCK Y TO X + 2.
    PRINT Y.       // Outputs 3
    SET X TO 4.
    PRINT Y.      // Outputs 6

### ON

Awaits a change in a boolean variable, then runs the selected command. This command is best used to listen for action group activations.
Example:

    ON AG3 PRINT “Action Group 3 Activated!”.
    ON SAS PRINT “SAS system has been toggled”.

### PRINT

Prints the selected text to the screen. Can print strings, or the result of an expression.
Example:

    PRINT “Hello”.
    PRINT 4+1.
    PRINT “4 times 8 is: “ + (4*8).

### RENAME

Renames a file or volume.
Example:

    RENAME VOLUME 1 TO AwesomeDisk
    RENAME FILE MyFile TO AutoLaunch.

### RUN

Runs the specified file as a program.
Example:

    RUN AutoLaunch.

### SET.. TO

Sets the value of a variable. Declares the variable if it doesn’t already exist.
Example:

    SET X TO 1.

### STAGE

Executes the stage action on the current vessel.
Example:

    STAGE.

### SWITCH TO

Switches to the specified volume. Volumes can be specified by number, or it’s name (if it has one). See LIST and RENAME.
Example:

    SWITCH TO 0.             // Switch to volume 0.
    RENAME 1 TO AwesomeDisk. // Name volume 1 as AwesomeDisk.
    SWITCH TO AwesomeDisk.   // Switch to volume 1.

### TOGGLE

Toggles a variable between true or false. If the variable in question starts out as a number, it will be converted to a boolean and then toggled. This is useful for setting action groups, which are activated whenever their values are inverted.
Example:

    TOGGLE AG1.			// Fires action group 1.
    TOGGLE SAS.			// Toggles SAS on or off.

### UNLOCK

Releases a lock on a variable. See LOCK.
Examples:

    UNLOCK X.                // Releases a lock on variable X.
    UNLOCK ALL.              // Releases ALL locks.

### WAIT

Halts execution for a specified amount of time, or until a specific set of criteria are met. Note that running a WAIT UNTIL statement can hang the machine forever if the criteria are never met.
Examples:

    WAIT 6.2.                     // Wait 6.2 seconds.
    WAIT UNTIL X > 40.            // Wait until X becomes greater than 40.
    WAIT UNTIL APOAPSIS > 150000. // You can see where this is going.
    
### WHEN.. THEN

Executes a command when a certain criteria are met. Unlike WAIT, WHEN does not halt execution.
Example:

    WHEN BCount < 99 THEN PRINT BCount + “ bottles of beer on the wall”.

### ..ON

Sets a variable to true. This is useful for the RCS and SAS bindings.
Example:

    RCS ON 			// Turns on the RCS

### ..OFF

Sets a variable to false. This is useful for the RCS and SAS bindings.
Example

    RCS OFF			// Turns off the RCS
