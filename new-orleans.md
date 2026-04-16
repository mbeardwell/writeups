# Write up for Microcorruption - New Orleans

```
447e <create_password>
447e:  3f40 0024      mov	#0x2400, r15
4482:  ff40 3600 0000 mov.b	#0x36, 0x0(r15)
4488:  ff40 7100 0100 mov.b	#0x71, 0x1(r15)
448e:  ff40 2200 0200 mov.b	#0x22, 0x2(r15)
4494:  ff40 6b00 0300 mov.b	#0x6b, 0x3(r15)
449a:  ff40 2c00 0400 mov.b	#0x2c, 0x4(r15)
44a0:  ff40 4500 0500 mov.b	#0x45, 0x5(r15)
44a6:  ff40 6700 0600 mov.b	#0x67, 0x6(r15)
44ac:  cf43 0700      mov.b	#0x0, 0x7(r15)
44b0:  3041           ret
```

The password the lock wants you to input is defined in this method.

It simply stores the hex string `36 71 22 6b 2c 45 67 00` at 0x2400 in memory.

Entering this in the input field completes the challenge.

