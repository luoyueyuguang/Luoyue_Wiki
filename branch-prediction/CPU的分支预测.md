## 相关概念
``` mermaid
graph TB
		st(("Strong taken\n11/T")) -->|N| wt(("Weakly taken\n10/T"))
		wt -->|T| st
		wnt(("Weakly not taken\n01/N")) -->|N| snt(("Strongly not taken\n00/N"))
		snt -->|T| wnt
		wt -->|N| wnt
		wnt -->|T| wt
		st-->|T|st
		snt-->|N|snt
```

aliasing:分支别名/混叠：
- 多组分支信息向量共享预测表中的同一项，导致这些分支预测相互干扰。