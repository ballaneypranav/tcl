Notes from https://www.youtube.com/watch?v=6s6YbIa2k_g


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

```tcl
for {set i 0} {$i < 5} {incr i} {
    puts "I inside for loop: $i"
}
```

Prints:
I inside for loop: 0
I inside for loop: 1
I inside for loop: 2
I inside for loop: 3
I inside for loop: 4

```tcl
for {set i 0} {$i < 5} {incr i 2} {
    puts "I inside for loop: $i"
}
```

Prints:
I inside for loop: 0
I inside for loop: 2
I inside for loop: 4

### Functions
```tcl
proc sum {arg1 arg2} {
    set x [expr {$arg1 + $arg2}];
    return $x
}
sum 4 5
```
Prints 9.

If return statement is omitted, the return value of the last command is returned.
Built-in commands like `for` can also be redefined using `proc`.

```tcl
proc abc {a {b 1} {c 2}} {
    puts "$a $b $c"
}

abc 10; # Prints 10 1 2
abc 10 20; # Prints 10 20 2
abc 10 20 30; # Prints 10 20 30
abc 10 20 30 40; # Throws an error
```

```tcl
proc abc {a {b 1} {c 2} args} {
    puts "$a $b $c"
}

abc 10 20 30 40; # Prints 10 20 30, no error
```

```tcl
proc example {first {second ""} args} {
    if {$second eq ""} {
        puts "There is only one argument and it is: $first"
        return 1
    } else {
        if {$args eq ""} {
            puts "There are two arguments - $first and $second"
            return 2
        } else {
        puts "There are many arguments - $first, $second, $args"
        return "many"
        }
    }
}
```

### Scope
As usual otherwise. Can be changed with `global` and `upvar`.
Skipped.

### Lists
```tcl
set abc {{a 1} {b 2}}
set abc [split "a 1.b 2" "."]
set abc [list "a 1" "b 2"]
```
All three set `abc` to `{a 1} {b 2}`.

```tcl
set x "a b c"
puts "Item at index 2 of list {$x} is: [lindex $x 2]"
```
Prints "Item at index 2 of list {a b c} is: c"

```tcl
set y [split 7/4/1776 "/"]
puts "We celebrate on the [lindex $y 1]'th day of the [lindex $y 0]'th month"
```
Prints "We celebrate on the 4'th day of the 7'th month."

```tcl
set z [list puts "arg 2 is $y"]
puts "A command resembles: $z"
```
Prints "A command resembles: puts {arg 2 is 7 4 1776}"

```tcl
set i 0
foreach j $x {
    puts "$j is item number $i in list x"
    incr i
}
```
Prints 
a is item number 0 in list x
b is item number 1 in list x
c is item number 2 in list x

Take `a` and `b` from different lists:
```tcl
foreach a $listA b $listB {...}
```

```tcl
set b [list a b {c d e} {f {g h}}]

puts $b; # Returns "a b {c d e} {f {g h}}"

llength $b; # Returns 4

lindex $b 3; # Returns "f {g h}"

lindex [lindex $b 3] 1; # Returns "g h"

set b [split "a b {c d e} {f {g h}}"];
llength $b; # Returns 8 because splits at spaces
lindex $b 2; # Returns "{c"

set a [concat a b {c d e} {f {g h}}]
llength $a; # Returns 7

set a [concat a b "{c d e}" "{f {g h}}"]
llength $a; # Returns 4

lappend a {ij K lm}; # Returns "a b {c d e} {f {g h}} {ij K lm}"
llength $a; # Returns 5

set b [linsert $a 3 "1 2 3"]
# Returns "a b {c d e} {1 2 3} {f {g h}} {ij K lm}"
llength $b; # Returns 6

set b [lreplace $b 3 5 "AA" "BB"]
# Replaces elements 3 to 5 with "AA" "BB".
# 3 members are replaced with 2 memberes. Length goes down by 1.
puts "After lreplacing 3 positions with 2 values at position 3: $b\n"
# Returns "a b {c d e} AA BB"
llength $b; # Returns 5

lset b 2 "New"
# Returns "a b New AA BB"

set list [list {Marc 100} {Abdul 56} {Mahesh 20} {Daniel 25} {Paul 56}]
llength $list; # Returns 5
lsearch $list Ma*; # Returns 0 because elements 0 matches
lsearch $list Mah*; # Returns 2 because elements 2 matches


lsort $list
# Returns "{Abdul 56} {Daniel 25} {Mahesh 20} {Marc 100} {Paul 56}"

lrange $list 2 3
# Returns "{Mahesh 20} {Daniel 25}"

lrange $list 3 2; # Returns nothing
```

