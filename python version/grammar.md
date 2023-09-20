statements  : NEWLINE* statement (NEWLINE+ statement)* NEWLINE*

statement		: KEYWORD:Return expr?
						: KEYWORD:Continue
						: KEYWORD:Break
						: expr

expr        : KEYWORD:Var IDENTIFIER EQ expr
            : comp-expr ((KEYWORD:And|KEYWORD:Or) comp-expr)*

comp-expr   : NOT comp-expr
            : arith-expr ((EE|LT|GT|LTE|GTE) arith-expr)*

arith-expr  :	term ((PLUS|MINUS) term)*

term        : factor ((MUL|DIV) factor)*

factor      : (PLUS|MINUS) factor
            : power

power       : call (POW factor)*

call        : atom (LPAREN (expr (COMMA expr)*)? RPAREN)?

atom        : INT|FLOAT|STRING|IDENTIFIER
            : LPAREN expr RPAREN
            : list-expr
            : if-expr
            : for-expr
            : while-expr
            : func-def

list-expr   : LSQUARE (expr (COMMA expr)*)? RSQUARE

if-expr     : KEYWORD:If expr KEYWORD:Then
              (statement if-expr-b|if-expr-c?)
            | (NEWLINE statements KEYWORD:End|if-expr-b|if-expr-c)

if-expr-b   : KEYWORD:Elif expr KEYWORD:Then
              (statement if-expr-b|if-expr-c?)
            | (NEWLINE statements KEYWORD:End|if-expr-b|if-expr-c)

if-expr-c   : KEYWORD:Else
              statement
            | (NEWLINE statements KEYWORD:End)

for-expr    : KEYWORD:For IDENTIFIER EQ expr KEYWORD:To expr 
              (KEYWORD:Step expr)? KEYWORD:Then
              statement
            | (NEWLINE statements KEYWORD:End)

while-expr  : KEYWORD:While expr KEYWORD:Then
              statement
            | (NEWLINE statements KEYWORD:End)

func-def    : KEYWORD:Function IDENTIFIER?
              LPAREN (IDENTIFIER (COMMA IDENTIFIER)*)? RPAREN
              (ARROW expr)
            | (NEWLINE statements KEYWORD:End)

* = Zero or More
? = Optional