# Mesure de performance
### Mesures pour la classification : Matrice de confusion
Confusion matrix for classification
![[Pasted image 20220927085153.png]]
**True positive :** The model correctly predicts the positive class

## Rappel et Précision
Le rappel et sensibilité <span color="blue">recall</span> correspond au pourcentage de la classe cible qui a été bien classé. 
rappel = instances bien détéctées de la classe / nombre total d'instances de la classe

Précision ou prédictive positive correspond à la fraction d'instances bien classées parmi toutes les instances analysées.
>Quelle est la fiabilité d'un classement proposé ?

précision = instances bien détéctés de la classe / nombre d'instances prédit de la classe

## Mesures pour la régression : Erreur et R²
Ecart absolu moyen (MAE : Mean Absolute Error) 
MAE = 1/n ∑ | yi - yi* |
yi : valeur réelle
yi* : valeur prédit
...

## Mesures pour l'apprentissage non-supervisé
Mesure de l'homogénité des partitions proposées
- Distance intra-classe mesure la diespersion des éléments d'une m$eme classe en calculant la moyenne de la distance entre les éléments entre les éléments de la classe et le centre de la classe
L'algorithme de clustering devra fraie en sorte que cette distance soit minimale

- Distance inter-classe mesure la distance entre les centres des différentes classes
...
## Overfitting et underfitting...
