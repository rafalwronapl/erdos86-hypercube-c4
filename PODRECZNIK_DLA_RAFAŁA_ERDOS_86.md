# Podrecznik dla Rafala - Erdos #86

Temat: `ex(Q_n, C_4)` - maksymalna liczba krawedzi w podgrafie hiperkostki `Q_n` bez cyklu `C_4`.  
Cel praktyczny: zrozumiec, co zrobilismy, co wolno publicznie twierdzic i jak bezpiecznie wrzucic post na Erdos Problems.

## 1. Jednozdaniowo

Znalezlismy jawne, komputerowo sprawdzalne konstrukcje podgrafow hiperkostek `Q_9` do `Q_15`, ktore maja duzo krawedzi i **nie zawieraja zadnego 4-cyklu**.

To daje nowe **dolne ograniczenia** na `ex(Q_n, C_4)` dla `n = 9,...,15`.

Nie twierdzimy, ze sa optymalne. Nie twierdzimy, ze problem zostal rozwiazany.

## 2. Co to jest `Q_n`

`Q_n` to n-wymiarowa hiperkostka.

Mozna myslec tak:

- wierzcholki to wszystkie ciagi zer i jedynek dlugosci `n`;
- dwa wierzcholki sa polaczone krawedzia, jesli roznia sie dokladnie w jednej pozycji.

Przyklad:

```text
0000 i 0100 sa polaczone, bo roznia sie w jednej pozycji.
0000 i 0110 nie sa polaczone, bo roznia sie w dwoch pozycjach.
```

Liczby:

```text
|V(Q_n)| = 2^n
|E(Q_n)| = n * 2^(n-1)
```

## 3. Co to jest problem `ex(Q_n, C_4)`

Szukamy jak najwiekszego zbioru krawedzi hiperkostki, takiego ze w wybranym podgrafie nie ma zadnego kwadratu, czyli cyklu dlugosci 4.

Formalnie:

```text
ex(Q_n, C_4) = maksymalna liczba krawedzi w podgrafie Q_n bez C_4.
```

To jest Erdos Problems #86.

## 4. Co bylo znane przed nami

Wazne punkty:

1. Dla malych `n` sa znane dokladne wartosci do `n=6`.
2. Minamo Minamoto podal konstrukcje:

```text
ex(Q_7, C_4) >= 304
ex(Q_8, C_4) >= 680
```

3. Ogolna hipoteza asymptotyczna mowi mniej wiecej:

```text
ex(Q_n, C_4) = (1/2 + o(1)) * n * 2^(n-1)
```

Czyli dla bardzo duzych `n` maksimum powinno byc blisko polowy wszystkich krawedzi hiperkostki.

Wazne: wartosci dla malych `n` moga miec gestosc wieksza niz `1/2`. To nie obala hipotezy, bo hipoteza jest asymptotyczna.

## 5. Co my uzyskalismy

Uzyskalismy jawne certyfikaty dla:

| n | wynik | gestosc | liczba sprawdzonych 4-cykli |
|---:|---:|---:|---:|
| 9 | `ex(Q_9,C_4) >= 1505` | 0.6532 | 4608 |
| 10 | `ex(Q_10,C_4) >= 3304` | 0.6453 | 11520 |
| 11 | `ex(Q_11,C_4) >= 7156` | 0.6353 | 28160 |
| 12 | `ex(Q_12,C_4) >= 15372` | 0.6255 | 67584 |
| 13 | `ex(Q_13,C_4) >= 32856` | 0.6170 | 159744 |
| 14 | `ex(Q_14,C_4) >= 69909` | 0.6096 | 372736 |
| 15 | `ex(Q_15,C_4) >= 148126` | 0.6028 | 860160 |

To sa **dolne ograniczenia**, bo pokazujemy konkretne przyklady z taka liczba krawedzi.

## 6. Dlaczego to jest sprawdzalne

Kazdy wynik jest zapisany jako plik JSON z lista krawedzi.

Verifier robi dwie rzeczy:

1. Sprawdza, czy kazda krawedz naprawde jest krawedzia `Q_n`.
2. Enumeruje wszystkie 4-cykle w `Q_n` i sprawdza, czy ktorys jest w calosci wybrany.

Jesli verifier znajduje zero naruszen, to konstrukcja jest poprawna.

To jest najwazniejsza rzecz dla publikacji:

> Claim ma byc oceniany po certyfikatach i verifierze, nie po tym, kto albo jak znalazl konstrukcje.

## 7. Jak dzialal pomysl konstrukcyjny

Byly dwa kroki.

### Krok 1: product lift

Mamy dobra konstrukcje w `Q_n`. Chcemy zrobic konstrukcje w `Q_{n+1}`.

`Q_{n+1}` mozna widziec jako dwie kopie `Q_n`:

```text
slice 0
slice 1
```

Wkladamy:

- w pierwsza warstwe stara konstrukcje `G`;
- w druga warstwe przestawiona kopie `gG`, gdzie `g` jest automorfizmem hiperkostki;
- dodajemy niektore krawedzie miedzy warstwami.

