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

This level appears to be similar to Narnia2, where we had to rewrite the return address of the program to execute our shell code. The key difference appears to be the existence of environ a pointer to a pointer to a char.

I attempted a vanilla return address overwrite, and got a segfault. The environ variable is likely interfering with our ability to return to an address of our choosing, so I will dig deeper.


So after the program runs, the memory at `0xffffd8ac` is overwritten with null bytes until it hits a null byte already there. 
Now we're going to look at where everything is stored on the stack.

int i - ebp-0x4
argv[x] - ebp+0xc
buffer[256] - ebp-0x104 
environ[x] - 0xffffd8ac
i really dont get the point of the extern environ variable. is it preventing me from doing shellcode stuff like in narnia2? returning to narnia2 for a refresher.

