---
title: picoGym Exclusive - Pwn - Local Target & Picker IV 
date: '2024-03-11'
tags: ['ctf', 'pwn', 'bof', 'picoctf']
draft: false
summary: All picoGym Exclusive Binary exploitation challenges writeups.
---

# Local Target

## Challenge Description

![Screenshot](/static/writeups/picoCTF/pwn/picoGym/local-target.png)

## Solution

Following files were provided:

```
local-target
local-target.c
```

Source code provided to us:

```c:local-target.c
#include <stdio.h>
#include <stdlib.h>


int main(){
  FILE *fptr;
  char c;

  char input[16];
  int num = 64;
  
  printf("Enter a string: ");
  fflush(stdout);
  gets(input);
  printf("\n");
  
  printf("num is %d\n", num);
  fflush(stdout);
  
  if( num == 65 ){
    printf("You win!\n");
    fflush(stdout);
    // Open file
    fptr = fopen("flag.txt", "r");
    if (fptr == NULL)
    {
        printf("Cannot open file.\n");
        fflush(stdout);
        exit(0);
    }

    // Read contents from file
    c = fgetc(fptr);
    while (c != EOF)
    {
        printf ("%c", c);
        c = fgetc(fptr);
    }
    fflush(stdout);

    printf("\n");
    fflush(stdout);
    fclose(fptr);
    exit(0);
  }
  
  printf("Bye!\n");
  fflush(stdout);
}
```

Here we can see that input buffer is of 16 bytes. And we just have to make the value of `num` equal to 65. We can do that by overflowing the buffer. Checking file security permissions:

![Screenshot](/static/writeups/picoCTF/pwn/picoGym/perm.png)

To overflow the `num` variable, just pass `9` x 24. 

Here, you can see that we have successfully overflowed the `num` variable, and its current value is `0`:

![Screenshot](/static/writeups/picoCTF/pwn/picoGym/lt-1.png)

Now, as we want to set value of **num** equal to `65`, we will add an `A` at the end (because capital A has an ASCII Code: **65**). So the value will become `65`:

```bash:fake-flag-for-testing
echo "flag{fake_fl4g_4_t3sting}" > flag.txt
```

![Screenshot](/static/writeups/picoCTF/pwn/picoGym/lt-2.png)

### Flag:

Running against remote to get the flag:

![Screenshot](/static/writeups/picoCTF/pwn/picoGym/lt-3.png)

---

# Picker IV

## Challenge Description

![Screenshot](/static/writeups/picoCTF/pwn/picoGym/picker-iv.png)

## Solution

Following files were provided:

```
picker-IV
picker-IV.c
```

Source code provided to us:

```c:picker-IV.c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>


void print_segf_message(){
  printf("Segfault triggered! Exiting.\n");
  sleep(15);
  exit(SIGSEGV);
}

int win() {
  FILE *fptr;
  char c;

  printf("You won!\n");
  // Open file
  fptr = fopen("flag.txt", "r");
  if (fptr == NULL)
  {
      printf("Cannot open file.\n");
      exit(0);
  }

  // Read contents from file
  c = fgetc(fptr);
  while (c != EOF)
  {
      printf ("%c", c);
      c = fgetc(fptr);
  }

  printf("\n");
  fclose(fptr);
}

int main() {
  signal(SIGSEGV, print_segf_message);
  setvbuf(stdout, NULL, _IONBF, 0); // _IONBF = Unbuffered

  unsigned int val;
  printf("Enter the address in hex to jump to, excluding '0x': ");
  scanf("%x", &val);
  printf("You input 0x%x\n", val);

  void (*foo)(void) = (void (*)())val;
  foo();
}
```

Probably the most easy pwn challenge I have ever seen. We just have to find the address of `win` function. 

I used `objdump` to find it:

```bash:objump-command
objdump -D ./picker-IV | grep "win"
```

![Screenshot](/static/writeups/picoCTF/pwn/picoGym/p4-1.png)

### Flag:

Pass the win address to remote server to execute it:

![Screenshot](/static/writeups/picoCTF/pwn/picoGym/p4-2.png)