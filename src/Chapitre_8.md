# Chapitre 8 - Gestion de la mémoire

> [!link] Index
> [[Index|Retour à l'index]]

> [!pdf] PDF
> [[cours_sem_12_gestion-memoire_memo.pdf|Début du cours du chapitre 8]]

```pdf
{
	"url" : "src/cours_sem_12_gestion-memoire_memo.pdf",
	"scale" : 2.0,
	"page" : 0
}
```

## III - Pile d'exécution

### 5 - Risque de l'utilisation d'une fonction récursive

> [!warning] Attention
L'exécution d'une fonction récursive peut entraîner un **débordement de la pile d'exécution** (*stack overflow*).

### 6 - Récursivité terminale

> [!example] Exemple 
Exemple de fonction somme récursive :
>```ocaml
>let rec somme n = 
>	if n = 0 then 0
>	else n * somme (n-1)
>```
>
>Même fonction mais en **"récursif terminal"** :
>```ocaml
>let somme_rt n =
>	let rec aux n s =
>		if n = 0 then s
>		else aux (n-1) (s+n)
>	in aux n 0
>```

> [!note] Définition
> Une fonction est **récursive terminale** si le résultat renvoyé par un (potentiel) appel récusrif n'est pas utilisé dans une opération.

> [!tip] Propriété du compilateur Ocaml
Si une fonction est récursive terminale, le compilateur Ocaml la traduit de manière itérative.
