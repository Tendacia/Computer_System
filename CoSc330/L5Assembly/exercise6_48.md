**Translating Linked Data Sturctures**
**exercise 6.48**

**Source code**

```C
#include <stdio.h>
#include <stdlib.h>

struct node
{
    int data;
    struct node * next;
}

int main()
{
    struct node * first, *p;
    int value;
    first = 0;
    scanf("%d", &value);
    while(value != -9999)
    {

    }
}


```



**Assembly code**

```Assembly code
;exercise 6.48
         BR      main
data:    .EQUATE 0           ;struct field #2d
next:    .EQUATE 2           ;struct field #2h
first:   .EQUATE 4           ;local #2h
p:       .EQUATE 2           ;local #2h
value:   .EQUATE 0           ;local #2d
;
main:    SUBSP   6,i         ;push #first #p #value
         LDWA    0,i         ;first = 0
         STWA    first,s
         DECI    value,s     ;scanf("%d", &value)
while:   LDWA    value,s     ;while(value != -9999)
         CPWA    -9999,i
         BREQ    endWh
         LDWA    first,s     ;p = first
         STWA    p,s
         LDWA    4,i         ;first = (struct node *) malloc(sizeof (struct node))         
         CALL    malloc      ;malloc #2d #2h
         STWX    first,s
         LDWX    data,i      ;first->data = value
         LDWA    value,s
         STWA    first,sfx
         LDWA    p,s         ;first->next = p
         LDWX    next,i
         STWA    first,sfx
         DECI    value,s     ;scanf("%d", &value)
         BR      while
endWh:   LDWA    first,s     ;p = first
         STWA    p,s         
for:     LDWA    p,s         ;p != 0
         CPWA    0,i
         BREQ    endFor
         LDWX    data,i      ;printf("%d ", p->data)
         DECO    p,sfx
         LDBA    ' ',i
         STBA    charOut,d
         LDWX    next,i      ;p = p->next
         LDWA    p,sfx
         STWA    p,s
         BR      for
endFor:  STOP
;malloc
malloc:  LDWX    hpPtr,d     ;returned pointer to bytes
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
             ;exercise 6.48
0000  120003          BR      main        
             data:    .EQUATE 0           ;struct field #2d
             next:    .EQUATE 2           ;struct field #2h
             first:   .EQUATE 4           ;local #2h
             p:       .EQUATE 2           ;local #2h
             value:   .EQUATE 0           ;local #2d
             ;
0003  580006 main:    SUBSP   6,i         ;push #first #p #value
0006  C00000          LDWA    0,i         ;first = 0
0009  E30004          STWA    first,s     
000C  330000          DECI    value,s     ;scanf("%d", &value)
000F  C30000 while:   LDWA    value,s     ;while(value != -9999)
0012  A0D8F1          CPWA    -9999,i     
0015  18003F          BREQ    endWh       
0018  C30004          LDWA    first,s     ;p = first
001B  E30002          STWA    p,s         
001E  C00004          LDWA    4,i         ;first = (struct node *) malloc(sizeof (struct node))
0021  240067          CALL    malloc      ;malloc #2d #2h
0024  EB0004          STWX    first,s     
0027  C80000          LDWX    data,i      ;first->data = value
002A  C30000          LDWA    value,s     
002D  E70004          STWA    first,sfx   
0030  C30002          LDWA    p,s         ;first->next = p
0033  C80002          LDWX    next,i      
0036  E70004          STWA    first,sfx   
0039  330000          DECI    value,s     ;scanf("%d", &value)
003C  12000F          BR      while       
003F  C30004 endWh:   LDWA    first,s     ;p = first
0042  E30002          STWA    p,s         
0045  C30002 for:     LDWA    p,s         ;p != 0
0048  A00000          CPWA    0,i         
004B  180066          BREQ    endFor      
004E  C80000          LDWX    data,i      ;printf("%d ", p->data)
0051  3F0002          DECO    p,sfx       
0054  D00020          LDBA    ' ',i       
0057  F1FC16          STBA    charOut,d   
005A  C80002          LDWX    next,i      ;p = p->next
005D  C70002          LDWA    p,sfx       
0060  E30002          STWA    p,s         
0063  120045          BR      for         
0066  00     endFor:  STOP                
             ;malloc
0067  C90071 malloc:  LDWX    hpPtr,d     ;returned pointer to bytes
006A  610071          ADDA    hpPtr,d     ;allocate from heap
006D  E10071          STWA    hpPtr,d     ;update hpPtr
0070  01              RET                 
0071  0073   hpPtr:   .ADDRSS heap        ;address of next free byte
0073  00     heap:    .BLOCK  1           ;first byte in the heap
0074                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
charOut   FC16         data      0000         
endFor    0066         endWh     003F         
first     0004         for       0045         
heap      0073         hpPtr     0071         
main      0003         malloc    0067         
next      0002         p         0002         
value     0000         while     000F         
--------------------------------------



```