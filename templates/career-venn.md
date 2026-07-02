---
name: Career Venn Diagram
tags: [career, venn-diagram]
---

# Career Venn Diagram

Find your path, e.g., passion $\rightarrow$ market $\rightarrow$ ability.

```mermaid
venn-beta
  set A[Ability]
  set M[Market]
  set P[Passion]
    text P1[1]
  union M,P[incapable]
    text MP1[2]
  union A,P[No money]
  union A,M[Unhappy]
  union A,M,P[Ideal]
    text AMP1[3]
```