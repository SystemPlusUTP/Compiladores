# Ejercicio N° 1:

# Gramatica:

	```antlr
		E -> E+T | E-T | T
		T -> T*F | T/F | F
		F -> (E) | id  | num
	```
Primero eliminamos la recursividad por la izquierda la nueva gramatica sera:

	```antlr
		E 	-> 	TE'
		E' 	->	+TE'	|	-TE'	|	λ
		T	->	FT'
		T'	->	*FT'	|	/FT'	|	λ
		F	->	(E)		|	id		| num
	```

1. Muestre las acciones  del analizador sintactico LL para la cadena  de  entrada:

	* id-id*id+id+id

	Construya los conjunto primeros y siguientes

* Primeros:

		PRIMEROS(E) => {( , id , num}
		PRIMEROS(E') 	=> {+ , - , λ}
		PRIMEROS(T) => {( , id , num}
		PRIMEROS(T') 	=> {* , / , λ}
		PRIMEROS(F) => {( , id , num}

 * Siguientes:

		SIGUIENTES(E)	=> {$ , )}
		SIGUIENTES(E')	=> {$ , )}			
		SIGUIENTES(T)	=> {$ , ) , + , -}			
		SIGUIENTES(T')	=> {$ , ) , + , -}			
		SIGUIENTES(F)	=> {$ , ) , + , - , * , /}			


| Pasos     | Pila     | 		Cadena      | Acciones     |
| :-------- | :------- | :------------- | :----------- |
|			1			|        E$| id-id*id+id+id$|	E -> TE'		 |
|			2			|      TE'$| id-id*id+id+id$|	T -> FT'		 |
|			3			|		 FT'E'$| id-id*id+id+id$|	F -> id		   |
|			4			|		idT'E'$| id-id*id+id+id$|	Reducir			 |
|			5			|		  T'E'$|   -id*id+id+id$|	T'-> λ			 |
|			6			|				E'$|   -id*id+id+id$|	E'->-TE'		 |
|			7			|		  -TE'$|   -id*id+id+id$|	Reducir			 |
|			8			|		   TE'$|    id*id+id+id$|	T ->FT'			 |
|			9			|		 FT'E'$|    id*id+id+id$|	F ->id			 |
|			10		|		idT'E'$|    id*id+id+id$|	Reducir			 |
|			11		|		  T'E'$|      *id+id+id$|	T'->*FT'		 |

### Continua el ejercicio :)

2. Construya la tabla para el analizador sintactico. continue oe hagale pues la tabla

| NT\T |   +    |   -     |   /     |     *    |     id    |    num   |    (    |    )    |    $   |
| :--- | :----- | :------ | :------ | :------- | :-------- | :------- | :------ | :------ | :----- |
| E    | error  |  error  |  error  |  error   | E -> TE'  | E -> TE' | E -> TE'|  error  |  error |
| E'   |E'->+TE'| E'->-TE'|  error  |  error   |   error   |   error  |  error  |  E'->λ  |  E'->λ |
| T    | error  |  error  |  error  |  error   | T -> FT'  | T -> FT' | T -> FT'|  error  |  error |
| T'   | T'->λ  |  T'->λ  | T'->/FT'| T'->*FT' |   error   |   error  |  error  |  T'->λ  |  T'->λ |
| F    | error  |  error  |  error  |  error   | F -> id   | F -> num | F -> (E)|  error  |  error |

3. La gramatica resultante es LL(1) justifique su respuesta

La gramatica resultante es LL(1) ya que solo necesita un token para determinar la regla que debe
aplicar para dicho token es decir no tiene mas de una regla de un simbolo terminal por token evitando
el proceso de backtraking.

# Ejercicio N°2:

# Gramatica:

	```antlr
		S -> (S)S | λ
	```
1. Muestre las acciones  del analizador sintactico LL para la cadena  de  entrada:

	* ((())((())()))
	Construya los conjunto primeros y siguientes
	* Primeros:

			PRIMEROS(S)	=> {( , λ}

	* Siguientes:

			SIGUIENTES(S)	=> {$ , )}

| Pasos     | Pila     | 		Cadena      | Acciones     |
| :-------- | :------- | :------------- | :----------- |
|			1			|        S$| ((())((())()))$|	S -> (S)S		 |
|			2			|     (S)S$| ((())((())()))$|	Reducir			 |
|			3			|		 	 S)S$|  (())((())()))$|	S -> (S)S		 |
|			4			|		(S)S)S$|  (())((())()))$|	Reducir			 |
|			5			|		 S)S)S$|   ())((())()))$|	S -> (S)S		 |
|			6			|	(S)S)S)S$|   ())((())()))$|	Reducir 		 |
|			7			|	 S)S)S)S$|    ))((())()))$|	S -> λ 			 |
|			8			|		)S)S)S$|    ))((())()))$|	Reducir			 |
|			9			|		 S)S)S$|     )((())()))$|	S -> λ			 |
|			10		|		  )S)S$|     )((())()))$|	Reducir			 |
|			11		|		   S)S$|      ((())()))$|	S -> (S)S		 |

### Continua el ejercicio :)

2. Construya la tabla para el analizador sintactico. continue oe hagale pues la tabla

| NT\T |   (    |   )   |   $   |
| :--- | :----- | :---- | :---- |
|   S  |  (S)S  |   λ   |   λ   |
