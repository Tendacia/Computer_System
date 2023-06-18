**Tanslating Call-by-Reference Parameters with Local Variables**
**Exercise 6.29**



**Source Code**

```C
#include <stdio.h>

void rect(int *p, int w, int h)
{
    *p = (w + h) * 2;
}

int main()
{
    int perim, width, height;
    printf("Enter width: ");
    scanf("%d", &width);
    printf("Enter height: ");
    scanf("%d", &height);
    rect(&perim, width, height);
    //ral
    printf("Perimeter = %d\n", perim);
    return 0;
}



```



**Assembly Code**

```Assembly Code
;exercise 6.29
         BR      main
p:       .EQUATE 6           ;formal parameter #2h
w:       .EQUATE 4           ;formal parameter #2d
h:       .EQUATE 2           ;formal parameter #2d
rect:    LDWA    w,s         ;*p = (w + h) * 2
         ADDA    h,s
         ASLA
         STWA    p,sf
         RET
perim:   .EQUATE 4           ;local variable #2d
width:   .EQUATE 2           ;local variable #2d
height:  .EQUATE 0           ;local variable #2d
main:    SUBSP   6,i         ;push #perim #width #height
         STRO    msg1,d      ;printf("Enter width: ")
         DECI    width,s     ;scanf("%d", &height)
         STRO    msg2,d      ;printf("Enter height: ")
         DECI    height,s    ;scanf("%d", &height)
         MOVSPA              ;move &perim
         ADDA    perim,i
         STWA    -2,s
         LDWA    width,s     ;move width
         STWA    -4,s
         LDWA    height,s    ;move height
         STWA    -6,s
         SUBSP   6,i         ;push #p #w #h       
         CALL    rect        ;rect(&perim, width, height)
         ADDSP   6,i         ;pop #p #w #h
         STRO    msg3,d      ;printf("Perimeter = %d\n", perim)
         DECO    perim,s
         LDBA    '\n',i
         STBA    charOut,d
         ADDSP   6,i         ;pop #height #width #perim
         STOP
msg1:    .ASCII  "Enter width: \x00"
msg2:    .ASCII  "Enter height: \x00"
msg3:    .ASCII  "Perimeter = \x00"
         .END


```


**Object Code**

```Machine Code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.29
0000  12000E          BR      main        
             p:       .EQUATE 6           ;formal parameter #2h
             w:       .EQUATE 4           ;formal parameter #2d
             h:       .EQUATE 2           ;formal parameter #2d
0003  C30004 rect:    LDWA    w,s         ;*p = (w + h) * 2
0006  630002          ADDA    h,s         
0009  0A              ASLA                
000A  E40006          STWA    p,sf        
000D  01              RET                 
             perim:   .EQUATE 4           ;local variable #2d
             width:   .EQUATE 2           ;local variable #2d
             height:  .EQUATE 0           ;local variable #2d
000E  580006 main:    SUBSP   6,i         ;push #perim #width #height
0011  490049          STRO    msg1,d      ;printf("Enter width: ")
0014  330002          DECI    width,s     ;scanf("%d", &height)
0017  490057          STRO    msg2,d      ;printf("Enter height: ")
001A  330000          DECI    height,s    ;scanf("%d", &height)
001D  03              MOVSPA              ;move &perim
001E  600004          ADDA    perim,i     
0021  E3FFFE          STWA    -2,s        
0024  C30002          LDWA    width,s     ;move width
0027  E3FFFC          STWA    -4,s        
002A  C30000          LDWA    height,s    ;move height
002D  E3FFFA          STWA    -6,s        
0030  580006          SUBSP   6,i         ;push #p #w #h
0033  240003          CALL    rect        ;rect(&perim, width, height)
0036  500006          ADDSP   6,i         ;pop #p #w #h
0039  490066          STRO    msg3,d      ;printf("Perimeter = %d\n", perim)
003C  3B0004          DECO    perim,s     
003F  D0000A          LDBA    '\n',i      
0042  F1FC16          STBA    charOut,d   
0045  500006          ADDSP   6,i         ;pop #height #width #perim
0048  00              STOP                
0049  456E74 msg1:    .ASCII  "Enter width: \x00"
      657220 
      776964 
      74683A 
      2000   

0057  456E74 msg2:    .ASCII  "Enter height: \x00"
      657220 
      686569 
      676874 
      3A2000 

0066  506572 msg3:    .ASCII  "Perimeter = \x00"
      696D65 
      746572 
      203D20 
      00     

0073                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
charOut   FC16         h         0002         
height    0000         main      000E         
msg1      0049         msg2      0057         
msg3      0066         p         0006         
perim     0004         rect      0003         
w         0004         width     0002         
--------------------------------------





````