**Translating Local Arrays**
**exercise 6.36**

**Source code**

```C
#include <stdio.h>

int main()
{
    int vector[4];
    int j;
    for(j = 0; j < 4; j++)
    {
        scanf("%d", &vector[j]);
    }
    for(j = 3; j >= 0; j--)
    {
        printf("%d %d\n", j, vector[j]);
    }
    return 0;
}

```



**Assembly code**

```Assembly code
;exercise 6.36
         BR      main        
vector:  .EQUATE 2           ;local variable #2d4a
j:       .EQUATE 0           ;local variable #2d
main:    SUBSP   10,i        ;push #vector #j
         LDWA    0,i         ;j=0
         STWA    j,s         
for1:    LDWX    j,s         
         CPWX    4,i         ;j<4
         BRGE    endFor1     
         ASLX                
         DECI    vector,sx   ;scanf("%d", &vector[j])
         LDWX    j,s         ;j++
         ADDX    1,i         
         STWX    j,s         
         BR      for1        
endFor1: LDWX    3,i         ;j=3
         STWX    j,s         
for2:    LDWX    j,s         ;j>=0
         CPWX    0,i         
         BRLT    endFor2     
         DECO    j,s         ;printf("%d %d\n", j, vector[j])
         ASLX                
         LDBA    ' ',i       
         STBA    charOut,d   
         DECO    vector,sx   
         LDBA    '\n',i      
         STBA    charOut,d   
         LDWA    j,s         ;j--
         SUBA    1,i         
         STWA    j,s         
         BR      for2        
endFor2: ADDSP   10,i        ;pop #j #vector
         STOP                
         .END                  


```


**Machine code**

```Machine code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.36
0000  120003          BR      main        
             vector:  .EQUATE 2           ;local variable #2d4a
             j:       .EQUATE 0           ;local variable #2d
0003  58000A main:    SUBSP   10,i        ;push #vector #j
0006  C00000          LDWA    0,i         ;j=0
0009  E30000          STWA    j,s         
000C  CB0000 for1:    LDWX    j,s         
000F  A80004          CPWX    4,i         ;j<4
0012  1C0025          BRGE    endFor1     
0015  0B              ASLX                
0016  360002          DECI    vector,sx   ;scanf("%d", &vector[j])
0019  CB0000          LDWX    j,s         ;j++
001C  680001          ADDX    1,i         
001F  EB0000          STWX    j,s         
0022  12000C          BR      for1        
0025  C80003 endFor1: LDWX    3,i         ;j=3
0028  EB0000          STWX    j,s         
002B  CB0000 for2:    LDWX    j,s         ;j>=0
002E  A80000          CPWX    0,i         
0031  160053          BRLT    endFor2     
0034  3B0000          DECO    j,s         ;printf("%d %d\n", j, vector[j])
0037  0B              ASLX                
0038  D00020          LDBA    ' ',i       
003B  F1FC16          STBA    charOut,d   
003E  3E0002          DECO    vector,sx   
0041  D0000A          LDBA    '\n',i      
0044  F1FC16          STBA    charOut,d   
0047  C30000          LDWA    j,s         ;j--
004A  700001          SUBA    1,i         
004D  E30000          STWA    j,s         
0050  12002B          BR      for2        
0053  50000A endFor2: ADDSP   10,i        ;pop #j #vector
0056  00              STOP                
0057                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
charOut   FC16         endFor1   0025         
endFor2   0053         for1      000C         
for2      002B         j         0000         
main      0003         vector    0002         
--------------------------------------



```