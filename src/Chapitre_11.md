# Chapitre 11 - Traits impératifs en OCaml

> [!link] Index
> [[Index|Retour à l'index]]

## I - 
### A - type unit

Le type `unit` en OCaml est un type produit donc le constructeur est vide.
```ocaml
();;
- : unit = ()
let res = ();;
val res : unit = ()
```

### B - affichage

```ocaml
print_int;;
(* int -> unit *)

(* il existe aussi les fonction print_float, print_char, print_endline etc *)
```

Une autre manière de faire :
```ocaml
Printf.printf "ceci est un entier %d" 10;;
```

### C - l'opérateur ;

L'opérateur `;` permet de séparer plusieurs expressions de type `unit`.
```ocaml
let x = e1 in e2; e3
(* est équivalent à *)
let x = e1 in (e2; e3)
```

Exemple:
```ocaml
print_int 10; print_newline ();;
```

### D - blocs

```ocaml
if e then begin
e1; e2;
e3;
end
else f1
```

Sans le `begin ... end`, l'expression est équivalente à
```ocaml
(if e then e1); e2; e3; else f1
```

## II - Boucles

### A - Boucle for

**Syntaxe :**
```ocaml
for i = e1 to e2 do
	e
done
```
`i`, `e1` et `e2` sont de type `int`
La borne `e2` du `for` est inclusive.

On peut boucler à l'envers :
```ocaml
for i = e1 to e2 downto
	e 
done
```

### B - Boucle while

**Syntaxe :**
```ocaml
while c do
	e
done
```
`c` est de type `bool` et `e` de type `unit`

## VI - Structures mutables

### A - Mutabilité

Une structure de donnée est **mutable** si certains de ses "champs" peuvent être modifiés.