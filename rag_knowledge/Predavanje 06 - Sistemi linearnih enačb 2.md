# Sistemi linearnih enačb - nadaljevanje

## Razcep LU

Za rešitev sistema linearnih enačb zahteva Gaussov eliminacijski postopek najmanjše število računskih operacij. 

V primeru, ko se matrika koeficientov $\mathbf{A}$ ne spreminja in se spreminja zgolj vektor konstant $\mathbf{b}$, se je mogoče izogniti ponovni Gaussovi eliminaciji matrike koeficientov. Z razcepom matrike $\mathbf{A}$ lahko pridemo do rešitve z manj računskimi operacijami. V ta namen si bomo pogledali razcep LU!

Poljubno matriko lahko zapišemo kot produkt dveh matrik:

$$
\mathbf{A}=\mathbf{B}\,\mathbf{C}.
$$

Pri tem je možnosti za zapis matrik $\mathbf{B}$ in $\mathbf{C}$ neskončno veliko.

Pri razcepu LU zahtevamo, da je matrika $\mathbf{B}$ spodnje trikotna in matrika $\mathbf{C}$ zgornje trikotna:

$$
\mathbf{A}=\mathbf{L}\,\mathbf{U}.
$$

Vsaka od matrik $\mathbf{L}$ in $\mathbf{U}$ ima $(n+1)\,n/2$ neničelnih elementov; skupaj torej $n^2+n$ neznank. Znana matrika $\mathbf{A}$ definira $n^2$ vrednosti. Za enolično določitev matrik $\mathbf{L}$ in $\mathbf{U}$ torej manjka 
$n$ enačb. Tukaj bomo uporabili razcep LU, ki dodatne enačbe pridobi s pogojem $L_{ii}=1$, $i=0, 1,\dots,n-1$.

Sistem linearnih enačb:

$$
\mathbf{A}\mathbf{x}=\mathbf{b}
$$

torej zapišemo z razcepom matrike $\mathbf{A}$:

$$
\mathbf{L}\,\underbrace{\mathbf{U}\,\mathbf{x}}_{\mathbf{y}}=\mathbf{b}.
$$

Do rešitve sistema $\mathbf{A}\mathbf{x}=\mathbf{b}$ sedaj pridemo tako, da rešimo dva trikotna sistema enačb.

Najprej izračunamo vektor $\mathbf{y}$:

$$
\mathbf{L}\,\mathbf{y}=\mathbf{b}.\qquad \textrm{(direktno vstavljanje)}
$$

Ko je $\mathbf{y}$ izračunan, lahko iz:

$$
\mathbf{U}\,\mathbf{x}=\mathbf{y}\qquad \textrm{(obratno vstavljanje)}
$$

določimo $\mathbf{x}$.

### Razcep LU matrike koeficientov $\mathbf{A}$

V nadaljevanju bomo pokazali, da Gaussova eliminacija dejanjsko predstavlja razcep LU matrike koeficientov $\mathbf{A}$. Pri tem si bomo pomagali s simbolnim izračunom, zato uvozimo paket `sympy`:

```python
import sympy as sym # uvozimo sympy
sym.init_printing() # za lep prikaz izrazov
```

Prikaz začnimo na primeru simbolno zapisanih matrik $\mathbf{L}$ in $\mathbf{U}$ dimenzije $3\times 3$:

```python
L21, L31, L32 = sym.symbols('L21, L31, L32')
U11, U12, U13, U22, U23, U33 = sym.symbols('U11, U12, U13, U22, U23, U33')
L = sym.Matrix([[  1,   0,  0],
                [L21,   1,  0],
                [L31, L32,  1]])
U = sym.Matrix([[U11, U12, U13],
                [  0, U22, U23],
                [  0,   0, U33]])
```

Matrika koeficientov $\mathbf{A}$ zapisana z elementi matrik $\mathbf{L}$ in  $\mathbf{U}$ torej je:

```python
A = L*U
A
```

Izvedimo sedaj Gaussovo eliminacijo nad matriko koeficientov $\mathbf{A}$. 

S pomočjo prve vrstice izvedemo Gaussovo eliminacijo v prvem stolpcu:

```python
A[1,:] -= L21 * A[0,:]
A[2,:] -= L31 * A[0,:]
A
```

Nadaljujemo v drugem stolpcu:

```python
A[2,:] -= L32 * A[1,:]
A
```

