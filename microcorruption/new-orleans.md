# Write up for Microcorruption - New Orleans

```
44bc <check_password>
44bc:  0e43           clr	r14
44be:  0d4f           mov	r15, r13
44c0:  0d5e           add	r14, r13
44c2:  ee9d 0024      cmp.b	@r13, 0x2400(r14)
44c6:  0520           jnz	$+0xc <check_password+0x16>
44c8:  1e53           inc	r14
44ca:  3e92           cmp	#0x8, r14
44cc:  f823           jnz	$-0xe <check_password+0x2>
44ce:  1f43           mov	#0x1, r15
44d0:  3041           ret
44d2:  0f43           clr	r15
44d4:  3041           ret
```

This compares the entered password at r13 to the password at 0x2400 created by create\_password().

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

create\_password() simply pastes some sequence of bytes into memory and so it is the same every time. The password is just the hex string `36 71 22 6b 2c 45 67 00`.
Since it doesn't change, there was no need to reverse create\_password() and you could've just noted the password stored in memory in plaintext.

I then entered this sequence into the actual challenge and solved it.
