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

** environ is a pointer to a pointer.
What does the program do? 
firstly it does a memset regarding the environ variable.
then, if an argument is provided it copies it into the char array called buffer. 
the environ variable is declared but not defined. 
environ -> ADDRESS -> char.
so why is it iterating the pointer's pointer? don't understand that.
