**Translating Global Pointers**
**exercise 6.42**
**Translating rules for global pointers**
* To allocate storage for the pointer, it generates .BlOCK 2 because an address occupies two bytes.
* To get the pointer, it generates LDWA with direct addressing
* To get the content of the cell pointed to by the pointer, it genereates LDWA or LDBA, depending on the type in the cell with inderect addressing



**Source code**

```C

#include <stdio.h>
#include <stdlib.h>

int *a, *b, *c;

int main()
{
    a = (int *) malloc(sizeof(int));
    *a = 5;
    b = (int *) malloc(sizeof(int));
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
;exercise 6.42
         BR      main        
a:       .BLOCK  2           ;global #2h
b:       .BLOCK  2           ;global #2h
c:       .BLOCK  2           ;global #2h

;malloc()
;Precondition: A contains number of bytes
;Postconditon: X contains pointer to bytes
malloc:  LDWX    hpPtr,d     ;returned pointer
         ADDA    hpPtr,d     ;allocate from heap
         STWA    hpPtr,d     ;update hpPtr
         RET                 
hpPtr:   .ADDRSS heap        
heap:    .BLOCK  1           

;main
main:    LDWA    2,i         ;a = (int*) malloc(sizeof (int))
         CALL    malloc      ;allocate #2d
         STWX    a,d         
         LDWA    5,i         ;*a = 5
         STWA    a,n         
         LDWA    2,i         ;b = (int*)malloc(sizeof(int))
         CALL    malloc      ;allocate #2d
         STWX    b,d         
         LDWA    3,i         ;*b = 3
         STWA    b,n         
         LDWA    a,d         ;c = a
         STWA    c,d         
         LDWA    b,d         ;a = b
         STWA    a,d         
         LDWA    2,i         ;*a = 2 + *c
         ADDA    c,n         
         STWA    a,n         
         STRO    msg1,d      ;printf("*a = %d\n", *a)
         DECO    a,n         
         LDBA    '\n',i      
         STBA    charOut,d   
         STRO    msg2,d      ;printf("*b = %d\n", *b)
         DECO    b,n         
         LDBA    '\n',i      
         STBA    charOut,d   
         STRO    msg3,d      ;printf("*c = %d\n", *c)
         DECO    c,n         
         LDBA    '\n',i      
         STBA    charOut,d   
         STOP                
;
msg1:    .ASCII  "*a = \x00" 

msg2:    .ASCII  "*b = \x00" 

msg3:    .ASCII  "*c = \x00" 


         .END                  



```

**Machine code**

```Machine code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.42
0000  120016          BR      main        
0003  0000   a:       .BLOCK  2           ;global #2h
0005  0000   b:       .BLOCK  2           ;global #2h
0007  0000   c:       .BLOCK  2           ;global #2h

             ;malloc()
             ;Precondition: A contains number of bytes
             ;Postconditon: X contains pointer to bytes
0009  C90013 malloc:  LDWX    hpPtr,d     ;returned pointer
000C  610013          ADDA    hpPtr,d     ;allocate from heap
000F  E10013          STWA    hpPtr,d     ;update hpPtr
0012  01              RET                 
0013  0015   hpPtr:   .ADDRSS heap        
0015  00     heap:    .BLOCK  1           

             ;main
0016  C00002 main:    LDWA    2,i         ;a = (int*) malloc(sizeof (int))
0019  240009          CALL    malloc      ;allocate #2d
001C  E90003          STWX    a,d         
001F  C00005          LDWA    5,i         ;*a = 5
0022  E20003          STWA    a,n         
0025  C00002          LDWA    2,i         ;b = (int*)malloc(sizeof(int))
0028  240009          CALL    malloc      ;allocate #2d
002B  E90005          STWX    b,d         
002E  C00003          LDWA    3,i         ;*b = 3
0031  E20005          STWA    b,n         
0034  C10003          LDWA    a,d         ;c = a
0037  E10007          STWA    c,d         
003A  C10005          LDWA    b,d         ;a = b
003D  E10003          STWA    a,d         
0040  C00002          LDWA    2,i         ;*a = 2 + *c
0043  620007          ADDA    c,n         
0046  E20003          STWA    a,n         
0049  49006E          STRO    msg1,d      ;printf("*a = %d\n", *a)
004C  3A0003          DECO    a,n         
004F  D0000A          LDBA    '\n',i      
0052  F1FC16          STBA    charOut,d   
0055  490074          STRO    msg2,d      ;printf("*b = %d\n", *b)
0058  3A0005          DECO    b,n         
005B  D0000A          LDBA    '\n',i      
005E  F1FC16          STBA    charOut,d   
0061  49007A          STRO    msg3,d      ;printf("*c = %d\n", *c)
0064  3A0007          DECO    c,n         
0067  D0000A          LDBA    '\n',i      
006A  F1FC16          STBA    charOut,d   
006D  00              STOP                
             ;
006E  2A6120 msg1:    .ASCII  "*a = \x00" 
      3D2000 

0074  2A6220 msg2:    .ASCII  "*b = \x00" 
      3D2000 

007A  2A6320 msg3:    .ASCII  "*c = \x00" 
      3D2000 


0080                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
a         0003         b         0005         
c         0007         charOut   FC16         
heap      0015         hpPtr     0013         
main      0016         malloc    0009         
msg1      006E         msg2      0074         
msg3      007A         
--------------------------------------



```