Problem: nowe 4-cykle moga pojawic sie wtedy, gdy te same krawedzie sa obecne w obu slice'ach i wybierzemy zle cross edges.

Rozwiazanie: wybieramy cross edges na zbiorze niezaleznym w grafie przeciecia `G cap gG`.

Poniewaz ten graf jest dwudzielny, mozna znalezc maksimum dokladnie przez matching.

Wzor:

```text
nowa liczba krawedzi = 2m + alpha(G cap gG)
```

gdzie:

- `m` = liczba krawedzi starego grafu,
- `alpha` = rozmiar maksymalnego zbioru niezaleznego w grafie przeciecia.

### Krok 2: local ILP repair

Po product lift mielismy dobra konstrukcje, ale mozna bylo ja jeszcze poprawic.

Local repair:

1. Bierze brakujaca krawedz.
2. Sprawdza, jakie 4-cykle by zamknela.
3. Buduje lokalne sasiedztwo w grafie zaleznosci `edge <-> C4`.
4. Zamraza wszystko poza tym sasiedztwem.
5. Rozwiazuje maly ILP:
   - wymusza dodanie wybranej krawedzi;
   - pilnuje, zeby zaden 4-cykl nie mial wszystkich 4 krawedzi;
   - maksymalizuje liczbe krawedzi.

To czesto pozwalalo dodac kilka/kilkanascie krawedzi ponad lift.

## 8. Rzeczywisty lancuch wynikow

Startujemy od Minamoto `Q8 = 680`.

```text
Q8 = 680  (Minamoto)
  -> lift to Q9 = 1501
  -> repair to Q9 = 1505

Q9 = 1501
  -> lift to Q10 = 3292
  -> repair to Q10 = 3304

Q10 = 3304
  -> lift to Q11 = 7142
  -> repair to Q11 = 7156

Q11 = 7151
  -> lift to Q12 = 15350
  -> repair to Q12 = 15372

Q12 = 15366
  -> lift to Q13 = 32829
  -> repair to Q13 = 32856

Q13 = 32856
  -> lift to Q14 = 69883
  -> repair to Q14 = 69909

Q14 = 69909
  -> lift to Q15 = 148111
  -> repair to Q15 = 148126
```

Wazny niuans: najlepszy naprawiony graf nie zawsze byl najlepszym rodzicem do nastepnego liftu. Na przyklad `Q9=1505` byl lepszy jako finalny wynik Q9, ale gorszy jako rodzic dla Q10 niz wczesniejszy `Q9=1501`.

To jest obserwacja empiryczna, nie twierdzenie.

## 9. Co z "Japonczykiem" i czterema warstwami

To chodzi o Minamo Minamoto i Erdos #86.

Minamoto mial `Q7` i `Q8`. U nas pojawil sie eksperyment:

```text
Q8 x Q2
```

To znaczy: zamiast dwoch slice'ow, uzywamy czterech slice'ow `Q8`, odpowiadajacych wierzcholkom `Q2`:

```text
00, 01, 10, 11
```

Ten eksperyment byl ciekawy, bo wygladal jak naturalne "cztery warstwy".

Ale finalnie nie byl najlepszy:

```text
Q8 x Q2 four-slice construction for Q10 reached only Q10 >= 3248.
```

Nasza najlepsza konstrukcja dla Q10 to:

```text
Q10 >= 3304
```

Czyli cztery warstwy byly waznym eksperymentem, ale nie glownym finalnym wynikiem.

## 10. Czego nie twierdzimy

Nie wolno pisac:

- ze rozwiazalismy problem;
- ze znalezlismy dokladne wartosci `ex(Q_n,C4)`;
- ze `Q7=304` albo `Q8=680` sa udowodnione jako dokladne;
- ze obalamy hipoteze Erdosa;
- ze bijemy wszystkie mozliwe znane rekordy, jesli nie zrobiono pelnego literature search;
- ze local repair jest twierdzeniem matematycznym;
- ze czterowarstwowy wariant byl finalnym breakthrough.

Bezpieczny claim:

```text
I found explicit certified lower bounds for ex(Q_n,C_4), n = 9,...,15.
Each construction is given as an edge-list certificate and verified by exhaustive C4 enumeration.
No exactness claim is made.
```

## 11. Dlaczego nie arXiv

W Twojej sytuacji arXiv nie jest potrzebny.

Nie jestes specjalista od tej dziedziny, a jesli polityka arXiv moze grozic blokada po bledzie, lepiej nie ryzykowac.

Najbezpieczniejsza forma:

1. publiczne GitHub repo albo Gist;
2. certyfikaty + verifier + log;
3. post na Erdos Problems jako request for verification;
4. bez tonu formalnej publikacji.

## 12. Co wrzucic na GitHub / Gist

Minimalny publiczny pakiet:

```text
README.md
verify_c4_free.py
q9_edges_repair_from1503_iter2.json
q10_edges_repair_from3302_iter5.json
q11_edges_repair_from7151_iter3.json
q12_edges_repair_from15366_iter3.json
q13_edges_repair_from32842_iter2.json
q14_edges_repair_from69895_iter2.json
q15_edges_repair_from148111_iter1.json
lift_params.json
SHA256SUMS
VERIFY_ALL_CERTIFICATES.log
LICENSE
```

