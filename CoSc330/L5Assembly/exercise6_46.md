**Translating Structers**
**exercise 6.46**

**Translation rules for global structures**
* It equates each field of the structer to its offset from the first byte of the structure
* To allocate storage for the structure, it generates .BLOCK tot where tot is the total number of bytes occupied by the structure
* To get a field of the structer, it generates LDWX to load the offset of the field into the index register with immediate addressing, followed by an LDBA or LDWA instruction with indexed addressing

**Translation rules for local structures**
* similar to global structures


**Source code**

```C

#include <stdio.h>

struct person
{
    char first;
    char last;
    int age;
    char gender;
};

struct person bill;

int main()
{
    scanf("%c%c%d %c", &bill.first, &bill.last, &bill.age, &bill.gender);
    printf("Initials: %c%c\n", bill.first, bill.last);
    printf("Age: %d\n", bill.age);
    printf("Gender: ");
    if(bill.gender == 'm')
    {
        printf("male\n");
    }
    else
    {
        printf("female\n");
    }


    return 0;
}



```



**Assembly code**

```Assembly code
;exercise 6.46
         BR      main        
first:   .EQUATE 0           ;struct field #1c
last:    .EQUATE 1           ;struct field #1c
age:     .EQUATE 2           ;struct field #2d
gender:  .EQUATE 4           ;struct field #1c
bill:    .BLOCK  5           ;global #first #last #age #gender
;main
main:    LDWX    first,i     ;scanf("%c%c%d %c",
         LDBA    charIn,d    
         STBA    bill,x      
         LDWX    last,i      
         LDBA    charIn,d    
         STBA    bill,x      
         LDWX    age,i       
         DECI    bill,x      
         LDBA    ' ',i       
         STBA    charOut,d   
         LDWX    gender,x    
         LDBA    charIn,d    
         STBA    bill,x      
         STRO    msg1,d      ;printf("Initials: %c%c\n", bill.first, bill.last)
         LDWX    first,i     
         LDBA    bill,x      
         STBA    charOut,d   
         LDWX    last,i      
         LDBA    bill,x      
         STBA    charOut,d   
         LDBA    '\n',i      
         STBA    charOut,d   
         STRO    msg2,d      ;printf("Age: %d\n", bill.age)
         LDWX    age,i       
         DECO    bill,x      
         LDBA    '\n',i      
         STBA    charOut,d   
         STRO    msg3,d      ;printf("Gender: ")
if:      LDWX    gender,i    ;if(bill.gender == 'm')
         LDBA    bill,x      
         CPBA    'm',i       
         BRNE    else        
         STRO    msg4,d      ;printf("male\n")
         BR      endIf       
else:    STRO    msg5,d      ;printf("female\n")
endIf:   STOP                
;
msg1:    .ASCII  "Initials: \x00"

msg2:    .ASCII  "Age: \x00" 

msg3:    .ASCII  "Gender: \x00"

msg4:    .ASCII  "male\n\x00"

msg5:    .ASCII  "female\n\x00"

         .END                  



```



**Machine code**

```Machine code
-------------------------------------------------------------------------------
      Object
Addr  code   Symbol   Mnemon  Operand     Comment
-------------------------------------------------------------------------------
             ;exercise 6.46
0000  120008          BR      main        
             first:   .EQUATE 0           ;struct field #1c
             last:    .EQUATE 1           ;struct field #1c
             age:     .EQUATE 2           ;struct field #2d
             gender:  .EQUATE 4           ;struct field #1c
0003  000000 bill:    .BLOCK  5           ;global #first #last #age #gender
      0000   
             ;main
0008  C80000 main:    LDWX    first,i     ;scanf("%c%c%d %c",
000B  D1FC15          LDBA    charIn,d    
000E  F50003          STBA    bill,x      
0011  C80001          LDWX    last,i      
0014  D1FC15          LDBA    charIn,d    
0017  F50003          STBA    bill,x      
001A  C80002          LDWX    age,i       
001D  350003          DECI    bill,x      
0020  D00020          LDBA    ' ',i       
0023  F1FC16          STBA    charOut,d   
0026  CD0004          LDWX    gender,x    
0029  D1FC15          LDBA    charIn,d    
002C  F50003          STBA    bill,x      
002F  490072          STRO    msg1,d      ;printf("Initials: %c%c\n", bill.first, bill.last)
0032  C80000          LDWX    first,i     
0035  D50003          LDBA    bill,x      
0038  F1FC16          STBA    charOut,d   
003B  C80001          LDWX    last,i      
003E  D50003          LDBA    bill,x      
0041  F1FC16          STBA    charOut,d   
0044  D0000A          LDBA    '\n',i      
0047  F1FC16          STBA    charOut,d   
004A  49007D          STRO    msg2,d      ;printf("Age: %d\n", bill.age)
004D  C80002          LDWX    age,i       
0050  3D0003          DECO    bill,x      
0053  D0000A          LDBA    '\n',i      
0056  F1FC16          STBA    charOut,d   
0059  490083          STRO    msg3,d      ;printf("Gender: ")
005C  C80004 if:      LDWX    gender,i    ;if(bill.gender == 'm')
005F  D50003          LDBA    bill,x      
0062  B0006D          CPBA    'm',i       
0065  1A006E          BRNE    else        
0068  49008C          STRO    msg4,d      ;printf("male\n")
006B  120071          BR      endIf       
006E  490092 else:    STRO    msg5,d      ;printf("female\n")
0071  00     endIf:   STOP                
             ;
0072  496E69 msg1:    .ASCII  "Initials: \x00"
      746961 
      6C733A 
      2000   

007D  416765 msg2:    .ASCII  "Age: \x00" 
      3A2000 

0083  47656E msg3:    .ASCII  "Gender: \x00"
      646572 
      3A2000 

008C  6D616C msg4:    .ASCII  "male\n\x00"
      650A00 

0092  66656D msg5:    .ASCII  "female\n\x00"
      616C65 
      0A00   

009A                  .END                  
-------------------------------------------------------------------------------


Symbol table
--------------------------------------
Symbol    Value        Symbol    Value
--------------------------------------
age       0002         bill      0003         
charIn    FC15         charOut   FC16         
else      006E         endIf     0071         
first     0000         gender    0004         
if        005C         last      0001         
main      0008         msg1      0072         
msg2      007D         msg3      0083         
msg4      008C         msg5      0092         
--------------------------------------


```