Iz zgornje eliminacije ugotovimo:
1. matrika $\mathbf{U}$ je enaka matriki, ki jo dobimo, če izvedemo Gaussovo eliminacijo nad matriko koeficientov $\mathbf{A}$.
* izven diagonalni členi $\mathbf{L}$ so faktorji, ki smo jih uporabili pri Gaussovi eliminaciji.

### Numerična implementacija razcepa LU

Numerično implementacijo si bomo pogledali na sistemu, ki je definiran kot:

```python
import numpy as np

A = np.array([[8, -6, 3],
              [-6, 6, -6],
              [3, -6, 6]], dtype=float) 
b = np.array([-14, 36, 6], dtype=float)
```

Izvedimo Gaussovo eliminacijo in koeficiente, s katerim množimo pivotno vrsto `m`, shranimo v matriko `L` na mesto z indeksi, kot jih ima v matriki $\mathbf{A}$ eliminirani element.

```python
(v, s) = A.shape # v=število vrstic, s=število stolpcev
U = A.copy()     # pripravimo matriko U kot kopijo A
L = np.zeros_like(A) # pripravimo matriko L dimenzije enake A (vrednosti 0)
## eliminacija
for p, pivot_vrsta in enumerate(U[:-1]):
    for i, vrsta in enumerate(U[p+1:]):
        if pivot_vrsta[p]:
            m = vrsta[p]/pivot_vrsta[p]
            vrsta[p:] = vrsta[p:]-pivot_vrsta[p:]*m
            L[p+1+i, p] = m
    print('Korak: {:g}'.format(p))
    print(U)
```

```python
L
```

Dopolnimo diagonalo $\mathbf{L}$:

```python
for i in range(v):
    L[i, i] = 1.
```

```python
L
```

Sedaj rešimo spodnje trikotni sistem enačb $\mathbf{L}\,\mathbf{y}=\mathbf{b}$:

```python
## direktno vstavljanje
y = np.zeros_like(b)
for i, b_ in enumerate(b):
    y[i] = (b_ - np.dot(L[i, :i], y[:i]))
```

```python
y
```

Nadaljujemo z reševanjem zgornje trikotnega sistema $\mathbf{U}\,\mathbf{x}=\mathbf{y}$:

```python
U
```

```python
y
```

```python
## obratno vstavljanje
x = np.zeros_like(b)
for i in range(v-1, -1,-1):
    x[i] = (y[i] - np.dot(U[i, i+1:], x[i+1:])) / U[i, i]
x
```

Kakor smo navedli zgoraj, ob spremembi vektorja konstant $\mathbf{b}$ ponovna Gaussova eliminacija ni potrebna. Izvesti je treba samo direktno in nato obratno vstavljanje. Poglejmo primer:

```python
b = np.array([-1., 6., 7.])
y = np.zeros_like(b)
for i, b_ in enumerate(b):#direktno vstavljanje
    y[i] = (b_ - np.dot(L[i, :i], y[:i]))    
x = np.zeros_like(b)
for i in range(v-1, -1,-1):# obratno vstavljanje
    x[i] = (y[i] - np.dot(U[i, i+1:], x[i+1:])) / U[i, i]
x
```

```python
A.dot(x)
```

## Pivotiranje

Poglejmo si spodnji sistem enačb:

```python
A = np.array([[0, -6, 6],
              [-6, 6, -6],
              [8, -6, 3]], dtype=float) # poskusite tukaj izpustiti dtype=float in preverite rezultat
b = np.array([6, 36, -14], dtype=float)
```

Če bi izvedli Gaussovo eliminacijo v prvem stolpcu matrike $\mathbf{A}$:

```python
A[1,:] - A[1,0]/A[0,0] * A[0,:]
```

Opazimo, da imamo težavo z deljenjem z 0 v prvi vrstici. Elementarne operacije, ki jih nad sistemom lahko izvajamo, dovoljujejo zamenjavo poljubnih vrstic. Sistem lahko preuredimo tako, da pivotni element ni enak 0. Vseeno se lahko zgodi, da ima pivotni element, s katerim delimo, zelo majhno vrednost $\varepsilon$. Ker bi to povečevalo zaokrožitveno napako, izmed vseh vrstic za pivotno vrstico izberemo tisto, katere pivot ima največjo absolutno vrednost.

