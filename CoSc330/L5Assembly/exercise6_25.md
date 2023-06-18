    # exercise 6.25
    # Translating non-void Function calls
    # Recursive function


    **Source Code**

    ```C
    #include <stdio.h>

    int binCoeff(int n , int k)
    {
        int y1, y2;
        if((k==0) || (n==k))
        {
            return 1;
        }
        else
        {
            y1 = binCoeff(n-1, k); //ra2
            y2 = binCoeff(n-1, k - 1); //ra3
            return y1 + y2;
        }
    } 

    int main()
    {
        printf("binCoeff(3, 1) = %d \n", binCoeff(3, 1)); //ra1
        return 0;
    }



    ```


    ```Assembly code
        ;exercise 6.25
            BR      main
    y2:      .EQUATE 0           ;local variable #2d
    y1:      .EQUATE 2           ;local variable #2d
    k:       .EQUATE 6           ;formal parameter #2d
    n:       .EQUATE 8           ;formal parameter #2d
    retVal:  .EQUATE 10          ;return value #2d
    ;
    binCoeff:SUBSP   4,i         ;push #y1 #y2 
    if:      LDWA    k,s         ;if((k==0))
            CPWA    0,i
            BREQ    then
            LDWA    n,s         ;(n==k)
            CPWA    k,s
            BRNE    else
    then:    LDWA    1,i         ;return 1
            STWA    retVal,s
            ADDSP   4,i         ;pop #y2 #y1 
            RET
    else:    LDWA    n,s         ;move n-1
            SUBA    1,i
            STWA    -4,s
            LDWA    k,s         ;move k
            STWA    -6,s
            SUBSP   6,i         ;push #retVal #n #k
            CALL    binCoeff
    ra2:     ADDSP   6,i         ;pop #k #n #retVal
            LDWA    -2,s        ;y1 = binCoeff(n-1,k)
            STWA    y1,s
            LDWA    n,s         ;move n-1
            SUBA    1,i
            STWA    -4,s
            LDWA    k,s         ;move k
            STWA    -6,s
            SUBSP   6,i         ;push #retVal #n #k
            CALL    binCoeff
    ra3:     ADDSP   6,i         ;pop #k #n #retVal
            LDWA    -2,s        ;y2 = binCoeff(n-1, k-1)
            STWA    y2,s
            LDWA    y1,s        ;return y1+y2
            ADDA    y2,s
            STWA    retVal,s
    endIf:   ADDSP   4,i         ;pop #y2 #y1
            RET
    ;main
    main:    STRO    msg,d       ;printf("binCoeff(3, 1) = )
            LDWA    3,i         ;move 3
            STWA    -4,s        
            LDWA    1,i         ;move 1
            STWA    -6,s
            SUBSP   6,i         ;push #retVal #n #k
            CALL    binCoeff    
            ADDSP   6,i         ;pop #k #n #retVal
            DECO    -2,s        ;binCoeff(3,1)
            LDBA    '\n',i      ;printf('\n')
            STBA    charOut,d
            STOP
    msg:     .ASCII  "binCoeff(3, 1) = "
            .END 

    ```

    ```Machine Code
    -------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.25
0000  12006B          BR      main        
             y2:      .EQUATE 0           ;local variable #2d
             y1:      .EQUATE 2           ;local variable #2d
             k:       .EQUATE 6           ;formal parameter #2d
             n:       .EQUATE 8           ;formal parameter #2d
             retVal:  .EQUATE 10          ;return value #2d
             ;
0003  580004 binCoeff:SUBSP   4,i         ;push #y1 #y2
0006  C30006 if:      LDWA    k,s         ;if((k==0))
0009  A00000          CPWA    0,i         
000C  180018          BREQ    then        
000F  C30008          LDWA    n,s         ;(n==k)
0012  A30006          CPWA    k,s         
0015  1A0022          BRNE    else        
0018  C00001 then:    LDWA    1,i         ;return 1
001B  E3000A          STWA    retVal,s    
001E  500004          ADDSP   4,i         ;pop #y2 #y1
0021  01              RET                 
0022  C30008 else:    LDWA    n,s         ;move n-1
0025  700001          SUBA    1,i         
0028  E3FFFC          STWA    -4,s        
002B  C30006          LDWA    k,s         ;move k
002E  E3FFFA          STWA    -6,s        
0031  580006          SUBSP   6,i         ;push #retVal #n #k
0034  240003          CALL    binCoeff    
0037  500006 ra2:     ADDSP   6,i         ;pop #k #n #retVal
003A  C3FFFE          LDWA    -2,s        ;y1 = binCoeff(n-1,k)
003D  E30002          STWA    y1,s        
0040  C30008          LDWA    n,s         ;move n-1
0043  700001          SUBA    1,i         
0046  E3FFFC          STWA    -4,s        
0049  C30006          LDWA    k,s         ;move k
004C  E3FFFA          STWA    -6,s        
004F  580006          SUBSP   6,i         ;push #retVal #n #k
0052  240003          CALL    binCoeff    
0055  500006 ra3:     ADDSP   6,i         ;pop #k #n #retVal
0058  C3FFFE          LDWA    -2,s        ;y2 = binCoeff(n-1, k-1)
005B  E30000          STWA    y2,s        
005E  C30002          LDWA    y1,s        ;return y1+y2
0061  630000          ADDA    y2,s        
0064  E3000A          STWA    retVal,s    
0067  500004 endIf:   ADDSP   4,i         ;pop #y2 #y1
006A  01              RET                 
             ;main
006B  49008D main:    STRO    msg,d       ;printf("binCoeff(3, 1) = )
006E  C00003          LDWA    3,i         ;move 3
0071  E3FFFC          STWA    -4,s        
0074  C00001          LDWA    1,i         ;move 1
0077  E3FFFA          STWA    -6,s        
007A  580006          SUBSP   6,i         ;push #retVal #n #k
007D  240003          CALL    binCoeff    
0080  500006          ADDSP   6,i         ;pop #k #n #retVal
0083  3BFFFE          DECO    -2,s        ;binCoeff(3,1)
0086  D0000A          LDBA    '\n',i      ;printf('\n')
0089  F1FC16          STBA    charOut,d   
008C  00              STOP                
008D  62696E msg:     .ASCII  "binCoeff(3, 1) = "
      436F65 
      666628 
      332C20 
      312920 
      3D20   

009E                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
binCoeff  0003         charOut   FC16         
else      0022         endIf     0067         
if        0006         k         0006         
main      006B         msg       008D         
n         0008         ra2       0037         
ra3       0055         retVal    000A         
then      0018         y1        0002         
y2        0000         
--------------------------------------





    ```


