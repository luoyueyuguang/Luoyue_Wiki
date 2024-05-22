*结构*
``` mermaid
flowchart LR
A[Frontend]
B[Optimizer]
C[Backend]
D(('source code'))-->A --> B --> C --> E(('Machine Code'))
```

``` mermaid
flowchart LR
	C[C Frotend]
	For[Fortran Frotend]
	Ada[Ada Frotend]
	Opt[Common Optimizer]
	X86[x86 Backend]
	PPC[PowerPC Backend]
	ARM[ARM Backend]
	C-->Opt
	For-->Opt
	Ada-->Opt
	Opt-->X86
	Opt-->PPC
	Opt-->ARM
```

 
``` mermaid
graph TB
a-->b
a-->c
b --> a
b --> c
c --> a
c --> b
d --> e
e --> d
```
