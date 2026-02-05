
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

## Tłumaczenie $FO \to RA$

1. $R(x_1, \ldots, x_n) \to R$

2. $x=y \to \sigma_{i=j}(sth)$

3. $\phi \land \psi \to R_\phi \cap R_\psi = R_\phi - (R_\phi - R_\psi)$

4. $\phi \lor \psi \to R_\phi \cup R_\psi$

5. $\neg \phi = ActiveDomain - R_\phi$ (ActiveDomain to dziedzina możliwych rekordów)

6. $\exists x \phi \to \pi_y(R_\phi)$ (dla otrzymanych y będzie istniał taki x)


## Zapytania w RA

Tutaj rozwiąże kilka zadań z egzaminu, gdzie należy przedstawić zapytanie za pomocą operacji Algebry Relacji.
Będę korzystał z działań: $\times$, $\sigma$, $\pi$, $\cup$, $-$.

### Egzamin 2024/2025 Zad.3

#### Treść zadania

Mamy relację $E$ reprezentującą krawędzie grafu skierowanego, przy czym nie bierzemy pod uwagę wierzchołków izolowanych, nulli i duplikatów (tzn. zakładamy, że takich nie ma). Odległość $u$ do $v$ definiujemy jako najkrótszą ścieżkę z $u$ do $v$. Napiszemy wyrażenie algebry relacji wyliczający to $(u,v)$, że ich odległość wynosi $4$.

#### Rozwiązanie

Podejdźmy do tego rekurencyjnie. RA nie ma w samej sobie rekurencji, aczkolwiek tutaj jedynie mamy na myśli skończone złożenie jakiegoś działania algebry relacji konkretną ilość razy. Niech $C_n$ to będzie wynik dla naszego zapytania dla jakiegoś $n$. Oprócz tego zdefiniujmy $D_n = C_n \cup D_{n-1}$ dla $n \geq 1$, gdzie $D_0 = \emptyset$ - jest to zbiór wszystkich par $(u,v)$ które wystąpiły już wcześniej. Możemy więc zdefiniować:

$$
C_1 = E
$$

$$
C_{n + 1} = \pi_{1,4}(\sigma_{2,3}(E \times C_n)) - D_{n}  \text{ dla } n \geq 1
$$

Odpowiedzią jest zatem $C_4$ - to jest wyrażenie algebry relacji, rekurencja jest tylko wykorzystywana do obliczenia zapytania, lecz nie jest w samym zapytaniu.

### Egzamin 2022/2023 Zad.1

#### Treść zadania

Mamy relacje $Osoba(id)$ oraz $Zna(kto,kogo)$. $Zna$ jest symetryczna, nikt nie zna samego siebie i $Zna \subseteq Osoba \times Osoba$. Napiszemy wyrażenie algebry relacji które wyznacza wszystkie pary znajomych, które łącznie znają wszystkie osoby w bazie.

#### Rozwiązanie

Wszyscy znajomi osoby $O$: $Z(O) :=\pi_2(\sigma_{1 = O}(Zna))$

Wszyscy znajomi osób $O_1, O_2$: $Z(O_1, O_2) := Z(O_1) \cup Z(O_2)$

Zauważmy, że znajomi $O_1, O_2$ znają wszystkie osoby $\iff Z(O_1, O_2) = Osoba$. W szczególności $O_1, O_2$ są zawarte w $Z(O_1, O_2)$ jeśli tylko $O_1, O_2$ są znajomymi.

Teraz jednak, jeśli chcemy wykorzystać w pełni algebrę relacji, to należałoby jednak skorzystać z $\times$ by sprawdzić zapytaniem wszystkie możliwe pary na raz.

Dla przejżystości przyjmijmy oznaczenia $O1, O2 := Osoba$.

Weźmy: $A:=\pi_{1,2,4}(\sigma_{1 = 3} (O1 \times O2 \times Zna) \cup \sigma_{2 = 3} (O1 \times O2 \times Zna))$. Jest to relacja typu: (pierwszy znajomy, drugi znajomy, znajomy jednego z nich).

Teraz znajdziemy te pary, dla których istnieje osoba będące znajomym żadnego z nich: $B := \pi_{1,2}((Osoba \times Osoba \times Osoba) - A)$.

Zatem te pary, dla których nie istnieje osoba będąca znajmomym którejś z tych osób, to: $(Osoba \times Osoba) - B$, a podstawiając $A$ i $B$ dostajemy ostateczny wzór:

$$
(Osoba \times Osoba) - \pi_{1,2}((Osoba \times Osoba \times Osoba) - \pi_{1,2,4}(\sigma_{1 = 3} (Osoba \times Osoba \times Zna) \cup \sigma_{2 = 3} (Osoba \times Osoba \times Zna)))
$$

### Egzamin 2021/2022 Zad.1

#### Treść:

Mamy w bazie danych tabelę $Lubi(gracz, gra)$. Chcemy, korzystając tylko z algebry relacji, wyznaczyć wszystkich graczy takich, że mają takie same zbiory lubianych gier.

#### Rozwiązanie:

Przekształćmy:

$$
A := \set{(g_1,g_2) \mid \exists h (Lubi(g_1,h) \land \neg Lubi(g_2,h)) } =
$$

$$
\pi_{1,2}(\set{(g_1,g_2,h) \mid (Lubi(g_1,h) \land \neg Lubi(g_2,h)) }) =
$$

$$
\pi_{1,2}(\set{(g_1,g_2,h) \mid Lubi(g_1,h)} \cap \set{(g_1,g_2,h) \mid \neg Lubi(g_2,h)})
$$

Możemy uzyskać, że:

$$
B := \set{(g_1,g_2,h) \mid (Lubi(g_1,h)} = \pi_{1,3,2}({Lubi \times \pi_1{Lubi}})
$$

Oraz:

$$
C := \set{(g_1,g_2,h) \mid \neg Lubi(g_2,h)} = \pi_1(Lubi) \times (\pi_1(Lubi) \times \pi_2(Lubi) - Lubi) 
$$

Wtedy:

$$
A = \pi_{1,2}(B \cap C) = \pi_{1,2}(B - (B - C))
$$

Wystarczy teraz zadbać o symetrię rozwiązania (tj. pary są symetryczne i nasz warunek tego jeszcze nie uwzględnia). Wszystkie te pary które to nie spełniają to dokładnie $A \cup \pi_{2,1}(A)$. Zatem ostateczny wynik to:

$$
    \pi_1(Lubi) \times \pi_1(Lubi) - (A \cup \pi_{2,1}(A))
$$