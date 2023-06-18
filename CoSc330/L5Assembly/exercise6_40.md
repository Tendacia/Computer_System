**Translating the Switch Statement**
**exercise 6.40**

**Source code**

```C
#include <stdio.h>

int main()
{
    int guess;
    printf("Pick a number 0..3: ");
    scanf("%d", &guess);
    switch(guess)
    {
        case 0:
            printf("Not close\n");
            break;
        case 1:
            printf("Close\n");
            break;
        case 2:
            printf("Right on\n");
            break;
        case 3:
            printf("Too high\n");
    }
    return 0;
}


```



**Assembly code**

```Assembly code
         BR      main        
guess:   .EQUATE 0           ;local variable #2d
main:    SUBSP   2,i         ;push #guess
         STRO    msgIn,d     ;printf("Pick a number 0..3: ")
         DECI    guess,s     ;scanf("%d", &guess)
         LDWX    guess,s     ;switch (guess)
         ASLX                ;two bytes per address
         BR      guessJT,x   
guessJT: .ADDRSS case0       
         .ADDRSS case1       
         .ADDRSS case2       
         .ADDRSS case3       
case0:   STRO    msg0,d      ;printf("Not close\n")
         BR      endCase     ;break
case1:   STRO    msg1,d      ;printf("Close\n")
         BR      endCase     ;break
case2:   STRO    msg2,d      ;printf("Right on\n")
         BR      endCase     ;break
case3:   STRO    msg3,d      ;printf("Too high\n")
endCase: ADDSP   2,i         ;pop #guess
         STOP                
;
msgIn:   .ASCII  "Pick a number 0..3: \x00"

msg0:    .ASCII  "Not close\n\x00"

msg1:    .ASCII  "Close\n\x00"

msg2:    .ASCII  "Right on\n\x00"

msg3:    .ASCII  "Too high\n\x00"

         .END                  



```

**Machine code**

```Machine code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
0000  120003          BR      main        
             guess:   .EQUATE 0           ;local variable #2d
0003  580002 main:    SUBSP   2,i         ;push #guess
0006  490034          STRO    msgIn,d     ;printf("Pick a number 0..3: ")
0009  330000          DECI    guess,s     ;scanf("%d", &guess)
000C  CB0000          LDWX    guess,s     ;switch (guess)
000F  0B              ASLX                ;two bytes per address
0010  130013          BR      guessJT,x   
0013  001B   guessJT: .ADDRSS case0       
0015  0021            .ADDRSS case1       
0017  0027            .ADDRSS case2       
0019  002D            .ADDRSS case3       
001B  490049 case0:   STRO    msg0,d      ;printf("Not close\n")
001E  120030          BR      endCase     ;break
0021  490054 case1:   STRO    msg1,d      ;printf("Close\n")
0024  120030          BR      endCase     ;break
0027  49005B case2:   STRO    msg2,d      ;printf("Right on\n")
002A  120030          BR      endCase     ;break
002D  490065 case3:   STRO    msg3,d      ;printf("Too high\n")
0030  500002 endCase: ADDSP   2,i         ;pop #guess
0033  00              STOP                
             ;
0034  506963 msgIn:   .ASCII  "Pick a number 0..3: \x00"
      6B2061 
      206E75 
      6D6265 
      722030 
      2E2E33 
      3A2000 

0049  4E6F74 msg0:    .ASCII  "Not close\n\x00"
      20636C 
      6F7365 
      0A00   

0054  436C6F msg1:    .ASCII  "Close\n\x00"
      73650A 
      00     

005B  526967 msg2:    .ASCII  "Right on\n\x00"
      687420 
      6F6E0A 
      00     

0065  546F6F msg3:    .ASCII  "Too high\n\x00"
      206869 
      67680A 
      00     

006F                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
case0     001B         case1     0021         
case2     0027         case3     002D         
endCase   0030         guess     0000         
guessJT   0013         main      0003         
msg0      0049         msg1      0054         
msg2      005B         msg3      0065         
msgIn     0034         
--------------------------------------

```