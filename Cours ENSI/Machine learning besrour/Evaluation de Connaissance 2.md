# Evaluation de Connaissance 2
**QCM**
1. **Afin de trouver le meilleur modèle, quelle procédure convient ?**
- On utilise la fonction svm de notre toolbox préféré (par exemple svc de scikit-learn) avec les paramètres par défaut.
- On fixe γ=1et on cherche la meilleure valeur du paramètre C du svm par validation croisée.
- On fixe C=1 et on cherche la meilleure valeur du paramètre γ par validation croisée.
- ==On utilise une procédure de recherche sur C et γ et on retient la paire (C,γ) donnant la meilleure performance en validation.== 
Une telle procédure peut être ==Grid search.==

2. Après avoir entrainé un modèle de régression Ridge, vous obetnez des précisions sur le training set et test set respectivement de 0.98 et de 0.54.
Que devriez-vous faire ?
- Le modèle souffre d'un overfitting, on doit diminuer la valeur de alpha.
- ==Le modèle souffre d'un overfitting, on doit augmenter la valeur de alpha.==
- Le modèle souffre d'un underfitting, on doit diminuer la valeur de alpha.
- Le modèle souffre d'un underfitting, on doit augmenter la valeur de alpha.

3. En utilisant les données pour les classes 0, 1 et 2 indiquées ci-dessous, quelle classe un KNeighborsClassifier classerait le nouveau (représenté par l'étoile rouge) pour k=1 et k=3
![[Pasted image 20221023170727.png|center]]
- k=1 : class 0
- ==k=3 : class 2==
- k=1 : class 2
- k=3 : class 1
- ==k=1 : class 1==
- k=3 : class 2
- k=1 : class 0
- k=3 : class 1

4. La figure suivante montre l'ajustement de trois modèles différents (en ligne bleue) sur les mêmes données d'apprentissage.
Que pouvez-vous conclure de ces visulisations ?
![[Pasted image 20221023171221.png]]
- L'erreur d'apprentissage du premier modèle est plus élevée que celle du deuxième et du troixième modèle.
- Le meilleur modèle pour ce problème de régression est le dernier (troixième) modèle, car il présente une erreur d'apprentissage minimale.
- ==Le deuxième modèle est plus robuste que le premier et le troixème car il sera plus performant sur des données non vues.==
- ==Le troixième modèle est surajusté par rapport au premier et au deuxième modèle.==
- Tous les modèles auront les mêmes performances car nous n'avons pas vu les meilleures données.
**Questions de réflexion**
1. **Supposons que vous utilisiez une régression polynomiale. Après avoir imprimé les courbes d'apprentissage, vous remarquez qu'il y a un écart important entre l'erreur d'entrainement et l'erreur de validation. Que se passe-t-il ? Quelles sont les trois manières de résoudre le problème ?**
==Un écart important entre l'erreur d'entrainement et l'erreur de validation indique le phénomène d'Overfitting. C'est à dire que le modèle n'a pas pu se généraliser. Pour résoudre le problème on peut aboutir à :==
	1. Augmenter la taille de donnée.
	2. Arrêter l'apprentissage dès qu'on détécte un écart croissant entre l'erreur d'apprentissage et l'erreur de validation.
	3. Equilibrer les données et les répartir aléatoirement en partie d'entrainement et de validation.
2. **Supposons que vous utilisiez une régression ridge. Vous remarquez que l'erreur d'entrainement et l'erreur de validation sont à peu près identiques et assez élevés : à votre avis, est-ce le biais ou la variance du modèle qui est trop élevés(e)? Devez-vous accroitre l'hyperparamètre de régularisation ∝ ou le réduire ?**
Puisque l'erreur de validation et l'erreur d'entrainement sont tous les deux assez élevés ceci montre que le **biais** est **trop élevé**. Pour corriger l'apprentissage, il faut réduire l'hyperparamètre ∝, le paramètre de pénalisation.

**Exercice KNN**
Distance entre (1, 4) et (6, 1) est 5.830951894845301
Distance entre (1, 4) et (6, 5) est 5.0990195135927845
Distance entre (1, 4) et (5, 6) est 4.47213595499958
Distance entre (1, 4) et (4, 2) est 3.605551275463989
Distance entre (1, 4) et (2, 1) est 3.1622776601683795
==Distance entre (1, 4) et (2, 6) est 2.23606797749979==
==Distance entre (1, 4) et (1, 2) est 2.0==
==Distance entre (1, 4) et (2, 5) est 1.4142135623730951==

==La classe est C2==
