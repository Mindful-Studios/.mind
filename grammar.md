# Parser

expr : KEYWORD:var IDENTIFIER EQ expr
     : comp-expr ((KEYWORD:AND|KEYWORD:OR) comp-expr)*

comp-expr : NOT comp-expr
          : arith-expr ((EE|LT|GT|LTE|GTE) arith-expr)*

arith-expr : term ((PLUS|MINUS) term)*

term : factor ((MUL|DIV) factor)*

factor : (PLUS|MINUS) factor
       : power

power : call ((POW) factor)*

call : atom (LPAREN (expr (COMMA expr)*)? RPAREN)?

atom : INT|FLOAT|STRING|IDENTIFIER
     : LPAREN expr RPAREN
     : list-expr
     : if-expr
     : for-expr
     : while-expr
     : func-def

list-expr : LSQUARE (expr (COMMA expr)*)? RSQUARE

if-expr : KEYWORD:IF expr KEYWORD:THEN expr
          (KEYWORD:ELIF expr KEYWORD:THEN expr)*
          (KEYWORD:ELSE expr)?

for-expr : KEYWORD:FOR IDENTIFIER EQ expr KEYWORD:TO expr
           (KEYWORD:STEP expr)? KEYWORD:THEN expr

while-expr : KEYWORD:WHILE expr KEYWORD:THEN expr

func-def :KEYWORD:FUN IDENTIFIER?
          LPAREN (IDENTIFIER (COMMA IDENTIFIER)*)? RPAREN
          ARROW expr

* = Zero or More
? = Optional

# Other Stuff
**List**
LBRACKET (IDENTIFIER (COMMA IDENTIFIER)*)? RBRACKET

+ = append(INT|FLOAT|STRING|IDENTIFIER)
- = remove(INT:INDEX)
* = append_list(LIST)
/ = get(INT:INDEX)

[]
[1, 2, 3, 4, 5]

[1, 2, 3] + 4 => [1, 2, 3, 4]
[1, 2, 3] * [4, 5, 6] => [1, 2, 3, 4, 5, 6]
[1, 2, 3] - 0 => [1, 2]
[1, 2, 3] - 1 => [1, 3]
[1, 2, 3] - -1 => [1, 2]
[1, 2, 3] / 0 =>  1
[1, 2, 3] / 1 => 2
[1, 2, 3] / -1 => 3