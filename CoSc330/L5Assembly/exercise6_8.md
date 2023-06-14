

# Source code

```C
#include <stdio.h>

int main()
{
    const int limit = 100;
    int num;
    scanf("%d", &num);
    if(num >= limit)
    {
        printf("hign\n");
    }
    else
    {
        printf("low\n");
    }

    return 0;
}

```


# Assembly code

```Assembly code
;exercise 6.8

         BR      main
limit:   .EQUATE 100; constant
num:     .EQUATE 0; local variable #2d
;
main:    SUBSP   2,i; push #num
         DECI    num,s; scanf("%d, &num)
if:      LDWA    num,s; if(num >= limit)
         CPWA    limit,i
         BRLT    else
         STRO    msg1,d; printf("high\n")
         BR      endIf
else:    STRO    msg2,d; printf("low\n")
endIf:   ADDSP   2,i; pop #num
         STOP
;
msg1:    .ASCII  "high\n\x00" 
msg2:    .ASCII   "low\n\x00"
         .END

```

# Object code

```Machine code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.8

0000  120003          BR      main        
             limit:   .EQUATE 100         ; constant
             num:     .EQUATE 0           ; local variable #2d
             ;
0003  580002 main:    SUBSP   2,i         ; push #num
0006  330000          DECI    num,s       ; scanf("%d, &num)
0009  C30000 if:      LDWA    num,s       ; if(num >= limit)
000C  A00064          CPWA    limit,i     
000F  160018          BRLT    else        
0012  49001F          STRO    msg1,d      ; printf("high\n")
0015  12001B          BR      endIf       
0018  490025 else:    STRO    msg2,d      ; printf("low\n")
001B  500002 endIf:   ADDSP   2,i         ; pop #num
001E  00              STOP                
             ;
001F  686967 msg1:    .ASCII  "high\n\x00"
      680A00 

0025  6C6F77 msg2:    .ASCII  "low\n\x00" 
      0A00   

002A                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
else      0018         endIf     001B         
if        0009         limit     0064         
main      0003         msg1      001F         
msg2      0025         num       0000         
--------------------------------------



```