Če med Gaussovo eliminacijo zamenjamo vrstice tako, da je pivotni element največji, to imenujemo **pivotiranje vrstic** ali tudi **delno pivotiranje**. Tako dosežemo, da je Gaussova eliminacija numerično stabilna.

Pokazati je mogoče, da pri reševanju sistema enačb $\mathbf{A}\,\mathbf{x}=\mathbf{b}$, pri katerem je matrika $\mathbf{A}$ diagonalno dominantna, pivotiranje po vrsticah ni potrebno. Reševanje je brez pivotiranja numerično stabilno.

Pravokotna matrika $\mathbf{A}$ dimenzije $n$ je **diagonalno dominantna**, če je absolutna vrednost diagonalnega elementa vsake vrstice večja od vsote absolutnih vrednosti ostalih elementov v vrstici:

$$
\left|A_{ii}\right|> \sum_{j=1, j\ne i}^n \left|A_{ij}\right|
$$

### Gaussova eliminacija z delnim pivotiranjem

Pogledali si bomo Gaussovo eliminacijo z **delnim pivotiranjem**. Brez delnega pivotiranja v $i$-tem koraku eliminacije izberemo vrstico $i$ za pivotiranje. Pri delnem pivotiranju pa najprej preverimo, ali je $i$-ti diagonalni element po absolutni vrednosti največji element v stolpcu $i$ na ali pod diagonalo; če ni, zamenjamo vrstico $i$ s 
tisto vrstico pod njo, v kateri je v stolpcu $i$ po absolutni vrednosti največji element. Z delnim pivotiranjem zmanjšamo vpliv zaokrožitvene napake na rezultat. 

V koliko bi izvedli **polno pivotiranje**, bi poleg zamenjave vrstic uporabili tudi zamenjavo vrstnega reda spremenljivk (zamenjava stolpcev). Polno pivotiranje izboljša stabilnost, se pa redko uporablja in ga tukaj ne bomo obravnavali.

Algoritem za Gaussovo eliminacijo z delnim pivotiranjem torej je:

```python
def gaussova_eliminacija_pivotiranje(A, b, prikazi_korake=False):
    """ Vrne Gaussovo eliminacijo razširjene matrike koeficientov, 
    uporabi delno pivotiranje.

    :param A: matrika koeficientov
    :param b: vektor konstant
    :param prikazi_korake: ali izpišem posamezne korake
    :return Ab: trapezna razširjena matrika koeficientov
    """
    Ab = np.column_stack((A, b))
    for p in range(len(Ab)-1):
        p_max = np.argmax(np.abs(Ab[p:,p]))+p
        if p != p_max:
            Ab[[p], :], Ab[[p_max], :] = Ab[[p_max], :], Ab[[p], :]
        pivot_vrsta = Ab[p, :]                
        for vrsta in Ab[p + 1:]:
            if pivot_vrsta[p]:
                vrsta[p:] -= pivot_vrsta[p:] * vrsta[p] / pivot_vrsta[p]
        if prikazi_korake:
            print('Korak: {:g}'.format(p))
            print('Pivot vrsta:', pivot_vrsta)
            print(Ab)
    return Ab
```

```python
A
```

```python
Ab = gaussova_eliminacija_pivotiranje(A, b, prikazi_korake=True)
```

### Razcep LU z delnim pivotiranjem

Podobno kakor pri Gaussovi eliminaciji lahko tudi razcep LU razširimo z delnim pivotiranjem. Reševanje tako postane numerično **stabilno**. Pri tem moramo shraniti informacijo o zamenjavi vrstic, ki jo potem posredujemo v funkcijo za rešitev ustreznih trikotnih sistemov.

```python
def LU_razcep_pivotiranje(A, prikazi_korake=False):
    """ Vrne razcep LU matriko in vektor zamenjanih vrstic pivotiranje, 
    uporabi delno pivotiranje.

    :param A: matrika koeficientov
    :param prikazi_korake: izpišem posamezne korake
    :return LU: LU matrika
    :return pivotiranje: vektor zamenjave vrstic (pomembno pri iskanju rešitve)    
    """
    LU = A.copy()
    pivotiranje = np.arange(len(A))
    for p in range(len(LU)-1):
        p_max = np.argmax(np.abs(LU[p:,p]))+p
        if p != p_max:
            LU[[p], :], LU[[p_max], :] = LU[[p_max], :], LU[[p], :]
            pivotiranje[p], pivotiranje[p_max] = pivotiranje[p_max], pivotiranje[p]
        pivot_vrsta = LU[p, :]                
        for vrsta in LU[p + 1:]:
            if pivot_vrsta[p]:
                m = vrsta[p] / pivot_vrsta[p]
                vrsta[p:] -= pivot_vrsta[p:] * m
                vrsta[p] = m
            else:
                raise Exception('Deljenje z 0.')
        if prikazi_korake:
            print('Korak: {:g}'.format(p))
            print('Pivot vrsta:', pivot_vrsta)
            print(LU)
    return LU, pivotiranje
```

