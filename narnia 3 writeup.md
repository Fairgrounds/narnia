**Narnia Level 3**
narnia3.c: 
```
int main(int argc, char **argv) {

    int  ifd,  ofd;
    char ofile[16] = "/dev/null";
    char ifile[32];
    char buf[32];

    if(argc != 2){
        printf("usage, %s file, will send contents of file 2 /dev/null\n",argv[0]);
        exit(-1);
    }

    /* open files */
    strcpy(ifile, argv[1]);
    if((ofd = open(ofile,O_RDWR)) < 0 ){
        printf("error opening %s\n", ofile);
        exit(-1);
    }
    if((ifd = open(ifile, O_RDONLY)) < 0 ){
        printf("error opening %s\n", ifile);
        exit(-1);
    }

    /* copy from file1 to file2 */
    read(ifd, buf, sizeof(buf)-1);
    write(ofd,buf, sizeof(buf)-1);
    printf("copied contents of %s to a safer place... (%s)\n",ifile,ofile);

    /* close 'em */
    close(ifd);
    close(ofd);

    exit(1);
} 
```

The program takes a file name argument and sends the contents of the file to /dev/null. 
There's appears to be a potential buffer overflow in the program in strcpy.

strcpy assembly:
```
   0x0804854d <+66>:    mov    0xc(%ebp),%eax
   0x08048550 <+69>:    add    $0x4,%eax
   0x08048553 <+72>:    mov    (%eax),%eax
   0x08048555 <+74>:    push   %eax
   0x08048556 <+75>:    lea    -0x38(%ebp),%eax
   0x08048559 <+78>:    push   %eax
   0x0804855a <+79>:    call   0x80483a0 <strcpy@plt>
```
```
```
