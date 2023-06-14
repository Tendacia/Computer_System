

# Source code

```C
#include <stdio.h>

char letter;

int main()
{

    scanf("%c", &letter);
    while(letter != '*')
    {
        if(letter == ' ')
        {
            printf("\n");
        }
        else
        {
            printf("%c", letter);
        }
        scanf("%c", &letter);
    }

    return 0;
}

```


# Assembly code


```Assembly code
             ; exercise 6.10

0000  120004          BR      main        
0003  00     letter:  .BLOCK  1           ; global variable #1c
0004  D1FC15 main:    LDBA    charIn,d    ; scanf("%c", &letter)
0007  F10003          STBA    letter,d    
000A  D10003 while:   LDBA    letter,d    ; while(letter != '*')
000D  B0002A          CPBA    '*',i       
0010  180031          BREQ    endWh       
0013  B00020 if:      CPBA    ' ',i       ; if(letter == ' ')
0016  1A0022          BRNE    else        
0019  D0000A          LDBA    '\n',i      ; printf("\n")
001C  F1FC16          STBA    charOut,d   
001F  120028          BR      endIf       
0022  D10003 else:    LDBA    letter,d    ; printf("%c", letter)
0025  F1FC16          STBA    charOut,d   
0028  D1FC15 endIf:   LDBA    charIn,d    ; scanf("%c", &letter)
002B  F10003          STBA    letter,d    
002E  12000A          BR      while       
0031  00     endWh:   STOP                
0032                  .END     

```


# Object Code

```Machine Code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ; exercise 6.10

0000  120004          BR      main        
0003  00     letter:  .BLOCK  1           ; global variable #1c
0004  D1FC15 main:    LDBA    charIn,d    ; scanf("%c", &letter)
0007  F10003          STBA    letter,d    
000A  D10003 while:   LDBA    letter,d    ; while(letter != '*')
000D  B0002A          CPBA    '*',i       
0010  180031          BREQ    endWh       
0013  B00020 if:      CPBA    ' ',i       ; if(letter == ' ')
0016  1A0022          BRNE    else        
0019  D0000A          LDBA    '\n',i      ; printf("\n")
001C  F1FC16          STBA    charOut,d   
001F  120028          BR      endIf       
0022  D10003 else:    LDBA    letter,d    ; printf("%c", letter)
0025  F1FC16          STBA    charOut,d   
0028  D1FC15 endIf:   LDBA    charIn,d    ; scanf("%c", &letter)
002B  F10003          STBA    letter,d    
002E  12000A          BR      while       
0031  00     endWh:   STOP                
0032                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
charIn    FC15         charOut   FC16         
else      0022         endIf     0028         
endWh     0031         if        0013         
letter    0003         main      0004         
while     000A         
--------------------------------------



```