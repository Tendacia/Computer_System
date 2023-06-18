**Translating Arrays Passed as Parameters**
**exercise6.38**

**Source code**

```C

#include <stdio.h>

void getVect(int v[], int *n)
{
    int j;
    scanf("%d", n);
    for(j = 0; j < *n; j++)
    {
        scanf("%d", &v[j]);
    }
}

void putVect(int v[], int n)
{
    int j;
    for (j = 0; j < n; j++)
    {
        printf("%d ", v[j]);
    }
    printf("\n");
}

int main()
{
    int vector[0];
    int numItms;
    getVect(vector, &numItms);
    putVect(vector, numItms);
    return 0;
}

```


**Assembly code**

```Assembly code
;exercise 6.38
         BR      main        
v:       .EQUATE 6           ;formal parameter #2h
n:       .EQUATE 4           ;formal parameter #2h
j:       .EQUATE 0           ;local variable #2d
getVect: SUBSP   2,i         ;push #j
         DECI    n,sf        ;scanf("%d", n)
         LDWX    0,i         ;j=0
         STWX    j,s         
for1:    LDWX    j,s         ;j < *n
         CPWX    n,sf        
         BRGE    endFor1     
         ASLX                
         DECI    v,sfx       ;scanf("%d", &v[j])
         LDWX    j,s         ;j++
         ADDX    1,i         
         STWX    j,s         
         BR      for1        
endFor1: ADDSP   2,i         ;pop #j
         RET                 
;putVect
v2:      .EQUATE 6           ;formal parameter #2h
n2:      .EQUATE 4           ;formal parameter #2d
j2:      .EQUATE 0           ;local variable #2d
putVect: SUBSP   2,i         ;push #j2
         LDWX    0,i         ;j=0
         STWX    j2,s        
for2:    LDWX    j2,s        ;j < n
         CPWX    n2,s        ;j<n
         BRGE    endFor2     
         ASLX                
         DECO    v2,sfx      ;printf("%d ", v[j])
         LDBA    ' ',i       
         STBA    charOut,d   
         LDWX    j2,s        ;j++
         ADDX    1,i         
         STWX    j2,s        
         BR      for2        
endFor2: LDBA    '\n',i      ;printf("\n")
         STBA    charOut,d   
         ADDSP   2,i         ;pop #j2
         RET                 
;main
vector:  .EQUATE 2           ;local variable #2d8a
numItms: .EQUATE 0           ;local variable #2d
main:    SUBSP   18,i        ;push #vector #numItms
         MOVSPA              ;move &vector
         ADDA    vector,i    
         STWA    -2,s        
         MOVSPA              ;move &numItms
         ADDA    numItms,i   
         STWA    -4,s        
         SUBSP   4,i         ;push #v #n
         CALL    getVect     
         ADDSP   4,i         ;pop #n #v
         MOVSPA              ;move &vector
         ADDA    vector,i    
         STWA    -2,s        
         LDWA    numItms,s   ;move numItms
         STWA    -4,s        
         SUBSP   4,i         ;push #v2 #n2
         CALL    putVect     
         ADDSP   4,i         ;pop #n2 #v2
         ADDSP   18,i        ;pop #numItms #vector
         STOP                
         .END                  



```


**Machine code**

```Machine code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.38
0000  12005E          BR      main        
             v:       .EQUATE 6           ;formal parameter #2h
             n:       .EQUATE 4           ;formal parameter #2h
             j:       .EQUATE 0           ;local variable #2d
0003  580002 getVect: SUBSP   2,i         ;push #j
0006  340004          DECI    n,sf        ;scanf("%d", n)
0009  C80000          LDWX    0,i         ;j=0
000C  EB0000          STWX    j,s         
000F  CB0000 for1:    LDWX    j,s         ;j < *n
0012  AC0004          CPWX    n,sf        
0015  1C0028          BRGE    endFor1     
0018  0B              ASLX                
0019  370006          DECI    v,sfx       ;scanf("%d", &v[j])
001C  CB0000          LDWX    j,s         ;j++
001F  680001          ADDX    1,i         
0022  EB0000          STWX    j,s         
0025  12000F          BR      for1        
0028  500002 endFor1: ADDSP   2,i         ;pop #j
002B  01              RET                 
             ;putVect
             v2:      .EQUATE 6           ;formal parameter #2h
             n2:      .EQUATE 4           ;formal parameter #2d
             j2:      .EQUATE 0           ;local variable #2d
002C  580002 putVect: SUBSP   2,i         ;push #j2
002F  C80000          LDWX    0,i         ;j=0
0032  EB0000          STWX    j2,s        
0035  CB0000 for2:    LDWX    j2,s        ;j < n
0038  AB0004          CPWX    n2,s        ;j<n
003B  1C0054          BRGE    endFor2     
003E  0B              ASLX                
003F  3F0006          DECO    v2,sfx      ;printf("%d ", v[j])
0042  D00020          LDBA    ' ',i       
0045  F1FC16          STBA    charOut,d   
0048  CB0000          LDWX    j2,s        ;j++
004B  680001          ADDX    1,i         
004E  EB0000          STWX    j2,s        
0051  120035          BR      for2        
0054  D0000A endFor2: LDBA    '\n',i      ;printf("\n")
0057  F1FC16          STBA    charOut,d   
005A  500002          ADDSP   2,i         ;pop #j2
005D  01              RET                 
             ;main
             vector:  .EQUATE 2           ;local variable #2d8a
             numItms: .EQUATE 0           ;local variable #2d
005E  580012 main:    SUBSP   18,i        ;push #vector #numItms
0061  03              MOVSPA              ;move &vector
0062  600002          ADDA    vector,i    
0065  E3FFFE          STWA    -2,s        
0068  03              MOVSPA              ;move &numItms
0069  600000          ADDA    numItms,i   
006C  E3FFFC          STWA    -4,s        
006F  580004          SUBSP   4,i         ;push #v #n
0072  240003          CALL    getVect     
0075  500004          ADDSP   4,i         ;pop #n #v
0078  03              MOVSPA              ;move &vector
0079  600002          ADDA    vector,i    
007C  E3FFFE          STWA    -2,s        
007F  C30000          LDWA    numItms,s   ;move numItms
0082  E3FFFC          STWA    -4,s        
0085  580004          SUBSP   4,i         ;push #v2 #n2
0088  24002C          CALL    putVect     
008B  500004          ADDSP   4,i         ;pop #n2 #v2
008E  500012          ADDSP   18,i        ;pop #numItms #vector
0091  00              STOP                
0092                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
charOut   FC16         endFor1   0028         
endFor2   0054         for1      000F         
for2      0035         getVect   0003         
j         0000         j2        0000         
main      005E         n         0004         
n2        0004         numItms   0000         
putVect   002C         v         0006         
v2        0006         vector    0002         
--------------------------------------



```