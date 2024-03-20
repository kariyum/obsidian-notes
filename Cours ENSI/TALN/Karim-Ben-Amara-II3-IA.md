## Etape 3:
1/
![[Pasted image 20230117143736.png]]

2/
```
./network.sh up
```
![[Pasted image 20230117152533.png]]

3/
```
Vérification que les noeuds sont fonctionnels
```
![[Pasted image 20230117152722.png]]

## Etape 4
```
Anchor peer set for org 'Org1MSP' on channel 'mychannel'
Anchor peer set for org 'Org2MSP' on channel 'mychannel'
```
![[Pasted image 20230117154704.png]]

## Etape 5

![[Pasted image 20230117160106.png]]
```
On vérifie que le chaincode s'est déployé sur les deux node peers.
```
## Etape 6
1/ Setting environment variables.
2/
![[Pasted image 20230117162930.png]]
```
Chaincode invoke successful.
```
3/
![[Pasted image 20230117163109.png]]

4/ Invoquer la fonction readasset
![[Pasted image 20230117163410.png]]

5/
![[Pasted image 20230117163532.png]]

6/
![[Pasted image 20230117163917.png]]
```
On vérifie que l'asset5 est maintenant la propriété de Brad.
```