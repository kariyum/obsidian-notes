# Question 1
`head -50 vaccinationData.csv | python3 mapper.py`

![[Pasted image 20221004152452.png]]

### ii2/
**reducer.py**
```python
import sys
total = 0
old_key = None
for line in sys.stdin:
    data_mapped = line.strip().split("\t")
    if len(data_mapped) != 2:
        # not logic
        continue
    current_key, value = data_mapped
    if old_key and old_key != current_key:
        print(old_key, "\t", total)
        old_key = current_key
        total = 0
    old_key = current_key
    total += int(value)
if old_key != None:
    print(old_key, "\t", total)
```
![[Pasted image 20221004152633.png]]

## B/
**1/ Télécharger le fichier csv sur le système hadoop puis lancer le job mapreduce**
```bash
hdfs dfs -put tp3/vaccinationData.csv tp3
```

**2/ Lancer le job mapreduce sur vaccinationData.csv**
```bash
hs tp3/mapper.py tp3/reducer.py tp3/vaccinationData.csv vaccinationOutput1
```
![[Pasted image 20221004153224.png]]

```bash
hdfs dfs -cat vaccinationOutput1/part-00000
```
![[Pasted image 20221004153436.png]]

# Question 2
### Il suffit de déterminer la valeur maximale de la colonne nommée "people_fully_vaccinated_per_hundred"
```python
#!/usr/bin/env python3
import sys
max_value = 0
old_key = None
for line in sys.stdin:
    data_mapped = line.strip().split("\t")
    if len(data_mapped) != 2:
        # not logic
        continue
    current_key, value = data_mapped
    if old_key and old_key != current_key:
        print(old_key, "\t", max_value)
        old_key = current_key
        max_value = 0
    old_key = current_key
    if (max_value < float(value)):
        max_value = float(value)
if old_key != None:
    print(old_key, "\t", max_value)

```
![[Pasted image 20221004155906.png]]

**Resultat**
![[Pasted image 20221004160033.png]]

### Lancement de job
![[Pasted image 20221004160325.png]]

```bash
hdfs dfs -cat vaccinationDensity/part-00000
```
![[Pasted image 20221004160433.png]]

# Question 2-b
`mapper.py`
![[Pasted image 20221004161833.png]]

`reducer.py`
![[Pasted image 20221004161907.png]]

# Question 2-C
`mapper.py`
![[Pasted image 20221004163506.png]]

`reducer.py`
![[Pasted image 20221004163547.png]]

**Resultat**
![[Pasted image 20221004163625.png]]

**Lancement du job**
```bash
hs tp3/vaccinationData.csv tp3/mapper2c.py tp3/reducer2c.py vaccination2C
```
![[Pasted image 20221004163855.png]]