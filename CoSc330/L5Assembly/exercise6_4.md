

**source code**

```c
#include <stdio.h>

int main()
{
    const int bonus = 10;
    int exam1;
    int exam2;
    int score;
    scanf("%d %d", &exam1, &exam2);
    score = (exam1 + exam2) / 2 + bonus;
    printf("score = %d\n", score);
    return 0;
}



```


**Assembly language**

```assembly
;exercise 6.4

         BR      main
bonus:   .EQUATE 10; constant
exam1:   .EQUATE 4; local variable #2d
exam2:   .EQUATE 2; local variable #2d
score:   .EQUATE 0; local variable #2d
;
main:    SUBSP   6,i; push #exam1 #exam2 #score
         DECI    exam1,s; scanf("%d %d, &exam1, &exam2)
         DECI    exam2,s
         LDWA    exam1,s; score = (exam1 + exam2) / 2 + bonus
         ADDA    exam2,s
         ASRA
         ADDA    bonus,i
         STWA    score,s
         STRO    msg,d; printf("score = %d\n", score)
         DECO    score,s
         LDBA    '\n',i
         STBA    charOut,d
         ADDSP   6,i; pop #score #exam2 #exam1
         STOP
;
msg:     .ASCII  "score = \x00"
         .END
```

**Object language**


```Object
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.4

0000  120003          BR      main        
             bonus:   .EQUATE 10          ; constant
             exam1:   .EQUATE 4           ; local variable #2d
             exam2:   .EQUATE 2           ; local variable #2d
             score:   .EQUATE 0           ; local variable #2d
             ;
0003  580006 main:    SUBSP   6,i         ; push #exam1 #exam2 #score
0006  330004          DECI    exam1,s     ; scanf("%d %d, &exam1, &exam2)
0009  330002          DECI    exam2,s     
000C  C30004          LDWA    exam1,s     ; score = (exam1 + exam2) / 2 + bonus
000F  630002          ADDA    exam2,s     
0012  0C              ASRA                
0013  60000A          ADDA    bonus,i     
0016  E30000          STWA    score,s     
0019  490029          STRO    msg,d       ; printf("score = %d\n", score)
001C  3B0000          DECO    score,s     
001F  D0000A          LDBA    '\n',i      
0022  F1FC16          STBA    charOut,d   
0025  500006          ADDSP   6,i         ; pop #score #exam2 #exam1
0028  00              STOP                
             ;
0029  73636F msg:     .ASCII  "score = \x00"
      726520 
      3D2000 

0032                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
bonus     000A         charOut   FC16         
exam1     0004         exam2     0002         
main      0003         msg       0029         
score     0000         
--------------------------------------


```

