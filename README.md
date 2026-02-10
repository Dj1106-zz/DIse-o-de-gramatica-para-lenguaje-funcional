1. Componentes Léxicos (Tokens)Antes de la estructura, definimos las unidades básicas que el analizador léxico reconocerá:CategoríaPatrón / EjemploPalabras Reservadaslet, in, if, then, else, match, with, funOperadores+, -, *, /, ==, `Identificadores[a-z][a-zA-Z0-9_]* (Variables y funciones)Constructores[A-Z][a-zA-Z0-9_]* (Tipos de datos y variantes)LiteralesEnteros (123), Flotantes (3.14), Strings ("hola")Delimitadores(, ), [, ], ,, `2. Gramática Sintáctica (EBNF)Utilizaremos la notación EBNF para definir la jerarquía de las expresiones. En un lenguaje funcional, prácticamente todo es una Expresion.Estructura GlobalEBNFPrograma    ::= (Definicion | Expresion)*
Definicion  ::= "let" identificador (identificador)* "=" Expresion
Expresiones y ControlEBNFExpresion   ::= LetExpr
              | IfExpr
              | MatchExpr
              | LambdaExpr
              | AppExpr

LetExpr     ::= "let" identificador "=" Expresion "in" Expresion
IfExpr      ::= "if" Expresion "then" Expresion "else" Expresion
LambdaExpr  ::= "fun" (identificador)+ "->" Expresion
Aplicación y OperacionesPara manejar la precedencia, dividimos las expresiones aritméticas:EBNFAppExpr     ::= AtomExpr (AtomExpr)* (* Aplicación de función por espacio *)
AtomExpr    ::= identificador 
              | literal 
              | "(" Expresion ")" 
              | Lista
3. Ejemplo de Código en LúminaPara verificar si la gramática es funcional (valga la redundancia), veamos cómo se vería un programa real:Haskell-- Definición de una función recursiva (Factorial)
let factorial n = 
    if n == 0 
    then 1 
    else n * factorial (n - 1)

-- Uso de lambdas y operadores pipe
let lista = [1, 2, 3]
in lista |> map (fun x -> x * 2)
Puntos clave del diseño:Currificación: La regla AppExpr ::= AtomExpr (AtomExpr)* permite que las funciones reciban argumentos uno a uno, facilitando la aplicación parcial.Recursión: Al permitir que una Definicion contenga una Expresion que a su vez referencia al identificador original.Ausencia de Bucles: No hay for o while; la iteración se maneja mediante recursión o funciones de orden superior (map, filter).
