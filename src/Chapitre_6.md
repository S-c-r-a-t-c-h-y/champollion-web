# Chapitre 6 - Types structurés

> [!info] Index
> [[Index|Retour à l'index]]

## III - Retour sur l'allocation des structures en C

```c
#include <string.h>

// défini la structure personne_s
struct personnes_s { 
	char nom[256],
	int age,
}; 

typedef struct personne_s personne; // personne est l'alias du type (on le renomme)
```

```c
// Allocation d'une structure :
personne *p = (personne *) malloc(sizeof(personne));
```

```c
// Accès à un élément :
(*p).age = 50;
p -> age = 50;
strncpy(p->nom, "Bidule", 256) // on doit utiliser strncpy pour affecter des chaines de caractères
```

```c
// Allocation dynamique d'un tableau de structure :
personne *tab = (personne *) malloc(sizeof(personne) * n);

free(tab); // ne pas oublier de libérer l'espace mémoire
```

```c
void initialise(personne *tab, int n)
{
	for (int i = 0; i < n; i++)
	{
		tab[i].age = 20;
		sprintf(tab[i].nom, "Personne%d", i);
	}
}
```