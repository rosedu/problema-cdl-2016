# Eminescu și geometria

Mihai Eminescu, _poetul nepereche_, a decis că pentru a-și lărgi perspectivele, vrea să studieze la Facultatea de Informatică și Calculatoare. În anul 1, s-a îndrăgostit de geometrie, pe care a crezut-o ca fiind asemănătoare cu poezia. Până în anul 3 el a fost nemulțumit de expresivitatea limbajelor de programare din ziua de astăzi. Chiar și Haskell i se părea lui că e prea uman. Așa că s-a gândit că ar fi extraordinar dacă ar putea scrie poezii într-un limbaj de programare, care mai apoi să facă un desen geometric. Luceafărul ar putea deveni multidimensional în acest fel.

Bineînțeles că tu cititorule, coleg de bancă al lui, a trebuit să îl temperezi și să îi explici că așa ceva nu prea este posibil. Dar fiindcă Mihai devenise foarte trist, ai căzut de acord să implementezi ceva asemănător pentru a-l ajuta.

# Limbajul

Limbajul suporta mai multe comenzi:
- creare de obiecte 2D
- translație și rotire a obiectelor
- bloc de decizie (if), unde singura condiție este intersecția a două obiecte
- ștergerea de obiecte

Comenzile sunt *ascunse* în interiorul poeziei, fiind necesar ca poezia să nu-și piarda valoarea artistică.
Un vers este asociat unei comenzi dacă conține un cuvânt cheie, în funcție de comandă.

## Crearea de obiecte 2D

Există două moduri prin care se pot crea noi obiecte:

- Obiecte prestabilite, unde se dă un tip de obiect (pătrat sau dreptunghi) și poziția acestuia
- Obiecte poligon, unde se da o lista de puncte

Important este că fiecărui obiect creat i se asociază un ID unic; primul obiect creat va avea ID-ul egal cu 0, următorul cu 1, etc.
Un aspect care intervine este ștergerea de obiecte. Să presupunem următorul scenariu: s-au creat 4 obiecte (cu ID-urile 0, 1, 2 si 3), după care apare o comanda de ștergere a obiectului cu ID-ul 1; după ștergere obiectele rămase vor fi 0, 2 și 3. Dacă în acest moment se dorește crearea unui nou obiect, acesta va avea ID-ul 4. Cu alte cuvinte se va incrementa ID-ul ultimului obiect creat, fără să se țină cont de ștergeri. 

Cuvântul cheie pentru comanda de creare este `vreau`.
Versul de mai jos crează un pătrat de latură 2. Punctul (0, 0) din comandă corespunde colțului din stanga-jos a patratului,
deci celelalte puncte vor avea coordonatele (0, 2), (2, 2) şi (2, 0).

```
  Vreau un pătrat în punctul (0,0) de latură 2.
```

## Translația obiectelor
Translația se referă la mutarea obiectelor pe verticală sau pe orizontală.
Cuvântul cheie este `muta`, împreună cu `stanga` și `dreapta`.

Versul de mai jos mută obiectul cu ID-ul 0 la stânga cu două unitați. Dacă considerăm pătratul de mai sus, coordonatele vârfurilor vor fi (-2, 0), (-2, 2), (0, 2) şi (0, 0).

```
Mută în stânga figura 0 cu 2.
```

## Rotirea obiectelor
Rotirea e operația prin care un obiect se rotește în jurul unui punct dat.
Cuvântul cheie este `roteste`.

Versul de mai jos rotește obiectul cu ID-ul 0 (din nou, considerăm pătratul din exemplele de mai sus) cu 90 de grade în sens trigonometric în jurul punctului (0, 0). 
Coordonatele pătratului după rotire vor fi  (0, -2),  (-2, -2),  (-2, 0) și (0, 0). (rotirea se face după operația de translație de mai sus)

```
Rotește figura 0 cu 90 de grade la punctul (0,0).
```

## Structura de decizie

Structura de decizie (if) verifică dacă două obiecte 2D se intersectează sau nu; în funcție de rezultat de decide care comandă se execută în continuare. 

Cuvintele cheie sunt `daca`, `intersecteaza`, `altfel` și `gata`.
Linia care contine comanda `gata` nu are un format fix; linia va incepe mereu cu `gata`, dar dupa se poate continua un vers normal (vezi exemplele de mai jos).

