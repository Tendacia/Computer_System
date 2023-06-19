**Translating Local Pointers**
**exercise 6.44**
**Translating rules for local pointers**
* To allocate storage for the pointer, it generates SUBSP with two bytes for each pointer because an address occupies two bytes
* To get the pointer, it generates LDWA with stack-relative addressing
* To get the constent of the cell pointed to by the pointer, it generates LDWA with stack-relative deferred addressing





**Source code**

```C
#include <stdio.h>
#include<stdlib.h>

int main()
{
    int *a, *b *c;
    a = (int*) malloc(sizeof(int));
    *a = 5;
    b = (int *)malloc(sizeof(int));
    *b = 3;
    c = a;
    a = b;
    *a = 2 + *c;
    printf("*a = %d\n", *a);
    printf("*b = %d\n", *b);
    printf("*c = %d\n", *c);
    return 0;
}




```



**Assembly code**

```Assembly code
;exercise 6.44
         BR      main
a:       .EQUATE 4           ;local #2h
b:       .EQUATE 2           ;local #2h
c:       .EQUATE 0           ;local #2h
main:    SUBSP   6,i         ;push #a #b #c
         LDWA    2,i         ;a = (int *) malloc(sizeof(int))
         CALL    malloc      ;malloc #2d
         STWX    a,s
         LDWA    5,i         ;*a = 5
         STWA    a,sf
         LDWA    2,i         ;b = (int *)malloc(sizeof(int))
         CALL    malloc      ;malloc #2d
         STWX    b,s
         LDWA    3,i         ;*b = 3
         STWA    b,sf
         LDWA    a,s         ; c = a
         STWA    c,s         
         LDWA    b,s         ;a = b
         STWA    a,s         
         LDWA    2,i         ;*a = 2 + *c
         ADDA    c,sf
         STWA    a,sf
         STRO    msg1,d      ;printf("*a = %d\n", *a)
         DECO    a,sf
         LDBA    '\n',i
         STBA    charOut,d
         STRO    msg2,d      ;printf("*b = %d\n", *b)
         DECO    b,sf
         LDBA    '\n',i
         STBA    charOut,d
         STRO    msg3,d      ;printf("*c = %d\n", *c)
         DECO    c,sf
         LDBA    '\n',i
         STBA    charOut,d
         ADDSP   6,i         ;pop #c #b #a
         STOP
;
msg1:    .ASCII  "*a = \x00"
msg2:    .ASCII  "*b = \x00"
msg3:    .ASCII  "*c = \x00"
;malloc
malloc:  LDWX    hpPtr,d     ;returned pointer
         ADDA    hpPtr,d     ;allocate from heap
         STWA    hpPtr,d     ;update hpPtr
         RET
hpPtr:   .ADDRSS heap        ;address of next free byte
heap:    .BLOCK  1           ;first byte in the heap
         .END




```

**Machine code**

```Machine code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.44
0000  120003          BR      main        
             a:       .EQUATE 4           ;local #2h
             b:       .EQUATE 2           ;local #2h
             c:       .EQUATE 0           ;local #2h
0003  580006 main:    SUBSP   6,i         ;push #a #b #c
0006  C00002          LDWA    2,i         ;a = (int *) malloc(sizeof(int))
0009  240073          CALL    malloc      ;malloc #2d
000C  EB0004          STWX    a,s         
000F  C00005          LDWA    5,i         ;*a = 5
0012  E40004          STWA    a,sf        
0015  C00002          LDWA    2,i         ;b = (int *)malloc(sizeof(int))
0018  240073          CALL    malloc      ;malloc #2d
001B  EB0002          STWX    b,s         
001E  C00003          LDWA    3,i         ;*b = 3
0021  E40002          STWA    b,sf        
0024  C30004          LDWA    a,s         ; c = a
0027  E30000          STWA    c,s         
002A  C30002          LDWA    b,s         ;a = b
002D  E30004          STWA    a,s         
0030  C00002          LDWA    2,i         ;*a = 2 + *c
0033  640000          ADDA    c,sf        
0036  E40004          STWA    a,sf        
0039  490061          STRO    msg1,d      ;printf("*a = %d\n", *a)
003C  3C0004          DECO    a,sf        
003F  D0000A          LDBA    '\n',i      
0042  F1FC16          STBA    charOut,d   
0045  490067          STRO    msg2,d      ;printf("*b = %d\n", *b)
0048  3C0002          DECO    b,sf        
004B  D0000A          LDBA    '\n',i      
004E  F1FC16          STBA    charOut,d   
0051  49006D          STRO    msg3,d      ;printf("*c = %d\n", *c)
0054  3C0000          DECO    c,sf        
0057  D0000A          LDBA    '\n',i      
005A  F1FC16          STBA    charOut,d   
005D  500006          ADDSP   6,i         ;pop #c #b #a
0060  00              STOP                
             ;
0061  2A6120 msg1:    .ASCII  "*a = \x00" 
      3D2000 

0067  2A6220 msg2:    .ASCII  "*b = \x00" 
      3D2000 

006D  2A6320 msg3:    .ASCII  "*c = \x00" 
      3D2000 

             ;malloc
0073  C9007D malloc:  LDWX    hpPtr,d     ;returned pointer
0076  61007D          ADDA    hpPtr,d     ;allocate from heap
0079  E1007D          STWA    hpPtr,d     ;update hpPtr
007C  01              RET                 
007D  007F   hpPtr:   .ADDRSS heap        ;address of next free byte
007F  00     heap:    .BLOCK  1           ;first byte in the heap
0080                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
a         0004         b         0002         
c         0000         charOut   FC16         
heap      007F         hpPtr     007D         
main      0003         malloc    0073         
msg1      0061         msg2      0067         
msg3      006D         
--------------------------------------



```