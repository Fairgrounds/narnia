**Narnia Level 4**<br>
narnia4.c
```
extern char **environ;

int main(int argc,char **argv){
    int i;
    char buffer[256];

    for(i = 0; environ[i] != NULL; i++)
        memset(environ[i], '\0', strlen(environ[i]));

    if(argc>1)
        strcpy(buffer,argv[1]);

    return 0;
}
```

Examining the assembly of the program, specifically the `eax` register at the memset instruction, I found the location of environ[i]. 
`
   0x080484e6 <+59>:    push   %ecx
   0x080484e7 <+60>:    push   $0x0
   0x080484e9 <+62>:    push   %eax
   0x080484ea <+63>:    call   0x8048390 <memset@plt>
`
`
   Breakpoint 1, 0x080484ea in main ()
   (gdb) x/x $eax
   0xffffd8ac:     0x415f434c
`
So after the program runs, the memory at `0xffffd8ac` is overwritten with null bytes until it hits a null byte already there. 
Now we're going to look at where everything is stored on the stack.

int i
buffer[256]
environ
strcpy

