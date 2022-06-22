### Print 
```tcl
puts "Hello world"
puts {Hello world}
```

### Comments
```tcl
# This is a comment
puts {Hello world}; # Lines with comments must be terminated with a ;
```

### Assign variables
```tcl
set name Pranav
# Sets name = "Pranav" and also prints "Pranav"
set full_name "Pranav Ballaney"
set full_name {Pranav Ballaney}
# Arrays
set a(0) 5
set a(1) 10
```

### Retreive value of variable
```tcl
set name; # Prints "Pranav"
puts $name; # Prints "Pranav"
set a(0); # Prints 5
set a(1); # Prints 10
set a(2); # Throws error
```

### Print variable
```tcl
set var "Welcome to Tcl"
puts "$var"
# Prints "Welcome to Tcl" 
```
However, {} does not perform substitution.
```tcl
puts {$var}
```
Prints "$var" literally.

### Grouping arguments with []
```tcl
set x abc
puts $x; # Prits abc
set y [set x "def"]; # Prits def
# [] evaluate contents and return 
puts $x; # Prints def
```

### Math
```tcl
expr 2 + 2; # Prints 4
expr "{Hello}" eq "{Hello}"; # Prints 1
expr "{Hello}" eq "{hello}"; # Prints 0
expr $a(0) + $a(1); # Prints 15
```

### Conditionals
False:
* numerical value 0
* no
* false
True:
* all other numerial values
* yes
* true

Case-insensitive.

```tcl
set a 2
if {yes} {puts $a}; # Prints 2
if {TrUe} {puts $a}; # Prints 2

set x 1
if {$x == 2} {
    puts "\$x is 2"
}
else {
    puts "\$x is not 2"
}
```

```tcl
if "$$x == 2"
```
This causes it to first substitute $x with 1 and then compare $1 to 2. 
Since $1 does not exist here, it throws an error.

**Switch statement:**
```tcl
set x "ONE"
set y 1
set z ONE

switch $x {
    "$z" {
        set y1 [expr {$y + 1}]
        puts "MATCH \$z. $y + $z is $y1"
    }
    ONE {
        set y1 [expr {$y + 1}]
        puts "MATCH ONE. $y + 1 is $y1"
    }
    TWO {
        set y1 [expr {$y + 2}]
        puts "MATCH TWO. $y + 2 is $y1"
    }
    default {
        puts "$x is not a match."
    }
}
```

Prints "MATCH ONE. 1 + 1 is 2."
It does not match $z because it matches ONE
before it enters the substitution phase. 

However, if the block is written in the following way:
```tcl
switch $x "$z" {
        set y1 [expr {$y + 1}]
        puts "MATCH \$z. $y + $z is $y1"
    } ONE {
        set y1 [expr {$y + 1}]
        puts "MATCH ONE. $y + 1 is $y1"
    } TWO {
        set y1 [expr {$y + 2}]
        puts "MATCH TWO. $y + 2 is $y1"
    } default {
        puts "$x is not a match."
    }
```
Then it matches $z and prints
`MATCH $z. 1 + ONE is 2`

### Loops
```tcl
set x 1

while {$x < 5} {
    puts "x is $x"
    set x [expr {$x + 1}]
}
puts "exit first loop with x = $x"
```

Prints "exit first loop with x = 5"
If while conditions are placed in "" instead of {},
they are substituted once and then checked literally.
Here, `x < 5` will be substituted to `1 < 5` and 
will result in an infinite loop. 

