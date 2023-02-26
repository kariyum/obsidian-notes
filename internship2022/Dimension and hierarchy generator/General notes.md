### Rules
The dimension must be a connected ==directed== ==acyclic== graph.

**How do we check for a directed graph ?**
Will have to see if a child and a parent could travel to each other.
>[!info]
>This could be integrated within the acyclic check function.

**How do we check if a graph is acyclic ?**
Check all combinations and mark every entry as visited and if however we visit a visited node we raise an execption 