Poglejmo si primer:

```python
A = np.array([[0, -6, 6],
              [-6, 6, -6],
              [8, -6, 3]], dtype=float)
b = np.array([-14, 36, 6], dtype=float)
```

```python
lu, piv = LU_razcep_pivotiranje(A, prikazi_korake=True)
```

V zgornjem primeru smo uporabili kompakten način zapisa trikotnih matrik $\mathbf{L}$ in $\mathbf{U}$; vsaka je namreč definirana s 6 elementi, pri matriki $\mathbf{L}$ pa vemo, da so diagonalni elementi enaki 1. Matrika `lu` tako vsebuje $3\times3=9$ elementov:

```python
lu
```

Na diagonali in nad diagonalo so vrednosti zgornje trikotne matrike $\mathbf{U}$, pod diagonalo pa so poddiagonalni elementi matrike $\mathbf{L}$. Pri izračunu rešitve bomo upoštevali, da so diagonalne vrednosti $\mathbf{L}$ enake 1.

Numerični seznam `piv` nam pove, kako so bile zamenjane vrstice, kar je treba upoštevati pri izračunu rešitve:

```python
piv
```

Določimo sedaj še rešitev (koda za računanje rešitve je v modulu `orodja.py`)

```python
from moduli import orodja
```

```python
r = orodja.LU_resitev_pivotiranje(lu, b, piv)
r
```

Preverjanje rešitve:

```python
A@r
```

## Modul ``SciPy``