În exemplul de mai jos, dacă figura 0 intersectează figura 1, se va crea un poligon, altfel se va crea un pătrat.

```
Dacă figura 0 intersectează figura 1,
Vreau un poligon cu punctele (1,1), (3,3), (4,4).
Altfel,
Vreau un pătrat în punctul (1,1) de latură 2.
Gata---
```

Intre liniile cu `daca` si `altfel` (si intre `altfel` si `gata`) pot fi mai multe comenzi (sau niciuna).
Este valid să se trimită același ID (și e evident că cele două obiecte se vor intersecta).

## Ștergerea de obiecte

Ștergerea "distruge" obiectul cu ID-ul respectiv.

```
Șterge figura 0.
```

## Exemplu de poezie

```
Somnoroase păsărele digitale... 

Somnoroase pasarele
Vreau un patrat in punctul (0,0) de latura 2.
Pe la cuiburi se aduna,
Vreau un dreptunghi in punctul (1,1) de laturi (3,2).
Se ascund în ramurele -
Muta in stanga figura 0 cu 2.
Noapte buna!
Roteste figura 1 cu 90 de grade la punctul (1,1).

Doar izvoarele suspina,
Dacă figura 0 intersecteaza figura 1,
Vreau un poligon cu punctele (1,1), (3,3), (4,4).
Altfel,
Vreau un patrat in punctul (1,1) de latura 2.
Pe cand codrul negru tace;
Gata blocul de daca.
Dorm si florile-n gradina -
Dormi în pace!
```

După ce parsați întreaga poezie, va trebui la final să afișați pentru toate figurile din ea, toate punctele în care se află. Punctele vor fi sortate după X și apoi după Y.

```
0 Patrat:
0 0
0 2
2 0
2 2
1 Dreptunghi:
1 1
1 3
4 1
4 3
2 Poligon:
1 1
3 3
4 4

```

### Limite

* fișierul de intrare va avea maxim 10.000 de linii
* vor fi maxim 1.000 de figuri geometrice create
* fiecare figură geometrică va avea maxim 42 de puncte
* timpul de rulare va fi de 1 secundă per test, pe o mașină Linux Ubuntu de 512 MB RAM
* se garantează că toate punctele poligoanelor vor fi în intervalul [INT_MIN, INT_MAX] (valorile de [aici](http://www.cplusplus.com/reference/climits/)), indiferent de ce mișcări vor fi făcute pe planul geometric

### Specificațiile limbajului

- punctele pătratelor și dreptunghiurilor din poezie reprezintă punctul cel mai din stânga jos
- rotațiile se fac în sens trigonometric
- orice linie care nu conține cuvintele `muta`, `roteste`, `vreau`, `altfel`, `daca`, `gata` nu afecteaza planul geometric
- liniile de comandă (cele cu cuvintele cheie de mai sus) își vor păstra aceaiași formă, doar numerele vor fi schimbate
- punctele din plan vor încăpea într-un _int_
- rotațiile vor fi doar multiplu de 90
- toate poligoanele sunt convexe
- în fișierul de intrare nu vor fi diacritice

### Cum veți trimite soluția

- veți pune codul sursă pe un URL accesibil, dar private (un server, sau [gist](https://gist.github.com) pe Github), și veți pune acest URL în formularul de înscriere
- proiectul îl puteți face în orice limbaj doriți voi
- dar, va trebui să conțină un `Makefile` care va genera un fișier executabil (un ghid scurt pentru utilitarul Make e [aici](http://mrbook.org/blog/tutorials/make/)), pe o mașină Linux Ubuntu
- binarul va trebui să primească 2 parametrii, primul este fișierul de intrare, iar al doilea este fișierul de ieșire

#### Testarea

Noi am pregătit un set de teste pe care le vom folosi, doar că vrem să facem lucrurile puțin mai interesante. Vă recomandăm când ne trimiteți soluția, să faceți un folder `tests`, unde să aveți fișiere de genul `test1.in` și `test1.out`. Vom aduna toate testele voastre și le vom folosi pentru a testa soluțiile celorlați care au submitat. Cei care reușesc să găsească teste care respectă restricțiile și pe care soluțiile celelalte pică, vor primi puncte bonus.

#### Intrebari

Folositi cu incredere sectiunea de [Issues](https://github.com/rosedu/problema-cdl-2016/issues) pentru a pune intrebari legate de problema.
