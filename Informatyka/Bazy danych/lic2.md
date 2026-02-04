
# Języki zapytań dla relacyjnych baz danych.

## Język deklaratywny: logika pierwszego rzędu ($FO$).

Logiką pierwszego rzędu ($FO$) chcemy zmodelować rozumowanie. Składa się z następujących elementów:
1. Formuły atomowe:
  * Relacje $R(x_1, x_2,\ldots, x_n)$, np. relacja mniejszości $<(x_1,x_2)$.
  * Równość $x=y$ - atom wyrażający identyczność dwóch obiektów na dziedzinie.
2. Spójniki: $\phi \land \psi, \phi \lor \psi,\neg \phi,\phi \rightarrow \psi,\phi \leftrightarrow \psi $
3. Kwantyfikatory: $\exists x. \phi, \forall x. \phi$

## Język imperatywny: algebra relacji ($RA$).

Algebrą relacji ($RA$) chcemy zmodelować jak faktycznie realizujemy zapytania. Składa się z następujących elementów:

### Wyrażenia atomowe:

$\set{(a_1, a_2,\ldots, a_n)}$ - gdzie $a_i$ stałe oraz $R$ - to "tabele", czyli zbiory krotek

### Selekcja:

$\sigma _{i=j}(S) = \set{(a_1, a_2, \ldots, a_n) | (a_1, a_2, \ldots, a_n) \in S, a_i=a_j }$

$\sigma _{i,b}(S) = \set{(a_1, a_2, \ldots, a_n) | (a_1, a_2, \ldots, a_n) \in S, a_i=b }$
   
### Rzut, permutacja, duplikacja:

$\pi_{i_1,i_2, \ldots, i_k}(S) = \set{(a_{i_1}, a_{i_2}, \ldots, a_{i_k}) | (a_1, a_2, \ldots, a_n) \in S}$

### Produkt:

$S \times T = \set{(a_1,\ldots,a_n,b_1,\ldots,b_m)| (a_1,\ldots,a_n) \in S, (b_1,\ldots,b_m)\in T}$

### Suma:

$S \cup T = \set{(a_1,\ldots,a_n)| (a_1,\ldots,a_n) \in S, (a_1,\ldots,a_n) \in T}$

### Różnica:

$S - T = \set{(a_1,\ldots,a_n)| (a_1,\ldots,a_n) \in S, (a_1,\ldots,a_n) \notin T}$

## Równoważność języków pomaga pisać zapytania w SQL-u.

**Twierdzenie Codd'a:** $\textbf{FO=RA}$ - logika pierwszego rzędu i algebra relacji są tak samo ekspresywne. W praktyce oznacza to, że za pomocą $FO$ i $RA$ da się wyrazić te same zapytania. Daje nam to sposób, aby móc rozumować $(FO)$ o danych na fizycznym komputerze $(RA)$.
