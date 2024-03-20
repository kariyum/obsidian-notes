## Mini-exercice 1 - RSA

bob : p=61 et q=53
	n = pq = 3233 et φ(n) = (p-1) x (q-1) = 3120
	e = 17
	d = 1/e mod φ(n) = 2753 mod 3120 = 2753

enc(123) = (123^17) mod 3233
	= 855

dec(855) = (855^2753) mod 3233 = 123

**clé secréte de bob**
Bob choisi 
	**p=61** 
	**q=53**
	**n=pq=3233**
	**φ(n)=60x52=3120**
	**e=17**

E1