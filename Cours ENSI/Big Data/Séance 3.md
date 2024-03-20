# Anagramme
**Mapper**
```
pour chaque ligne faire
	pour chaque mot faire
		générer_couple(Tri(mot), mot)
	finpour
finpour

Tri(mot) est une fonction qui trie les différentes lettres d'un mot donnée en paramètre et qui retourne le mot composé par les lettres triées.
```
---
**Reducer**
```
Reducer identity
Entree = (mot_trié, [mot1, mot2, ...])
générer_couple(key, concat(mot, mot1))
```

# Critique de MapReduce
Langage bas niveau
Etape **shuffle and sort** est très coûteuse
