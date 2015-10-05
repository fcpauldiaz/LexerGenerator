# LexerGenerator
El proyecto consiste en la implementación de un generador de analizadores léxicos, tomando como base un subconjunto de las características de COCO/R 

El analizador léxico a generar, y dará como salida un archivo fuente en
el lenguaje de elección del programador, el cual implementará el scanner basado en la
especificación léxica ingresada. Este archivo será compilado y ejecutado para determinar
su validez.

##Vocabulario de Cocol (predeclarado)

ident = letter {letter | digit}.

number = digit {digit}.

string = '"' {anyButQuote} '"'.

char = '\'' anyButApostrophe '\''.

anyButQuote = ANY – '"

anyButApostrophe = ANY - '\'

##Sintaxis de Cocol

Cocol = "COMPILER" ident

#####ScannerSpecification

#####ParserSpecification (este proyecto no lo incluye)

"END" ident '.'.

#####ScannerSpecification =

[ "CHARACTERS" { SetDecl } ]

[ "KEYWORDS" { KeywordDecl } ]

[ "TOKENS" { TokenDecl } ]

{ WhiteSpaceDecl }.

SetDecl = ident '=' Set '.'.

Set = BasicSet { ('+'|'-') BasicSet }.

BasicSet = string | ident | Char [ ".." Char ].

Char = char | "CHR" '(' number ')'.

KeywordDecl = ident '=' string '.'.

TokenDecl = ident [ '=' TokenExpr ] [ "EXCEPT KEYWORDS" ] '.'.

TokenExpr = TokenTerm { '|' TokenTerm }.

TokenTerm = TokenFactor { TokenFactor }.

TokenFactor = Symbol
 | '(' TokenExpr ')'
 | '[' TokenExpr ']'
 | '{' TokenExpr '}'.
 
Symbol = ident | string | char.

WhiteSpaceDecl = "IGNORE" Set '.'.

#####ParserSpecification = "PRODUCTIONS" { Production }. (no lo incluye)

Production = ident [ Attributes ] [ SemAction ] '=' Expression '.'.

Expression = Term { '|' Term }.

Term = Factor { Factor }.

Factor = Symbol [ Attributes ]
| '(' Expression ')'
 | '[' Expression ']'
 | '{' Expression '}'
| SemAction.

Attributes = "<." {ANY} ".>".

SemAction = "(." {ANY} ".)".

13/09/2015
