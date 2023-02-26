# Calendar dimension
![[Calendar Dimension.png]]
# Location dimension
![[Location dimension.png|700]]
# Time dimension
![[Time dimension.png|700]]
# Product dimension
![[Product dimension.png|800]]

![[different case cases.png]]
# Acyclic test
![[acyclic case cases.png]]
# How are we going to check this ?
[]()
### Test case 1
![[case 1.png]]
Map :
One -> Two
Two -> One

Returns **false** 

### Test case 2
![[case 2.png]]
Map : 
One -> Two
Two -> Three
Three -> One

Returns **false**

## Test case 3
![[case 3.png]]

Map :
One -> Two
Two -> Three
Three -> Four
Four -> One

Returns **false**

==Other variations are just a combination of the above==

## The algorithm
Let's start by an example
![[Location dimension.png|700]]
The algorithm would start by removing the **nodes** that have points to **null**. In this case, in the first iteration it will remove the ==Line Of Business== node, since it points to nothing.
Going further, Region will be points to **null**, and so it will be removed in the next iteration.
Next up is ==Disctrict== and later ==Store==. Now that the dimension is empty, the algroithm returns **true**.

## Case 2
![[case 2.png]]
In this case, there aren't any node that points to **nothing** and so the algorithm will return **false** on the first iteration.
