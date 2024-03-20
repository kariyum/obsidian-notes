 ```bash
cd /usr/local/pighome/pig-0.17.0

wget https://dlcdn.apache.org/pig/pig-0.17.0/pig-0.17.0.tar.gz --no-check-certificate

tar xzf pig-0.17.0.tar.gz

echo "
export PIG_HOME=/usr/local/pighome/pig-0.17.0
export PATH=$PATH:$PIG_HOME/bin
export PIG_CLASSPATH=$HADOOP_HOME/conf
" >> ~/.bashrc

source ~/.bashrc
```

# Partie 1 - PIG
```bash
1/
docker cp achats.csv hadoop-master:/root/tp5
docker cp clients.csv hadoop-master:/root/tp5

hdfs dfs -put tp5 .

2/
pig

3/
achats = LOAD 'tp5/achats.csv' using PigStorage(';') AS (idachat, idclient, montant);
```

REMARQUE :
Il faut démarrer le serveur jobhistory exécuter sur la cmd du conatiner.
`mr-jobhistory-daemon.sh start historyserver`

```bash
4/
DUMP achats;
```
![[Pasted image 20221019185305.png]]

```bash
5/
achatsparclient = GROUP achats BY idclient;

6/
```
![[Pasted image 20221019185655.png]]

```bash
7/
totaux = FOREACH achatsparclient GENERATE group,COUNT(achats.idachat) AS total;

group represente le idclient et total represente le nombre d entrée 
8/
```
![[Pasted image 20221019185935.png]]
```bash
9/
EXPLAIN totaux;
```
![[Pasted image 20221019190744.png]]
```bash
10/
clients = LOAD 'tp5/clients.csv' using PigStorage(';') AS (idclient, nom);

```
![[Pasted image 20221019191012.png]]


#### 11/ Afficher les noms des clients avec le nombre d’achats qu’ils ont fait. 
```bash
jointure = JOIN clients BY idclient, totaux BY group;
dump jointure;
```
![[Pasted image 20221019192215.png]]
```bash
resultat = FOREACH jointure GENERATE nom,total;
dump resultat;
```
![[Pasted image 20221019192351.png]]

#### 12/ Afficher les noms et le nombre d'achats des 3 clients qui ont fait plus d'achats dans l'ordre décroissant.
```bash
miresultat = ORDER resultat BY total DESC;
finresultat = LIMIT miresultat 3;
```
![[Pasted image 20221019193440.png]]

# Partie 2 HIVE
```bash
cd /usr/local/hivehome

wget https://dlcdn.apache.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz --no-check-certificate

tar xzf apache-hive-3.1.2-bin.tar.gz

echo "
export HIVE_HOME=/usr/local/hivehome/apache-hive-3.1.2-bin
export PATH=$PATH:$HIVE_HOME/bin" >> ~/.bashrc
```
[Hive - HiveException java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient - Spark by {Examples} (sparkbyexamples.com)](https://sparkbyexamples.com/apache-hive/hive-hiveexception-runtimeexception-unable-to-instantiate-sessionhivemetastoreclient/)

#### 2/ creation.hql
```
Le séparateur doit être = ';'
```
![[Pasted image 20221020120049.png]]

#### 3/ show tables
```cql
use ag_t;
show tables;
```
![[Pasted image 20221020115744.png]]

#### 5/
```cql
reservation.hql

use ag_t;

LOAD DATA LOCAL INPATH 'clientH.csv' into table client;
LOAD DATA LOCAL INPATH 'reservation.csv' into table reservation;
LOAD DATA LOCAL INPATH 'hotel.csv' into table hotel;
```
`hive -f reservation.hql`
![[Pasted image 20221020121325.png]]

#### 6/
```cql
a/ cin et nom_prenom des personnes qui ont réservé plus de 4 nuitées.

select client.cin, nom_prenom from client JOIN reservation where nb_nuitee > 4;
```
![[Pasted image 20221020122855.png]]

```cql
b/ id_h et nom_h des hotels dans lesquels il y a plus q une reservation

SELECT hotel.id_h, hotel.nom_h FROM hotel JOIN reservation ON (hotel.id_h = reservation.id_h) GROUP BY hotel.id_h, hotel.nom_h HAVING COUNT(*) > 1;
```
![[Pasted image 20221020123459.png]]

#### 7/
```cql
create table bucket_hotel(id_h int, nom_h string, classe int, nb_ch int) partitioned by (adresse string) clustered by (classe) into 2 buckets;
```
#### 8/
```cql
insert into table bucket_hotel partition (adresse) select * from hotel;
```
#### 9/

![[Pasted image 20221020124549.png]]

```cql
select distinct adresse from bucket_hotel;
```
![[Pasted image 20221020124825.png]]

On remarque que la table bucket_hotel associe un répertoire pour chaque valeur de la colonne adresse.

#### 10/
![[Pasted image 20221020125019.png]]

#### 11/
```cql
alter table reservation replace columns (cin string, id_h int, date_debut string, nb_nuitee int);

desc reservation;
```
![[Pasted image 20221020125302.png]]

#### 12/
```cql
drop table hotel;
```
![[Pasted image 20221020125342.png]]
