# Evaluation de Connaissance 1

1. **Quels éléments essentiels caractérisent un problème de machine learning?**
- Une tache précise à accomplir
- ==Des données relatives au phénomène évoqué==
- ==Une mesure de la qualité des donnéees==
- Une mesure de performance du modèle

2. **Complétez la phrase suivante :**
"Le machine learning est un ensemble de méthodes qui permettent aux ordinateurs d'apprendre à traiter des tâches de manière automatique ==supervisée par des humains==."

3. **Un problème d'apprentissage supervisé, signifie que...**
- ==Les données d'entrainement sont totalement annotées de la sortie désirée==
- Les données d'entrainement doivent être au moins en partie annotées
- Les données d'entrainement ne sont pas annotées du tout

4. **Vous devez implémenter un algorithme de détection de transactions de frauduleuses.**
Supposons que la classe **Vrai** est la classe **fraude**.
- Vrai négatif 
- Faux négatif
- ==Faux positif== −≻ Le modèle n'a pas bien classifié les fraudes. 
- Vrai positif

5. **Une entreprise veut prédire le prix d'un plein de carburtant, connaissant les caratéristiques d'une voiture. Est-ce une problème de régression ou de classification ?**
- ==C'est un problème de régression== −≻ Prédire une valeur continu.
- C'est un problème de classification

6. **Un modèle prédictif qui se généralise bien signifie que**
- Le modèle est sur dimensionné (overfitting)
- Le modèle s'adapte bien au bruit des données.
- ==Le modèle fonctionne bien sur les données test==
- Le modèle fonctionne bien sur les données utilisées pour ajuster ses paramètres

7. La ligne de séparation n'est pas idéale pour classer les '+' des 'o'. Pourquoi ?
![[Pasted image 20221023161547.png]]
- Le modèle semble légérement en underfitting
- Je ne vois pas de problème avec cette ligne de séparation
- ==Le modèle semble légèrement en overfitting==

**Questions de réflexion**
**Q1:** comment définiriez-vous le Machine Learning ?
Pouvez-vous nommer 4 types de problèmes où el se distingue ?

Le machine learning est une approche opté par les scientifiques pour donner la capacité aux ordinateurs à apprendre à partir des données afin de résoudre des problèmes assez complexes.
- Prédiction de la consommation d'essence pour une voiture précise.
- Prédiction de prix des actions de bourse.
- Recommandation des marchandises aux clients.
- Classification entre plusieurs catégories de fruits.

**Q2:** Quel type d'algorithme utilisiez-vous pour segmenter vos clients en plusieurs groupes?
- Algorithme de classification notamment : **Logistic regression** (2 groupes), **KNN** (en plusieurs groupes), **SVM**

**Q3:** Quelle est la différence entre un paramètre de modèle et un hyperparamètre d'un algorithme d'apprentissage?
- Un paramètre de modèle affecte la prédiction, c'est à dire, ce paramètre est utilisé pour calculer la prédiction. Par contre, un hyperparamètre est 

|Paramètre de modèle | Hyperparamètre|
|--------------------|----------------|
|Ajusté par le modèle |Généralement fixe pour la durée d'entrainement|
|Utilisé pour calculer la prédiction |N'est pas utilisé pour la prédiction|
| | Ajusté par l'utilisateur pour que le modèle puisse converger vers le minimum global et donc vers une meilleur prédiction.|
