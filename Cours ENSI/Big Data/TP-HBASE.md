# TP-HBASE

```bash
sh start-hadoop.sh
sh start-hbase.sh
hbase shell
```
![[Pasted image 20221106111322.png]]

![[Pasted image 20221106111719.png]]

#### 1/ Creation d'une bibliothèque Bib
![[Pasted image 20221106112424.png]]

#### 2/ Vérification de la création et affichage de la description de la table bib.
![[Pasted image 20221106112256.png]]
![[Pasted image 20221106112354.png]]

#### 3/ Ajout de n-uplets
```cql
put 'bib', 'vhugo', 'auteur:NOM', 'Hugo'
put 'bib', 'vhugo', 'auteur:PRENOM', 'Victor'
```
![[Pasted image 20221106113526.png]]
![[Pasted image 20221106113539.png]]

#### 4/
![[Pasted image 20221106113812.png]]

#### 5/ Count 'bib'
![[Pasted image 20221106113924.png]]

#### 6/ Affichage

![[Pasted image 20221106121302.png]]

#### 7/
![[Pasted image 20221106121400.png]]

#### 8/
![[Pasted image 20221106121649.png]]

#### 9/
![[Pasted image 20221106121753.png]]

#### 10-11-12/
![[Pasted image 20221106121838.png]]
![[Pasted image 20221106121909.png]]
```text
scan 'bib'
-> affiche la table 'bib' sans filtrage / séléction

scan 'bib', {COLUMNS=>['livre']} 
-> affiche les entrées (ROWS) de la table 'bib' qui ont la famille livre

scan 'bib', {COLUMNs=>['livre:DATE']}
-> affiche les entrées de la table 'bib' qui ont la famille livre et la colonne DATE.
```

#### 14/ 
![[Pasted image 20221106122838.png]]

#### 15/
![[Pasted image 20221106123434.png]]

On remarque que les nouvelles valeur de ==auteur:NOM== n'ont pas remplacé les valeurs précédentes. Autrement dit, les anciennent valeurs persistent encore dans la table.

#### 16/
![[Pasted image 20221106123610.png]]
Les nouvelles valeurs ainsi que l'ancienne valeur de livre:DATE persiste encore dans la table. 

#### 17/ Suppressions des valeurs.
```
17/
deleteall 'bib', 'vhugo', 'auteur:NOM', 1667734454817

18/
deleteall 'bib', 'vhugo', 'auteur:PRENOM'

19/
deleteall 'bib', 'jvernes'
```

Verification des résultats
![[Pasted image 20221106124041.png]]
