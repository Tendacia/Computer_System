
# Source code

```C
#include <stdio.h>

int main()
{
    int number;
    scanf("%d", &number);
    if(number < 0)
    {
        number = -number;
    }
    printf("%d", number);
    return 0;
}


```

# Assembly Code

```Assembly code
;exercise 6.6

         BR      main
number:  .EQUATE 0; local variable #2d
;
main:    SUBSP   2,i; push #number
         DECI    number,s; scanf("%d", &number)
if:      LDWA    number,s; if(number < 0)
         BRGE    endIf
         LDWA    number,s; number = -number
         NEGA    
         STWA    number,s
endIf:   DECO    number,s; printf("%d", number)
         ADDSP   2,i; pop #number
         STOP
         .END




```


# Object code

```Machine code

-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.6

0000  120003          BR      main        
             number:  .EQUATE 0           ; local variable #2d
             ;
0003  580002 main:    SUBSP   2,i         ; push #number
0006  330000          DECI    number,s    ; scanf("%d", &number)
0009  C30000 if:      LDWA    number,s    ; if(number < 0)
000C  1C0016          BRGE    endIf       
000F  C30000          LDWA    number,s    ; number = -number
0012  08              NEGA                
0013  E30000          STWA    number,s    
0016  3B0000 endIf:   DECO    number,s    ; printf("%d", number)
0019  500002          ADDSP   2,i         ; pop #number
001C  00              STOP                
001D                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
endIf     0016         if        0009         
main      0003         number    0000         
--------------------------------------





```