Modul ``SciPy`` temelji na ``numpy`` modulu in vsebuje veliko različnih visokonivojskih programov/modulov/funkcij. Teoretično ozadje modulov so seveda različni numerični algoritmi; nekatere spoznamo tudi v okviru tega učbenika. Dober vir teh numeričnih algoritmov v povezavi s `SciPy` predstavlja [dokumentacija](https://docs.scipy.org/doc/scipy/reference/tutorial/). Za odličen uvod v SciPy si lahko ogledate [YouTube posnetek: SciPy Tutorial (2022): For Physicists, Engineers, and Mathematicians](https://www.youtube.com/watch?v=jmX4FOUEfgU).

Kratek pregled hierarhije modula:

* Linearna algebra ([scipy.linalg](http://docs.scipy.org/doc/scipy/reference/linalg.html))
* Integracija ([scipy.integrate](http://docs.scipy.org/doc/scipy/reference/integrate.html))
* Optimizacija ([scipy.optimize](http://docs.scipy.org/doc/scipy/reference/optimize.html))
* Interpolacija ([scipy.interpolate](http://docs.scipy.org/doc/scipy/reference/interpolate.html))
* Fourierjeva transformacija ([scipy.fftpack](http://docs.scipy.org/doc/scipy/reference/fftpack.html))

* Problem lastnih vrednosti (redke matrike) ([scipy.sparse](http://docs.scipy.org/doc/scipy/reference/sparse.html))
* Statistika ([scipy.stats](http://docs.scipy.org/doc/scipy/reference/stats.html))
* Procesiranje signalov ([scipy.signal](http://docs.scipy.org/doc/scipy/reference/signal.html))
* Posebne funkcije ([scipy.special](http://docs.scipy.org/doc/scipy/reference/special.html))
* Večdimenzijsko procesiranje slik ([scipy.ndimage](http://docs.scipy.org/doc/scipy/reference/ndimage.html))
* Delo z datotekami ([scipy.io](http://docs.scipy.org/doc/scipy/reference/io.html))

V sledečih predavanjih si bomo nekatere podmodule podrobneje pogledali.

Poglejmo si, kako je znotraj ``SciPy`` implementiran *razcep LU*:

```python
from scipy.linalg import lu_factor, lu_solve
```

Funkcija ``scipy.linalg.lu_factor`` ([dokumentacija](https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.lu_factor.html)):

```python
lu_factor(a, overwrite_a=False, check_finite=True)
```

zahteva vnos matrike koeficientov (ali seznama matrik koeficientov) `a`, `overwrite_a` v primeru `True` z rezultatom razcepa prepiše vrednost `a` (to je lahko pomembno, da se prihrani spomin in poveča hitrost). Funkcija `lu_factor` vrne terko `(lu, piv)`:

* `lu` - $\mathbf{L}$ \ $\mathbf{U}$ matrika (matrika, ki je enake dimenzije kot `a`, vendar pod diagonalo vsebuje elemente $\mathbf{L}$, preostali elementi pa definirajo $\mathbf{U}$; diagonalni elementi $\mathbf{L}$ imajo vrednosti 1.
* `piv` - pivotni indeksi, predstavljajo permutacijsko matriko `P`: vrsta  `i` matrike `a` je bila zamenjana z vrsto `piv[i]` (v vsakem koraku se upošteva predhodno stanje, malo drugačna logika kot v naši funkciji `LU_razcep_pivotiranje`).

``lu_factor`` uporabljamo v paru s funkcijo ``scipy.linalg.lu_solve`` ([dokumentacija](https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.lu_solve.html)), ki nam poda rešitev sistema:

```python
lu_solve(lu_and_piv, b, trans=0, overwrite_b=False, check_finite=True)
```

`lu_and_piv` je terka rezultata `(lu, piv)` iz ``lu_factor``, `b` je vektor (ali seznam vektorjev) konstant. Ostali parametri so opcijski.

Poglejmo si uporabo:

```python
A = np.array([[0, -6, 6],            
              [-6, 6, -6],
              [8, -6, 3]], dtype=float)
b = np.array([-14, 36, 6], dtype=float)
```

```python
lu, piv = lu_factor(A)
```

```python
lu
```

```python
piv
```

Pridobimo rešitev:

```python
r = lu_solve((lu, piv), b)
r
```

Preverimo ustreznost rešitve:

```python
A@r
```

## Računanje inverzne matrike

Inverzno matriko h kvadratni matriki $\textbf{A}$ reda $n\times n$ označimo z $\textbf{A}^{-1}$. Je matrika reda $n\times n$,  takšna, da velja 

$$\mathbf{A}\,\mathbf{A}^{-1} = \mathbf{A}^{-1}\,\mathbf{A} = \mathbf{I},$$

kjer je $\mathbf{I}$ enotska matrika.

Najbolj učinkovit način za izračun inverzne matrike od matrike $\mathbf{A}$ je rešitev matrične enačbe:

$$\mathbf{A}\,\mathbf{X}=\mathbf{I}.$$ 

Matrika $\mathbf{X}$ je inverzna matriki $\mathbf{A}$: $\mathbf{A}^{-1}=\mathbf{X}$.

Izračun inverzne matrike je torej enak reševanju $n$ sistemov $n$ linarnih enačb:

$$\mathbf{A}\,\mathbf{x}_i=\mathbf{b}_i,\qquad i=0,1,2,\dots,n-1,$$

kjer je $\mathbf{b}_i$ $i$-ti stolpec matrike $\mathbf{B}=\mathbf{I}$.

*Numerična zahtevnost:* izvedemo razcep LU nad matriko $\mathbf{A}$ (računski obseg reda $n^3$) in nato 
poiščemo rešitev za vsak $\mathbf{x}_i$ ($2\,n^2$ računskih operacij za vsak $i$). Skupni računski obseg je torej še vedno reda $n^3$. (Če bi $n$ krat izvajali Gaussovo eliminacijo, bi bil obseg reda $n^4$.)

```python
lu_piv = lu_factor(A)
lu_piv
```

Tukaj se sedaj pokaže smisel razcepa LU; razcep namreč izračunamo samo enkrat in potem za vsak vektor konstant poiščemo rešitev tako, da rešimo dva trikotna sistema. Če bi sisteme reševali po Gaussovi metodi, bi rabili $(n^3+n^2)\,n$ računskih operacij. 

Izračunajmo inverzno matriko od matrike $\textbf{A}$. Najprej z uporabo `np.identity()` pripravimo enotsko matriko:

```python
np.identity(len(A))
```

Nato rešimo sistem enačb za vsak stolpec enotske matrike:

```python
A_inv = np.zeros_like(A) #zeros_like vrne matriko oblike in tipa matrike A z vrednostmi 0
for b, a_inv in zip(np.identity(len(A)), A_inv):
    a_inv[:] = lu_solve(lu_piv, b)
A_inv = A_inv.T
```

Rešitev torej je:

```python
A_inv
```

Preverimo rešitev:

```python
A@A_inv
```

Rešitev z uporabo Numpy:

```python
#%%timeit
np.linalg.inv(A)
```

Z ustrezno uporabo funkcij iz modula `SciPy` lahko do rešitve pridemo še hitreje. V funkcijo ``lu_solve`` lahko vstavimo vektor $\mathbf{b}$ ali matriko $\mathbf{B}$, katere posamezni stolpec $i$ predstavlja nov vektor konstant $\mathbf{b}_i$.

```python
#%%timeit
lu_solve(lu_piv, np.identity(len(A)))
```

## Reševanje predoločenih sistemov

```python
from IPython.display import YouTubeVideo
YouTubeVideo('N6upu99LbfQ', width=800, height=300)
```

Kadar rešujemo sistem $m$ linearnih enačb z $n$ neznankami ter velja $m>n$ in je rang $n+1$, imamo predoločeni sistem.

Predoločeni (tudi **nekonsistenten sistem**):

$$\mathbf{A}\,\mathbf{x}=\mathbf{b},$$

nima rešitve. Lahko pa poiščemo najboljši približek rešitve z metodo **najmanjših kvadratov**.

Vsota kvadratov preostankov je definirana s skalarnim produktom:

$$\left\lVert r\right\rVert ^2=(\mathbf{A}\,\mathbf{x}-\mathbf{b})^T\,(\mathbf{A}\,\mathbf{x}-\mathbf{b})$$

kar preoblikujemo v:

$$\left\lVert r\right\rVert^2=\mathbf{x}^T\mathbf{A}^T\mathbf{A}\mathbf{x}-2\mathbf{b}^T\mathbf{A}\mathbf{x}+\mathbf{b}^T\mathbf{b},
$$

kjer smo upoštevali, da zaradi skalarne vrednosti velja: $\mathbf{b}^T\mathbf{A}\mathbf{x}=(\mathbf{b}^T(\mathbf{A}\mathbf{x}))^T=(\mathbf{A}\mathbf{x})^T\mathbf{b}$.

Rešitev enačbe, gradient vsote kvadratov, določa njen minimum:

$$
\nabla_x\,\left\lVert r\right\rVert^2=2\mathbf{A}^T\mathbf{A}\,\mathbf{x}-2\mathbf{A}^T\,\mathbf{b}=0
$$

Tako iz normalne enačbe:

$$
\mathbf{A}^T\mathbf{A}\,\mathbf{x}=\mathbf{A}^T\,\mathbf{b}
$$

določimo najboljši približek rešitve:

$$
\mathbf{x}=\left(\mathbf{A}^T\mathbf{A}\right)^{-1}\mathbf{A}^T\,\mathbf{b}.
$$

Z vpeljavo **psevdo inverzne matrike**: 

$$
\mathbf{A}^+=\left(\mathbf{A}^T\mathbf{A}\right)^{-1}\mathbf{A}^T
$$

k matriki $\mathbf{A}$ je rešitev predoločenega sistema zapisana:

$$
\mathbf{x}=\mathbf{A}^+\,\mathbf{b}.
$$

Zgoraj predstavljen postopek je relativo enostaven, je pa numerično zahteven in lahko v nekaterih primerih slabo pogojen; priporočeno je da v praksi psevdo inverzno matriko izračunamo z uporabo funkcij:

* `numpy.linalg.pinv` iz modula `numpy` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.linalg.pinv.html)),
* Funkcij `pinv`, `pinv2` ali `pinvh` iz modula `scipy.linalg` (izbira je odvisna od obravnavanega problema; glejte [dokumentacijo](https://docs.scipy.org/doc/scipy/reference/linalg.html)),

ki temeljijo na boljših numerični metodah.

Primer sistema z enolično rešitvijo:

```python
## število enačb enako številu neznank
A = np.array([[1., 2],
              [2, 3]])
b = np.array([5., 8])
np.linalg.solve(A, b)
```

Naredimo sedaj predoločeni sistem (funkcija `numpy.vstack` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.vstack.html)) sestavi sezname po stolpcih, `numpy.random.seed` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.random.seed.html)) ponastavi generator naključnih števil na vrednost semena `seed`, `numpy.random.normal` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.random.normal.html)) pa generira normalno porazdeljeni seznam dolžine `size` in standardne deviacije `scale`):

```python
vA = np.vstack([A,A,A]) 
np.random.seed(seed=0)
vA += np.random.normal(scale=0.01, size=vA.shape) # pokvarimo rešitev -> sistem je predoločen
vA # matrika koeficientov
```

```python
vb = np.hstack([b,b,b])
vb += np.random.normal(scale=0.01, size=vb.shape)
vb # vektor konstant
```

Rešimo sedaj predoločen sistem:

```python
Ap = np.linalg.pinv(vA)
Ap.dot(vb)
```

Vidimo, da predoločeni sistem z naključnimi vrednostmi (simulacija šuma pri meritvi) poda podoben rezultat kakor rešitev brez šuma. V kolikor bi nivo šuma povečevali, bi se odstopanje od enolične rešitve povečevalo.

Psevdo inverzno matriko lahko določimo tudi sami in preverimo razliko z vgrajeno funkcijo:

```python
#%%timeit # preverite hitrost!
Ap2 = np.linalg.inv(vA.T@vA) @ vA.T

Ap2 - Ap
```

## Iterativne metode

Pogosto se srečamo z velikimi sistemi linearnih enačb, katerih matrika koeficientov ima malo od nič različnih elementov (take matrike imenujemo **redke** ali tudi **razpršene**, angl. *sparse*). 

Pri reševanju takih sistemov linearnih enačb se zelo dobro izkažejo iterativne metode; prednosti v primerjavi z direktnimi metodami so:

* računske operacije se izvajajo samo nad neničelnimi elementi (kljub iterativnemu reševanju jih je lahko manj)
* zahtevani spominski prostor je lahko neprimerno manjši.

### Gauss-Seidlova metoda

V nadaljevanju si bomo pogledali idejo *Gauss-Seidelove* iterativne metode. Najprej sistem enačb $\mathbf{A}\,\mathbf{x}=\mathbf{b}$ zapišemo kot:

$$
\sum_{j=0}^{n-1} A_{ij}x_j=b_i\qquad{}i=0,1,\dots,n-1.
$$

Predpostavim, da smo v $k-1$ koraku iterativne metode in so znani približki $x_{j}^{(k-1)}$ ($j=0,1,\dots, n-1$). Iz zgornje vsote izpostavimo člen $i$:

$$
A_{ii}\,x_{i}^{(k-1)} +\sum_{j=0, j\ne i}^{n-1} A_{ij}x_{j}^{(k-1)}=b_i\qquad{}i=0,1,\dots,n-1.
$$

Ker približki $x_{j}^{(k-1)}$ ne izpolnjujejo natančno linearnega problema, lahko iz zgornje enačbe določimo nov približek $x_{i}^{(k)}$:

$$
x_{i}^{(k)} =\frac{1}{A_{ii}}\left(b_i-\sum_{j=0}^{i-1} A_{ij}x_{j}^{(k)}-\sum_{j=i+1}^{n-1} A_{ij}x_{j}^{(k-1)}\right)
$$

Vsoto smo razdelili na dva dela in za izračun $i$-tega člena upoštevali v $k$-ti iteraciji že določene člene z indeksom manjšim od $i$.

Iterativni pristop prekinemo, ko dosežemo želeno natančnost rešitve $\epsilon$:

$$
||x_{i}^{(k)}-x_{i}^{(k-1)}||<\epsilon
$$

### Zgled

```python
A = np.array([[8, -1, 1],
              [-1, 6, -1],
              [0, -1, 6]], dtype=float) 
b = np.array([-14, 36, 6], dtype=float)
```

Začetni približek:

```python
x = np.zeros(len(A))
x
```

Pripravimo matriko $\mathbf{A}$ brez diagonalnih elementov (Zakaj? Poskusite odgovoriti spodaj, ko bomo izvedli iteracije.)

```python
K = A.copy() #naredimo kopijo, da ne povozimo podatkov
np.fill_diagonal(K, np.zeros(3)) #spremenimo samo diagonalne elemente
```

```python
K
```

Izvedemo iteracije:

$$
x_{i}^{(k)} =\frac{1}{A_{ii}}\left(b_i-\sum_{j=0}^{i-1} A_{ij}x_{j}^{(k)}-\sum_{j=i+1}^{n-1} A_{ij}x_{j}^{(k-1)}\right)
$$

(Ker bomo vrednosti takoj zapisali v `x`, ni treba razbiti vsote na dva dela)

```python
for k in range(3):
    xk_1 = x.copy()
    print(5*'-' + f'iteracja {k}' + 5*'-')
    for i in range(len(A)): #opazujte kaj se dogaja, ko to celico poženete večkrat!
        x[i] = (b[i]-K[i,:].dot(x))/A[i,i]
        print(f'Približek za element {i}', x)
    e = np.linalg.norm(x-xk_1)
    print(f'Norma {e}')
```

Preverimo rešitev

```python
A@x
```

Metoda deluje dobro, če je matrika diagonalno dominantna (obstajajo pa metode, ki delujejo tudi, ko matrika ni diagonalno dominantna, glejte npr.: J. Petrišič, Reševanje enačb, 1996, str 149: Metoda konjugiranih gradientov).

---

# Vprašanja za vaje

---

## LU razcep

**Vprašanje 1:** Enačbe ravnotežja sil in momentov za nosilec na sliki zapišemo:

<img src="fig/vaje/nosilec.PNG" style="width:550px">

Določite reakcije v podporah za pet različnih kotov $\varphi$ iz podanega seznama, tako, da razcep matrike koeficientov opravite le enkrat. Rešitev vsakič izpišite s funkcijo ``print``.

$$
\begin{align}
\text{smer }x &:\quad  A_x = F\,\cos(\varphi)\\
\text{smer }y &:\quad A_y + B = F\,\sin(\varphi)\\
\text{moment okoli }A&:\quad Bl = Fa\,\sin(\varphi)
\end{align}
$$

```python
# Podatki
L = 1.5 # m
a = 0.55 # m
F = 85 # N
seznam_phi = [0, np.pi/6, np.pi/4, np.pi/3, np.pi/2]
```

----------

## Računanje inverza matrike

**Vprašanje 2:** uporabite LU razcep in po zgoraj opisanem postopku določite inverz matrike $\mathbf{A}$ iz prejšnje naloge.

-------------

## Reševanje predoločenih sistemov - psevdoinverz

**Vprašanje 3:** Določite aproksimacijsko premico, ki z minimalno kvadratično napako opiše $n$ podanih točk $(X, Y)$. Parametra $k$ in $n$ aproksimacijske premice $y = kx+n$ poiščite z rešitvijo sistema linearnih enačb:

$$\mathbf{[X, 1]}_{~n\times 2}~\mathbf{x} = \mathbf{[Y]}$$

kjer so v prvem stolpcu matrike koeficientov koordinate $X$ podanih točk, v drugem stolpcu pa same enice. Vektor desnih strani je enak vektorju $Y$ koordinat obravnvanih točk. 

Izrišite na isti sliki množico točk $(X, Y)$ in dobljeno aproksimacijsko premico.

```python
# Podatki
X = np.linspace(1, 10, 100)
Y = 0.7*X - 15.5 + np.random.randn(len(X)) 
plt.plot(X, Y, 'b.')
```

-------------

## Iterativne metode reševanja sistemov linearnih enačb

### Gauss-Seidel


$$x_i =\frac{1}{A_{ii}}\left(b_i-\sum_{j=0, j\ne i}^n A_{ij}x_j\right)\qquad{}i=0,1,\dots,n-1$$

**Vprašanje 4:** Z uporabo iterativne Gauss-Seidel metode rešite podan sistem enačb. 

Primerjajte rezultat z Gaussovo eliminacijo. Opazujte vpliv števila iteracij na natančnost rešitve.

```python
# Podatek
A = np.array([[-1, 2, 0, 0, 0, 0],
             [-1, 3, 1, 0, 0, 0],
             [0, -1, 4, 1, 0, 0],
             [0, 0, -2, 5, 0, 0],
             [0, 0, 0, 0, 5, 3],
             [0, 0, 0, 0, 1, -3]], dtype=float)

b = np.array([0, 18, 0, 12, 3, 5], dtype=float)
```