Nie wrzucaj calego roboczego folderu, bo jest tam duzo eksperymentow, logow i alternatywnych prob. Publiczny pakiet ma byc czysty i latwy do sprawdzenia.

## 13. Jak powinien wygladac README repo

README powinno miec:

1. tytul;
2. krotka tabele wynikow;
3. instrukcje:

```bash
python verify_c4_free.py
```

4. info, ze verifier enumeruje wszystkie `C4`;
5. SHA-256 hashes;
6. disclaimer:

```text
These are explicit lower bounds only. No exactness claim is made.
```

7. AI disclosure.

## 14. Co wkleic na Erdos Problems

Masz gotowy plik:

```text
C:\Users\rafal\erdos86_hypercube_c4\FORUM_POST_DRAFT_V3.md
```

Wklej tylko:

1. `Suggested Title`
2. `Post Body`

Nie wklejaj:

- checklisty;
- emaila do Minamoto;
- anticipated responses;
- internal strategy;
- "what not to post".

Przed wklejeniem podmien:

```text
[REPO_URL]
```

na prawdziwy link do GitHuba/Gista.

## 15. AI disclosure

Najlepszy disclosure:

```text
Disclosure: This was an AI-assisted computational discovery. My role was to run the search/verification pipeline, preserve the certificates, and post the result for independent checking. The claim should be judged by the explicit edge-list certificates and verifier, not by authorship.
```

Mozna dodac:

```text
I would be grateful for independent mathematical or computational verification.
```

Nie pisz dlugiego manifestu o AI. To ma byc uczciwa informacja, nie glowny temat.

## 16. Proponowany post - ton

Ton powinien byc:

- spokojny;
- weryfikacyjny;
- bez triumfalizmu;
- "oto certyfikaty, prosze o sprawdzenie";
- "nie roszcze sobie prawa do exactness".

Dobre frazy:

```text
explicit certified lower bounds
edge-list certificates
verified by exhaustive enumeration
no exactness claim
independent verification welcome
```

Zle frazy:

```text
breakthrough
solved
proof of optimality
counterexample to the conjecture
AI solved Erdos problem
```

## 17. Odpowiedzi na mozliwe komentarze

### "Do you claim exactness?"

Nie.

```text
No. These are lower bounds only. I do not claim exactness for n >= 7.
```

### "How do we know there are no C4s?"

```text
Each certificate is checked by exhaustive enumeration of all 4-cycles in Q_n. The verifier reports zero violating 4-cycles.
```

### "Why not Q16?"

```text
The next case is natural, but the local repair neighborhoods and search time grow quickly. I stopped at Q15 to post a checkable table.
```

### "Is this a contradiction to the asymptotic conjecture?"

Nie.

```text
No. The densities decrease from 0.6532 at n=9 to 0.6028 at n=15 and are consistent with the asymptotic conjecture.
```

### "Why mention Minamoto?"

Bo to bezposredni poprzednik.

```text
The construction starts from Minamoto's Q8=680 certificate and extends the explicit computational table to n=9,...,15.
```

### "Is the construction method a theorem?"

Product lift ma czyste uzasadnienie. Local repair jest procedura obliczeniowa.

```text
The final mathematical claim does not depend on trusting the heuristic search. It depends on the verified final certificates.
```

## 18. Najwazniejsza rzecz do zapamietania

Nie sprzedajesz "AI odkrylo twierdzenie".

Sprzedajesz:

```text
Oto siedem jawnych list krawedzi. Kazda zostala sprawdzona przez prosty verifier. Jesli verifier jest poprawny, to mamy dolne ograniczenia.
```

To jest bezpieczne, konkretne i matematycznie sprawdzalne.

## 19. Moja ocena

To jest obecnie najlepszy kandydat z Twoich watkow Erdos do publicznego pokazania.

Powody:

1. Claim jest waski.
2. Wynik jest certyfikowalny.
3. Nie wymaga pelnego dowodu optymalnosci.
4. Nie wymaga arXiv.
5. Mozna go niezaleznie sprawdzic.
6. Post jest juz prawie gotowy.

Najwieksze ryzyko to nie sama konstrukcja, tylko framing. Jesli napiszesz to jako "verified lower-bound certificates", jest dobrze. Jesli zabrzmi jak "AI solved Erdos #86", bedzie zle.

## 20. Checklista przed publikacja

1. Utworz czyste publiczne repo/Gist.
2. Wrzuc tylko minimalny zestaw plikow.
3. Uruchom verifier po skopiowaniu plikow.
4. Zapisz nowy `VERIFY_ALL_CERTIFICATES.log`.
5. Sprawdz SHA-256.
6. Podmien `[REPO_URL]` w `FORUM_POST_DRAFT_V3.md`.
7. Wklej tylko title + post body.
8. Dodaj AI disclosure.
9. Nie dodawaj claimow o exactness.
10. Monitoruj komentarze i poprawiaj repo, jesli ktos znajdzie blad w opisie albo verifierze.

