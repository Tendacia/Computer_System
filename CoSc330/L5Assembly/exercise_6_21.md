# exercise 6.21
# Translating Call-by-Value Parameters with Global Variables


**Source Code**

```C
#include <stdio.h>

int numPts;
int value;
int j;

void printBar(int n)
{
    int k;
    for (k = 1; k <= n; ++k)
    {
        printf("*");
    }
    printf("\n");
}


int main()
{
    scanf("%d", &numPts);
    for(j = 1; j <= numPts; ++j)
    {
        scanf("%d", &value);
        printBar(value);
    }
    return 0;
}



```

**Assembly Code**


``` Assembly Code

         BR      main        
numPts:  .BLOCK  2           ;global #2d
value:   .BLOCK  2           ;global #2d
j:       .BLOCK  2           ;global #2d
;printBar
k:       .EQUATE 0           ;local variable #2d
n:       .EQUATE 4           ;formal parameter #2d
;
printBar:SUBSP   2,i         ;push #k
         LDWA    1,i         ;k=1
         STWA    k,s         
for1:    LDWA    k,s         ;k<=n
         CPWA    n,s         
         BRGT    endFor1     
         LDBA    '*',i       ;printf("*")
         STBA    charOut,d   
         LDWA    k,s         ;k++
         ADDA    1,i         
         STWA    k,s         
         BR      for1        
endFor1: LDBA    '\n',i      ;printf("\n")
         STBA    charOut,d   
         ADDSP   2,i         ;pop #k
         RET                 ;return

;main
main:    DECI    numPts,d    ;scanf("%d",&numPts)
         LDWA    1,i         ;j=1
         STWA    j,d         
for2:    LDWA    j,d         ;j<=numPts
         CPWA    numPts,d    
         BRGT    endFor2     
         DECI    value,d     ;scanf("%d",&value)
         LDWA    value,d     ;move value
         STWA    -2,s        
         SUBSP   2,i         ;push #n
         CALL    printBar    ; printBar(value)
         ADDSP   2,i         ;pop #n
         LDWA    j,d         ;j++
         ADDA    1,i         
         STWA    j,d         
         BR      for2        
endFor2: STOP                
         .END                  





```


**Object Code**

```Machine Code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
0000  120037          BR      main        
0003  0000   numPts:  .BLOCK  2           ;global #2d
0005  0000   value:   .BLOCK  2           ;global #2d
0007  0000   j:       .BLOCK  2           ;global #2d
             ;printBar
             k:       .EQUATE 0           ;local variable #2d
             n:       .EQUATE 4           ;formal parameter #2d
             ;
0009  580002 printBar:SUBSP   2,i         ;push #k
000C  C00001          LDWA    1,i         ;k=1
000F  E30000          STWA    k,s         
0012  C30000 for1:    LDWA    k,s         ;k<=n
0015  A30004          CPWA    n,s         
0018  1E002D          BRGT    endFor1     
001B  D0002A          LDBA    '*',i       ;printf("*")
001E  F1FC16          STBA    charOut,d   
0021  C30000          LDWA    k,s         ;k++
0024  600001          ADDA    1,i         
0027  E30000          STWA    k,s         
002A  120012          BR      for1        
002D  D0000A endFor1: LDBA    '\n',i      ;printf("\n")
0030  F1FC16          STBA    charOut,d   
0033  500002          ADDSP   2,i         ;pop #k
0036  01              RET                 ;return

             ;main
0037  310003 main:    DECI    numPts,d    ;scanf("%d",&numPts)
003A  C00001          LDWA    1,i         ;j=1
003D  E10007          STWA    j,d         
0040  C10007 for2:    LDWA    j,d         ;j<=numPts
0043  A10003          CPWA    numPts,d    
0046  1E0067          BRGT    endFor2     
0049  310005          DECI    value,d     ;scanf("%d",&value)
004C  C10005          LDWA    value,d     ;move value
004F  E3FFFE          STWA    -2,s        
0052  580002          SUBSP   2,i         ;push #n
0055  240009          CALL    printBar    ; printBar(value)
0058  500002          ADDSP   2,i         ;pop #n
005B  C10007          LDWA    j,d         ;j++
005E  600001          ADDA    1,i         
0061  E10007          STWA    j,d         
0064  120040          BR      for2        
0067  00     endFor2: STOP                
0068                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
charOut   FC16         endFor1   002D         
endFor2   0067         for1      0012         
for2      0040         j         0007         
k         0000         main      0037         
n         0004         numPts    0003         
printBar  0009         value     0005         
--------------------------------------



```
