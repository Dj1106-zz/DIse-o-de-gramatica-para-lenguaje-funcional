
Cuestionario de lenguaje de gramática funcional.

Diseño del Lenguaje Funcional: AURORA

Este repositorio describe el análisis léxico, el modelo de autómata (DFA) y la gramática sintáctica del lenguaje funcional AURORA, diseñado bajo los principios de la teoría de compiladores.

AURORA es un lenguaje funcional minimalista que permite definir variables, evaluar expresiones aritméticas, condicionales y funciones.

1. Análisis Léxico (Fase de Scanning)

El análisis léxico transforma el flujo de caracteres del programa fuente en una secuencia de tokens que luego serán usados por el analizador sintáctico.

Cada token representa una categoría léxica con significado propio.

Especificación de Tokens
Token	Descripción	Expresión Regular
T_Kw	Palabras reservadas	let, in, if, then, else, fn
T_Ident	Identificadores	[a-z][a-zA-Z0-9_]*
T_Int	Constantes enteras	[0-9]+
T_Bool	Constantes booleanas	true, false
T_Assign	Asignación	=
T_Arrow	Flecha de función	=>
T_OpArit	Operadores aritméticos	+, -, *, /
T_OpRel	Operadores relacionales	==, <, >
T_Par	Paréntesis	(, )
T_Comma	Separador	,
Resolución de Conflictos

El analizador aplica:

Maximal Munch: siempre se reconoce el lexema más largo.

Prioridad: las palabras reservadas se reconocen antes que los identificadores.

Descartes: espacios, saltos de línea y tabulaciones se ignoran.

2. Implementación con Autómatas (DFA)

El analizador léxico se basa en autómatas finitos deterministas.

DFA para Identificadores y Enteros

Estados:

q0 → estado inicial

q1 → estado de aceptación para enteros

q2 → estado de aceptación para identificadores

Transiciones:

Desde	Símbolo	Hacia
q0	0–9	q1
q0	a–z, A–Z	q2
q1	0–9	q1
q2	a–z, A–Z, 0–9, _	q2

Estados finales:

q1 → T_Int

q2 → T_Ident

3. Gramática Sintáctica (EBNF)

La gramática define cómo se estructuran los programas de AURORA.

Programa    ::= { Definicion }

Definicion  ::= "let" T_Ident "=" Expresion

Expresion   ::= T_Int
              | T_Bool
              | T_Ident
              | LetExpr
              | IfExpr
              | Lambda
              | Call
              | Expresion OpArit Expresion

LetExpr     ::= "let" T_Ident "=" Expresion "in" Expresion

IfExpr      ::= "if" Expresion "then" Expresion "else" Expresion

Lambda      ::= "fn" T_Ident "=>" Expresion

Call        ::= T_Ident "(" Expresion { "," Expresion } ")"

OpArit      ::= "+" | "-" | "*" | "/"

4. Interpretación

El lenguaje es expresivo y funcional.

Cada programa es una secuencia de definiciones.

Las funciones son ciudadanos de primera clase.

El control de flujo se maneja con expresiones (if, let).

El análisis léxico garantiza una separación clara entre símbolos y valores.
