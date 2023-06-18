**exercise 6_27**
**translate Call-by-Reference Parameters with Global variables**



**Source Code**

```C
#include <stdio.h>

int a, b;

void swap(int *r, int *s)
{
    int temp;
    temp = *r;
    *r = *s;
    *s = temp;
}

void order(int *x, int *y)
{
    if(*x > *y)
    {
        swap(x, y);
    }// ra2
}


int main()
{
    printf("Enter an integer: ");
    scanf("%d", &a);
    printf("Enter an integer: ");
    scanf("%d", &b);
    order(&a, &b);
    printf("Order they are: %d %d\n", a, b); //ra1
    return 0;
}



```


**Assembly Code**

```Assembly code
         BR      main
a:       .BLOCK  2           ;global variable #2d
b:       .BLOCK  2           ;global variable #2d
;swap
r:       .EQUATE 6           ;formal parameter #2h
s:       .EQUATE 4           ;formal parameter #2h
temp:    .EQUATE 0           ;local variable #2d
swap:    SUBSP   2,i         ;push #temp
         LDWA    r,sf        ;temp = *r
         STWA    temp,s
         LDWA    s,sf        ;*r = *s
         STWA    r,sf
         LDWA    temp,s      ;*s = temp
         STWA    s,sf
         ADDSP   2,i         ;pop #temp
         RET
;order
x:       .EQUATE 4           ;formal parameter #2h
y:       .EQUATE 2           ;formal parameter #2h
order:   LDWA    x,sf        ;if(*x > *y)
if:      CPWA    y,sf        
         BRLE    endIf
         LDWA    x,s         ;move x
         STWA    -2,s        
         LDWA    y,s         ;move y
         STWA    -4,s        
         SUBSP   4,i         ;push #x #y
         CALL    swap
         ADDSP   4,i         ;pop #x #y
endIf:   RET
;main
main:    STRO    msg1,d      ;printf("Enter an integer: ")
         DECI    a,d         ;scanf("%d", &a)
         STRO    msg2,d      ;printf("Enter an integer: ")
         DECI    b,d         ;scanf("%d", &b)
         LDWA    a,i         ;move &a
         STWA    -2,s        
         LDWA    b,i         ;move b
         STWA    -4,s        
         SUBSP   4,i         ;push #x #y 
         CALL    order       ;order(&a, &b)
         ADDSP   4,i         ;pop #y #x 
         STRO    msg3,d      ;printf("Ordered they are: %d, %d\n", a, b)
         DECO    a,d
         LDBA    ' ',i
         STBA    charOut,d 
         DECO    b,d
         LDBA    '\n',i
         STBA    charOut,d 
         STOP
msg1:    .ASCII  "Enter an integer: \x00"
msg2:    .ASCII  "Enter an integer: \x00"
msg3:    .ASCII  "Ordered they are: \x00"
         .END





```


**Object Code**

```Machine Code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
0000  12003F          BR      main        
0003  0000   a:       .BLOCK  2           ;global variable #2d
0005  0000   b:       .BLOCK  2           ;global variable #2d
             ;swap
             r:       .EQUATE 6           ;formal parameter #2h
             s:       .EQUATE 4           ;formal parameter #2h
             temp:    .EQUATE 0           ;local variable #2d
0007  580002 swap:    SUBSP   2,i         ;push #temp
000A  C40006          LDWA    r,sf        ;temp = *r
000D  E30000          STWA    temp,s      
0010  C40004          LDWA    s,sf        ;*r = *s
0013  E40006          STWA    r,sf        
0016  C30000          LDWA    temp,s      ;*s = temp
0019  E40004          STWA    s,sf        
001C  500002          ADDSP   2,i         ;pop #temp
001F  01              RET                 
             ;order
             x:       .EQUATE 4           ;formal parameter #2h
             y:       .EQUATE 2           ;formal parameter #2h
0020  C40004 order:   LDWA    x,sf        ;if(*x > *y)
0023  A40002 if:      CPWA    y,sf        
0026  14003E          BRLE    endIf       
0029  C30004          LDWA    x,s         ;move x
002C  E3FFFE          STWA    -2,s        
002F  C30002          LDWA    y,s         ;move y
0032  E3FFFC          STWA    -4,s        
0035  580004          SUBSP   4,i         ;push #x #y
0038  240007          CALL    swap        
003B  500004          ADDSP   4,i         ;pop #x #y
003E  01     endIf:   RET                 
             ;main
003F  490076 main:    STRO    msg1,d      ;printf("Enter an integer: ")
0042  310003          DECI    a,d         ;scanf("%d", &a)
0045  490089          STRO    msg2,d      ;printf("Enter an integer: ")
0048  310005          DECI    b,d         ;scanf("%d", &b)
004B  C00003          LDWA    a,i         ;move &a
004E  E3FFFE          STWA    -2,s        
0051  C00005          LDWA    b,i         ;move b
0054  E3FFFC          STWA    -4,s        
0057  580004          SUBSP   4,i         ;push #x #y
005A  240020          CALL    order       ;order(&a, &b)
005D  500004          ADDSP   4,i         ;pop #y #x
0060  49009C          STRO    msg3,d      ;printf("Ordered they are: %d, %d\n", a, b)
0063  390003          DECO    a,d         
0066  D00020          LDBA    ' ',i       
0069  F1FC16          STBA    charOut,d   
006C  390005          DECO    b,d         
006F  D0000A          LDBA    '\n',i      
0072  F1FC16          STBA    charOut,d   
0075  00              STOP                
0076  456E74 msg1:    .ASCII  "Enter an integer: \x00"
      657220 
      616E20 
      696E74 
      656765 
      723A20 
      00     

0089  456E74 msg2:    .ASCII  "Enter an integer: \x00"
      657220 
      616E20 
      696E74 
      656765 
      723A20 
      00     

009C  4F7264 msg3:    .ASCII  "Ordered they are: \x00"
      657265 
      642074 
      686579 
      206172 
      653A20 
      00     

00AF                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
a         0003         b         0005         
charOut   FC16         endIf     003E         
if        0023         main      003F         
msg1      0076         msg2      0089         
msg3      009C         order     0020         
r         0006         s         0004         
swap      0007         temp      0000         
x         0004         y         0002         
--------------------------------------




```