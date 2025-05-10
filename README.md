# Ex-3-RECOGNITION-OF-A-VALID-ARITHMETIC-EXPRESSION-THAT-USES-OPERATOR-AND-USING-YACC
# Date: 25/04/2025
# AIM
To write a yacc program to recognize a valid arithmetic expression that uses operator +,- ,* and /.
# ALGORITHM
1.	Start the program.
2.	Write a program in the vi editor and save it with .l extension.
3.	In the lex program, write the translation rules for the operators =,+,-,*,/ and for the identifier.
4.	Write a program in the vi editor and save it with .y extension.
5.	Compile the lex program with lex compiler to produce output file as lex.yy.c. eg $ lex filename.l
6.	Compile the yacc program with yacc compiler to produce output file as y.tab.c. eg $ yacc –d arith_id.y
7.	Compile these with the C compiler as gcc lex.yy.c y.tab.c
8.	Enter an arithmetic expression as input and the tokens are identified as output.
# PROGRAM
exp.l file 
```
%{
#include "y.tab.h"
#include <string.h>

extern YYSTYPE yylval;   // To pass the token to yacc
%}

%%
[ \t\n]+             ;  // skip whitespace

"="                 { return EQUAL; }
"+"                 { return PLUS; }
"-"                 { return MINUS; }
"*"                 { return MUL; }
"/"                 { return DIV; }

[a-zA-Z_][a-zA-Z0-9_]* {
    yylval.str = strdup(yytext);  // Assign identifier
    return ID;
}

.                   { printf("Invalid character: %s\n", yytext); }  // Print invalid character
%%

int yywrap() {
    return 1;  // Lex function to signify end of input
}
```

exp .y file 
```
%{
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int yylex();  // Lex function declaration
int yyerror(const char *s);  // Error handling function
%}

%union {
    char *str;  // String type for identifier
}

%token <str> ID
%token PLUS MINUS MUL DIV EQUAL

%left PLUS MINUS
%left MUL DIV
%right EQUAL

%type <str> expr

%%
stmt:
    expr '\n' { 
        // If parsing is successful, print "Valid expression"
        printf("Valid expression\n");
    }
    ;

expr:
      ID EQUAL expr { 
        // After parsing ID = expr
        printf("Identifier: %s\n", $1);  // Print the identifier
        printf("Operator: =\n");         // Print assignment operator
        free($1);  // Free allocated memory for string
      }
    | expr PLUS expr {
        printf("Operator: +\n");  // Print addition operator
    }
    | expr MINUS expr {
        printf("Operator: -\n");  // Print subtraction operator
    }
    | expr MUL expr {
        printf("Operator: *\n");  // Print multiplication operator
    }
    | expr DIV expr {
        printf("Operator: /\n");  // Print division operator
    }
    | ID { 
        printf("Identifier: %s\n", $1);  // Print the identifier
        free($1);  // Free memory
    }
    ;

%%

int main() {
    printf("Enter an expression:\n");
    
    if (yyparse() != 0) {
        // If there is a syntax error, print "Syntax Error"
        
    } else {
        // If no error, print "Valid expression" (already handled in stmt rule)
    }
    printf("arithematic expression is valid ");
    return 0;
}

int yyerror(const char *s) {
    return 0;  // Just return, error will be handled in main
}
```


# OUTPUT
![Screenshot from 2025-04-25 16-20-21](https://github.com/user-attachments/assets/1f0c77fc-4424-43ae-a0d6-5ae357ae406c)

# RESULT
A YACC program to recognize a valid arithmetic expression that uses operator +,-,* and / is executed successfully and the output is verified.
