**Tanslating Global Arrays**
**exercise 6.34**

**Source Code**

```C
#include <stdio.h>

int vector[4];
int j;

int main()
{
    for (j = 0; i < 4; j++)
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
;exercise 6.34
         BR      main
vector:  .BLOCK  8           ;global  #2d4a
j:       .BLOCK  2           ;global  #2d
;main
main:    LDWA    0,i         ;j=0
         STWA    j,d
for1:    LDWX    j,d         ;j<4
         CPWX    4,i
         BRGE    endFor1
         ASLX
         DECI    vector,x    ;scanf("%d", &vector[j])
         LDWA    j,d
         ADDA    1,i
         STWA    j,d
         BR      for1
endFor1: LDWA    3,i         ;j = 3
         STWA    j,d         
for2:    LDWX    j,d
         CPWX    0,i
         BRLT    endFor2
         ASLX
         DECO    j,d         ;printf("%d %d\n",j , vector[j])
         LDBA    ' ',i
         STBA    charOut,d
         DECO    vector,x
         LDBA    '\n',i
         STBA    charOut,d
         LDWA    j,d
         SUBA    1,i
         STWA    j,d
         BR      for2
endFor2: STOP
         .END
         
```


**Machine code**

```Machine code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.34
0000  12000D          BR      main        
0003  000000 vector:  .BLOCK  8           ;global  #2d4a
      000000 
      0000   
000B  0000   j:       .BLOCK  2           ;global  #2d
             ;main
000D  C00000 main:    LDWA    0,i         ;j=0
0010  E1000B          STWA    j,d         
0013  C9000B for1:    LDWX    j,d         ;j<4
0016  A80004          CPWX    4,i         
0019  1C002C          BRGE    endFor1     
001C  0B              ASLX                
001D  350003          DECI    vector,x    ;scanf("%d", &vector[j])
0020  C1000B          LDWA    j,d         
0023  600001          ADDA    1,i         
0026  E1000B          STWA    j,d         
0029  120013          BR      for1        
002C  C00003 endFor1: LDWA    3,i         ;j = 3
002F  E1000B          STWA    j,d         
0032  C9000B for2:    LDWX    j,d         
0035  A80000          CPWX    0,i         
0038  16005A          BRLT    endFor2     
003B  0B              ASLX                
003C  39000B          DECO    j,d         ;printf("%d %d\n",j , vector[j])
003F  D00020          LDBA    ' ',i       
0042  F1FC16          STBA    charOut,d   
0045  3D0003          DECO    vector,x    
0048  D0000A          LDBA    '\n',i      
004B  F1FC16          STBA    charOut,d   
004E  C1000B          LDWA    j,d         
0051  700001          SUBA    1,i         
0054  E1000B          STWA    j,d         
0057  120032          BR      for2        
005A  00     endFor2: STOP                
005B                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
charOut   FC16         endFor1   002C         
endFor2   005A         for1      0013         
for2      0032         j         000B         
main      000D         vector    0003         
--------------------------------------


```

