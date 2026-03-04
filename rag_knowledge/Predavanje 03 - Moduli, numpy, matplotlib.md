```python
# Numpy, matplotlib
```

## Osnove modula ``numpy``

Kakor je omenjeno zgoraj, lahko modul ``numpy`` namestimo z upravljalnikom paketov ``pip`` z naslednjim ukazom (v kolikok ga še nimamo nameščenega):
```python
pip install numpy
```
oz. paket posodobimo na najnovešo različico:
```python
pip install numpy --upgrade
```

Najprej uvozimo modul (uveljavljeno je, da ga uvozimo v kratki obliki `np`):

```python
import numpy as np
```

Gre za enega najbolj pomembnih modulov. Na kratko: gre za visoko optimiran modul za numerične izračune; zelo dobri tutoriali so zbrani na naslovu: [github.com/numpy/numpy-tutorials](https://github.com/numpy/numpy-tutorials). Za odličen uvod v Numpy si lahko ogledate [YouTube posnetek: NumPy Tutorial (2021): For Physicists, Engineers, and Mathematicians](https://www.youtube.com/watch?v=DcfYgePyedM)

Poglejmo si najprej sintakso za vektor ničel ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.zeros.html)):
```python
numpy.zeros(shape, dtype=float, order='C')
```
argumenti so:

* `shape` definira obliko (lahko večdimenzijsko numerično polje),
* `dtype` definira tip podatka,
* `order` definira vrstni red (lahko je C ali F kot Fortran).

Poglejmo primer:

```python
np.zeros((3,4))
```

ali pa:

```python
np.zeros((3,5))
```

Podobno kot `zeros` se obnaša `ones`, vendar je namesto ničel vrednost 1 ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ones.html#numpy.ones)). 

Poglejmo si primer, kjer definiramo tudi tip `int` (privzeti tip je `float`):

```python
np.ones(4, dtype=int)
```

Pogosto bomo tudi uporabljali razpon vrednosti `arange` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.arange.html)):
```python
numpy.arange([start, ]stop, [step, ]dtype=None)
```
kjer so argumenti:

* `start` začetna vrednost razpona (privzeto 0),
* `stop` končna vrednost razpona,
* `step` korak in 
* `dtype` tip vrednosti (če tip ni podan, se vzame tip ostalih argumetnov, npr. `step`).

Poglejmo primer razpona od 0 do 9 (kakor vedno pri Pythonu *od* je vključen, *do* pa ni):

```python
np.arange(9)
```

ali pa od 7 do 12 po koraku 2, vendar število s plavajočo vejico:

```python
np.arange(7, 12, 2, dtype=float)
```

Še eno funkcijo bomo pogosto uporabili, to je `linspace` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.linspace.html)):
```python
numpy.linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
```
ki definiranemu razponu vrne numerično polje vrednosti na enaki razdalji (ekvidistanten razmik).

Argumenti so:

* `start` začetna vrednost razpona,
* `stop` končna vrednost razpona,
* `num` število točk/vozlišč, 
* `endpoint` ali je vrednost pri `stop` vključena ali ne,
* `retstep` v primeru `True` vrne funkcija terko `(rezultat, korak)`
* `dtype` tip vrednosti (če tip ni podan, se vzame tip ostalih argumetnov, npr. `step`).

Primer generiranja 10 točk na razponu od $-\pi$ do vključno $+\pi$:

```python
np.linspace(-np.pi, np.pi, 10)
```

Mimogrede smo zgoraj spoznali, da ima `numpy` vgrajene konstante (npr. $\pi$):

Poglejmo še vgrajene funkcije za generiranje naključnih števil. Te najdemo v `numpy.random` ([dokumentacija](https://docs.scipy.org/doc/numpy-1.16.0/reference/routines.random.html)). 

Najprej si poglejmo funkcijo `numpy.random.seed()`, ki se uporablja za ponastavitev generatorja naključnih števil ([dokumentacija](https://docs.scipy.org/doc/numpy-1.16.0/reference/generated/numpy.random.seed.html#numpy.random.seed)). To pomeni, da lahko z istim semenom (angl. *seed*) različni uporabniki generiramo ista naključna števila!

Spodnja vrstica:

```python
np.random.seed(0)
```

bo povzročila, da bo klic generatorja naključnih števil z enakomerno porazdelitvijo `numpy.random.rand()` ([dokumentacija](https://docs.scipy.org/doc/numpy-1.16.0/reference/generated/numpy.random.rand.html#numpy.random.rand)) vedno rezultiral v iste vrednosti:

```python
np.random.rand(3)
```

Preizkusimo ali je res, kar smo zapisali, in ponastavimo seme in kličimo generator:

```python
np.random.seed(0)
np.random.random(3)
```

### Zapis matrik in vektorjev

Matrika je dimenzije ``m x n``, kjer je na prvem mestu ``m`` število vrstic in ``n`` število stolpcev. Primer definiranja matrike dimenzije `m x n = 3 x 2` je:

```python
a = np.zeros((3, 2))
a
```

Vektor je lahko zapisan kot **vrstični vektor**:

```python
b = np.zeros(3) # (1 x 3)
b
```

ali kot **stolpični vektor**:

```python
c = np.zeros((3, 1)) # 3 x 1
c
```

V modulu ``numpy`` lahko vektorje in matrike zapisujemo kot:

* `numpy.array` (priporočeno, [dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.array.html)),
* `numpy.matrix` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.matrix.html)). 

Priporočena je prva oblika (`numpy.array`), ki pa ne sledi povsem matematičnemu zapisu (več o tem pozneje), nam pa omogoča **enostavnejše** programiranje in je tudi numerično **bolj učinkovit** pristop ([vir](http://wiki.scipy.org/NumPy_for_Matlab_Users#head-e9a492daa18afcd86e84e07cd2824a9b1b651935)). Pristopa `numpy.matrix` tukaj ne bomo obravnavali.

V angleškem jeziku bomo *array* prevajali kot **večdimenzijsko numerično polje** ali včasih **večdimenzijske sezname** (ker imajo nekatere podobnosti z navadnimi seznami). Nekatere knjige *array* tukaj prevajajo kot *tabela*.

### Rezanje

**Rezanje** (angl. *slicing*) seznamov smo si že pogledali v poglavju Uvod v Python. Podobno rezanje, vendar bolj splošno, velja tudi za numerična polja modula `numpy`.

Sintaksa rezanja ([dokumentacija](http://docs.scipy.org/doc/numpy/reference/arrays.indexing.html)) je:
```python
numpy_array[od:do:korak]
```
pri tem velja:

* indeksiranje se začne z 0 (kot sicer pri Pythonu),
* **`od`** pomeni **>=**,
* **`do`** pomeni **<**,
* `od`, `do`, `korak` so opcijski parametri,
* če parameter `od` ni podan, pomeni od *začetka*,
* če parameter `do` ni podan, pomeni do vključno zadnjega,
* če parameter `korak` ni podan, pomeni korak 1.

Primer od elementa 3 do elementa 8 po koraku 2:

```python
b = np.arange(10)
b[3:8:2]
```

Primer od elementa 3 naprej:

```python
b[3:]
```

Primer od elementa 3 naprej, vendar vsak tretji:

```python
b[3::3]
```

Primer zadnjih 5 elementov, vendar vsak drugi:

```python
b[-5::2]
```

Primer zadnjih 5 elementov brez zadnjih 2, vendar vsak drugi:

```python
b[-5:-2:2]
```

### Poglejmo si še rezanje večdimenzijskega numeričnega polja. 

Večdimenzijsko rezanje izvedemo tako, da dimenzije ločimo z vejico:
```python
numpy_array[rezanje0, rezanje1,...]
```
kjer `rezanje0` reže indeks 0 (prvo dimenzijo) v obliki `od:do:korak`, `rezanje1` reže indeks 1 (drugo dimenzijo) in tako naprej.

Poglejmo si primer; najprej pripravimo seznam 15 števil (`np.arange`), nato z metodo `reshape()` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.reshape.html)) spremenimo obliko v matriko `3 x 5`:

```python
a = np.arange(15)
a = a.reshape((3,5))
a
```

Obliko numeričnega polja lahko preverimo z atributom `shape` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.shape.html)).

```python
a.shape
```

Atribute bomo sicer podrobno spoznali pri obravnavi razredov.

Prikažimo vrstice z indeksom 0:

```python
a[0]
```

Isti rezultat bi dobili s prikazom vrstice z indeksom 0 in vseh stolpcev:

```python
a[0,:]
```

Pogosto želimo dostopati do stoplcev, npr. stolpca z indeksom 1 (torej režemo vse vrstice in stolpec z indeksom 1):

```python
a[:,1]
```

Poglejmo si še primer rezanja prvih dveh vrstic in zadnjih dveh stolpcev:

```python
a[:2, -2:]
```

Podobna logika se uporabi pri dimenzijah višjih od 2.

### Operacije nad numeričnimi polji

Poglejmo si sedaj bolj podrobno nekatere osnovne prednosti numeričnega polja `numpy.array` v primerjavi z navadnim seznamom Python.

Najprej pripravimo navaden Pythonov seznam:

```python
a = [1, 2, 3, 4, 5, 6, 7]
a
```

In nato še numerično polje `numpy.array` (kar iz seznama `a`):

```python
b = np.array(a)
b
```

Ko izpišemo `b`, smo opozorjeni, da gre za `array([...])`.

Poglejmo, kako se obnaša Pythonov seznam pri množenju:

```python
2*a
```

Opazimo, da se podvoji seznam, ne pa vrednosti, kar bi morebiti pričakovali.

Poglejmo, kako se pri množenju obnaša numerično polje `numpy.array`:

```python
2*b
```

Opazimo, da se podvojijo vrednosti; tako kakor bi pričakovali, ko množimo na primer skalarno vrednost in vektor!

### Aritmetične operacije

Izbor aritmetičnih operacij, ki jih ``numpy`` izvaja na nivoju posameznega elementa, je (po naraščajoči prioriteti):

* `x + y` vsota,	 	 
* `x - y` razlika,	 	 
* `x * y` produkt,	 	 
* `x / y` deljenje,	 	 
* `x // y` celoštevilsko deljenje (rezultat je celo število zaokroženo navzdol),
* `x % y` ostanek pri celoštevilskem deljenju,	 
* `x ** y`	vrne `x` na potenco `y`.

Primer:

```python
b + 3*b - b**2
```

Vse aritmetične operacije so sicer navedene v [dokumentaciji](https://docs.scipy.org/doc/numpy/reference/routines.math.html#arithmetic-operations) in namesto kratkih oblik imamo tudi *dolge*, npr: `numpy.power(x, y)` namesto `x**y`; primer:

```python
np.power(b, 2)
```

Mimogrede opazimo, da je rezultat tipa `int32` (integer). Ko smo ustvarili ime `b`, smo namreč ustvarili numerično polje z elementi tipa `int32`.

### Matematične funkcije

`numpy` ponuja praktično vse potrebne matematične (in druge) operacije, navedimo jih po skupinah, kot so strukturirane v [dokumentaciji](https://docs.scipy.org/doc/numpy/reference/routines.math.html#mathematical-functions):

* [trigonometrične funkcije](https://docs.scipy.org/doc/numpy/reference/routines.math.html#trigonometric-functions),
* [hiperbolične](https://docs.scipy.org/doc/numpy/reference/routines.math.html#hyperbolic-functions),
* [funkcije za zaokroževanje](https://docs.scipy.org/doc/numpy/reference/routines.math.html#rounding),
* [funkcije za vsoto, produkt in odvod](https://docs.scipy.org/doc/numpy/reference/routines.math.html#sums-products-differences),
* [eksponenti in logaritmi](https://docs.scipy.org/doc/numpy/reference/routines.math.html#exponents-and-logarithms),
* [posebne](https://docs.scipy.org/doc/numpy/reference/routines.math.html#other-special-functions) ter [preostale](https://docs.scipy.org/doc/numpy/reference/routines.math.html#miscellaneous) funkcije.

Poglejmo primer funkcije $\sin()$:

```python
a = np.linspace(0, 2*np.pi, 5)
np.sin(a)
```

Poglejmo še hitrost izvajanja:

```python
%timeit -n100 np.sin(a)
```

### Podatkovni tipi

``numpy`` ima vnaprej definirane podatkovne tipe (statično). Celoten seznam možnih tipov je naveden v [dokumentaciji](http://docs.scipy.org/doc/numpy/reference/arrays.dtypes.html).

Osredotočili se bomo predvsem na sledeče tipe:

* ``int`` - celo število (poljubno veliko)
* ``float`` - število s plavajočo vejico ([dokumentacija](https://docs.python.org/dev/library/functions.html#float))
* ``complex`` - kompleksno število s plavajočo vejico
* ``object`` - Python objekt.

Poglejmo si nekaj primerov (cela števila, število s plavajočo vejico in kompleksna števila):

```python
np.arange(5, dtype=int)
```

```python
np.arange(5, dtype=float)
```

```python
np.arange(5, dtype=complex)
```

### Spreminjanje elementov numeričnega polja (`numpy.array`)

Podatke spreminjamo na podoben način kakor pri navadnih seznamih; v kolikor uporabljamo rezanje, moramo paziti, da so na levi in desni strani enačaja podatki iste oblike (`array.shape`).

Poglejmo si primer, ko matriki ničel dimenzije `3 x 4` spremenimo element z indeksom `[2, 3]`:

```python
a = np.zeros((3, 4))
a[2, 3] = 100
a
```

Sedaj spremenimo še elemente prvih dveh vrstic in prvih dveh stolpcev v vrednost 1:

```python
a[:2, :2] = np.ones((2, 2))
a
```

Z 2 pomnožimo stolpec z indeksom 1:

```python
a[:,1] = 2 * a[:,1]
a
```

Bodite pozorni na to, da na tak način naredimo *pogled* (view) na podatke (**ne naredimo kopije podatkov**). 

Za primer najprej naredimo novo ime `pogled`:

```python
pogled = a[:, 2]
pogled
```

Sedaj spremenimo izbrane vrednosti numeričnega polja `a`:

```python
a[:, 2] = 5
a
```

Vrednosti `pogled` nismo spreminjali. Ker pa ime kaže na isto mesto kakor `a[:, 2]`, so vrednosti spremenjene:

```python
pogled
```

Če želimo kopijo, moramo narediti tako:

```python
kopija = a[:, 2].copy()
kopija
```

in rezultat `kopija` ostane nespremenjen:

```python
a[:, 2] = 2
kopija
```

### Osnove matričnega računanja

Če želite ponoviti matematične osnove matričnega računanja, potem sledite tej [povezavi](http://www.fmf.uni-lj.si/~kosir/poucevanje/skripta/matrike.pdf) (gre za kratek in dober pregled prof. dr. T. Koširja). Pogledali bomo, kako matrične račune izvedemo s pomočjo paketa `numpy`.

Najprej definirajmo matriki $\mathbf{A}$ in $\mathbf{B}$:

```python
A = np.array([[1, 2], [3, 2]])
B = np.array([[1, 1], [2, 2]])
```

ter vektorja $\mathbf{x}$ in $\mathbf{y}$.

```python
x = np.array([1, 2])
y = np.array([3, 4])
```

Skalarni produkt dveh vektorjev izvedemo s funkcijo `dot()` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.dot.html)):
```python
numpy.dot(a, b, out=None)
```
kjer argumenta `a` in `b` predstavljata numerični polji `numpy.array` (poljubne dimenzije), ki jih želimo množiti. Če sta `a` in `b` dimenzije 1 se izvede skalarni produkt. Pri dimenziji 2 (matrike) se izračuna produkt matrik. Za uporabo funkcije `dot()` pri dimenzijah več kot 2: glejte [dokumentacijo](https://docs.scipy.org/doc/numpy/reference/generated/numpy.dot.html).

Poglejmo primer množenja dveh vektorjev, to lahko izvedemo tako:

```python
np.dot(x, y)
```

ali tudi tako:

```python
x.dot(y)
```

Zgoraj smo omenili, da `numpy.array` ne sledi dosledno matematičnemu zapisu. Če bi, bi namreč eden od vektorjev moral biti vrstični, drugi stolpični. `numpy` to poenostavi in zato je koda lažje berljiva in krajša.

Poglejmo sedaj množenje matrike z vektorjem (opazimo, da transponiranje `x` ni potrebno):

```python
np.dot(A, x)
```

Lahko pa seveda pripravimo matematično korektno transponirano obliko vektorja (ampak vidimo, da je zapis neroden):

```python
A.dot(np.transpose([x]))
```

Transponiranje ima sicer tudi kratko obliko, prek atributa `T`, npr. za matriko $\mathbf{A}$:

```python
A.T
```

Poglejmo si primer množenja dveh matrik:

```python
np.dot(A, B)
```

Od Pythona 3.5 naprej se za množenje matrik (in vektorjev) uporablja tudi operator `@` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.matmul.html#numpy.matmul)), ki omogoča kratek in pregleden zapis.

Zgornji primeri zapisani z operatorjem `@`:

```python
x @ y
```

```python
A @ x
```

```python
A @ B
```

**Vektorski produkt** izračunamo s funkcijo `numpy.cross()` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.cross.html)):
```python
numpy.cross(a, b, axisa=-1, axisb=-1, axisc=-1, axis=None)
```
kjer `a` in `b` definirata komponente vektorjev. Če sta podani samo dve komponenti ($x$ in $y$), se izračuna skalarna vrednost (komponenta $z$); če so podane tri komponente, je rezultat tudi vektor s tremi komponentami. Uporaba funkcije je možna tudi na večdimenzijskih numeričnih poljih in temu so namenjeni preostali argumenti (glejte dokumentacijo).

Primer vektorskega produkta ravninskih vektorjev:

```python
x = np.array([1, 0])
y = np.array([0, 1])
np.cross(x, y)
```

Primer vektorskega produkta prostorskih vektorjev:

```python
x = np.array([1, 0, 0])
y = np.array([0, 1, 0])
np.cross(x, y)
```

### Nekatere funkcije knjižnice ``numpy``

Pogledali si bomo še nekatere funkcije, ki jih bolj ali manj pogosto potrebujemo.

Enotsko matriko definiramo s funkcijo `numpy.identity()` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.identity.html)):
```python
numpy.identity(n, dtype=None)
```
kjer argument `n` definira število vrstic in stolpcev pravokotne matrike. Tip `dtype` je privzeto `float`.

Primer enotske matrike:

```python
A = np.identity(3)
A
```

Do diagonalnih elementov matrike dostopamo s pomočjo funkcije `numpy.diagonal()` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.diagonal.html))
```python
numpy.diagonal(a, offset=0, axis1=0, axis2=1)
```
Če je matrika dvodimenzijska, potem funkcija s privzetimi argumenti vrne diagonalno os. Če je dimenzija višja od 2, se uporabi osi `axis1` in `axis2`, da se izloči dvodimenzijsko polje, nato pa določi diagonalo glede na elemente `[i, i+offset]`.

Poglejmo primer izločanja diagonale:

```python
np.diagonal(A)
```

in uporabe `offset=1` za sosednjo diagonalo (najprej pripravimo nesimetirčno matriko):

```python
A[0, 1] = 10
np.diagonal(A, offset=1)
```

Podobno sintakso kot `numpy.diagonal()` ima funkcija `numpy.trace()`, ki izračuna vsoto (sled) diagonalnih elementov ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.trace.html)):
```python
numpy.trace(a, offset=0, axis1=0, axis2=1, 
            dtype=None, out=None)
```

Primer sledi diagonale:

```python
np.trace(A)
```

in potem sosednje diagonale:

```python
np.trace(A, offset=1)
```

Pogosto nas zanimata največji ali najmanjši element nekega numeričnega polja. `numpy` je tukaj zelo splošen. Poglejmo si na primeru funkcije `numpy.max()` ([dokumentacija](https://docs.scipy.org/doc/numpy-1.13.0/reference/generated/numpy.amax.html)):
```python
numpy.max(a, axis=None, out=None)
```
Izpostavimo argument `axis`, ki pove, čez kateri indeks iščemo maksimalno vrednost. Če je `axis=None` se določi največja vrednost v celotnem polju.

Primer izračuna največje vrednosti celotnega polja (prej poglejmo `A`):

```python
A
```

```python
np.max(A, axis=1)
```

Primer izračuna največje vrednosti čez vrstice (torej po stolpcih):

```python
np.max(A, axis=0)
```

Primer izračuna največje vrednosti čez stolpce (torej po vrsticah):

```python
np.max(A, axis=1)
```

Par funkcije `max()` je `numpy.argmax()`, kateri določi indekse največje vrednosti.

Primer uporabe:

```python
A
```

```python
np.argmax(A, axis=0)
```

### Linearna algebra z ``numpy``

Pozneje bomo linearno algebro bolj podrobno spoznali in bomo sami pisali algoritme. Tukaj si poglejmo nekatere osnove, ki so vgrajene v modul `numpy`.

Za primer najprej definirajmo matriko in vektor:

```python
A = np.array([[4, -2],
              [-2, 4]])
b = np.array([1, 2])
```

Inverzno matriko izračunamo z uporabo funkcije `numpy.linalg.inv()` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.linalg.inv.html)):
```python
numpy.linalg.inv(a)
```


```python
np.linalg.inv(A)
```

Sistem linearnih enačb, ki ga definirata matrika koeficientov `a` in vektor konstant `b`, rešimo s pomočjo funkcije `numpy.linalg.solve()` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.linalg.solve.html)):
```python
numpy.linalg.solve(a, b)
```

Primer:

```python
rešitev = np.linalg.solve(A, b)
rešitev
```

Enakost elementov numeričnega polja `a` in `b` (znotraj določene tolerance) preverimo s funkcijo `numpy.isclose()` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.isclose.html)):
```python
numpy.isclose(a, b, rtol=1e-05, atol=1e-08, equal_nan=False)
```

Primer:

```python
np.isclose(np.dot(A, rešitev), b)
```

### Vektorizacija algoritmov

V tem poglavju želimo izpostaviti vektorizacijo algoritmov. Glede na to kako Python in `numpy` delujeta se je potrebno izogibati zankam. Bistveno hitreje lahko izvajamo izračune, če jih uspemo vektorizirati; to pomeni, da izračune izvajamo na nivoju vektorjev (oz. numeričnih polj) in ne elementov.

Za primer si najprej pripravimo podatke (dva vektorja dolžine 1000)

```python
N = 1000
a = np.arange(N)
b = np.arange(N)
```

Izračunajmo skalarni produkt vektorjev z uporabo zanke `for`:

```python
c = 0
for i in range(N):
    c += a[i] * b[i]
c
```

Izmerimo hitrost:

```python
%%timeit -n100
c = 0
for i in range(N):
    c += a[i] * b[i]
```

Isti rezultat pridobimo še v vektorski obliki:

```python
%%timeit -n100
c = a @ b
```

Vidimo, da je vektorski način bistveno hitrejši (za še hitrejši način glejte `numba` v dodatku)!

## Osnove modula ``matplotlib``

V Pythonu imamo več možnosti za prikaz rezultatov v grafični obliki. Najbolj uporabni paketi so:

* [`matplotlib`](http://matplotlib.org/) za visoko kakovostne, visoko prilagodljive slike (relativno počasno),
* [`pyqtgraph`](http://www.pyqtgraph.org/) za kakovostne in prilagodljive uporabniške vmesnike (zelo hitro),
* [`bokeh`](https://bokeh.pydata.org/en/latest/) za interaktiven prikaz v brskalniku (relativno hitro).

Obstaja še veliko drugih; dober pregled je naredil Jake VanderPlas na konferenci [PyCon 2017](https://www.youtube.com/watch?v=FytuB8nFHPQ) (sicer avtor paketa za deklerativno vizualizacijo: *Altair*).

### Osnovna uporaba

Najbolj razširjen in najbolj splošno uporabljen je paket [`matplotlib`](http://matplotlib.org/):
![Matplotlib](matplotlib.org/_static/logo2.svg)

Sposobnosti paketa najbolje prikazuje [galerija](https://matplotlib.org/stable/gallery/index.html). Gre za zelo sofisticiran paket in tukaj si bomo na podlagi primerov pogledali nekatere osnove.

Pri uporabi vam lahko koristi [plonk listek](https://github.com/rougier/matplotlib-cheatsheet):
![Matplotlib cheatsheet](github.com/rougier/matplotlib-cheatsheet/blob/master/matplotlib-cheatsheet.png?raw=true)

Tipično uvozimo `matplotlib.pyplot` kot `plt`:

```python
import matplotlib.pyplot as plt
```

Znotraj *Jupyter notebooka* obstajata dva načina prikaza slike (v oglatem oklepaju je magic ukaz za proženje):

1. `[%matplotlib inline]   `: slike so vključene v notebook (**medvrstični** način),
1. `[%matplotlib widget]   `: slike so interaktivno vključene v notebook (**medvrstični interaktivni** način), zahteva namestitev paketa **ipympl**,

3. `[%matplotlib notebook] `: slike so interaktivno vključene v notebook (**medvrstični interaktivni** način).

Opomba: *interaktivni način se v pasivni, spletni/pdf verziji te knjige ne prikaže pravilno*.

Tukaj bomo najpogosteje uporabljali *medvrstični* način:

```python
%matplotlib inline
```

Kratek primer:

```python
t = np.linspace(1, 130, 44000)
žvižg = np.sin(t**2)
plt.plot(t, žvižg, label='Žvižg')
plt.xlim(1, 10)
plt.title('Žvižg: $t^2$ (podpora za LaTeX: $\\sqrt{\\frac{a}{b}}$)')
plt.legend();
plt.show()
```

Mimogrede, zakaj to imenujemo žvižg (oz. kvadraten žvižg)? Da dobimo odgovor, podatke predvajamo na zvočnik:

```python
from IPython.display import Audio, display
display(Audio(data=žvižg, rate=44000))
```

Aktivirajmo sedaj interaktivni način (glejte tudi ``%matplotlib``):

```python
%matplotlib notebook
%matplotlib notebook
```

```python
plt.plot([1,2,3], [2,4,5]);
```

Zgornjo sliko lahko sedaj interaktivno klikamo in tudi dopolnjujemo s kodo:

```python
plt.title('Pozneje dodani naslov!')
plt.xlabel('Čas [$t$]');
```

### Interaktivna uporaba

Pri tem predmetu bomo večkrat uporabljali interaktivnost pri delu z grafičnimi prikazi. V ta namen najprej uvozimo `interact` iz paketa `ipywidgets`:

```python
from ipywidgets import interact
```

Potem definiramo sliko kot funkcijo z argumenti `amplituda`, `fr`, `faza` in `dušenje`:

```python
def slika(amplituda=1, fr=10, faza=0, dušenje=0.):
    t = np.linspace(0, 1, 200)
    f = amplituda * np.sin(2*np.pi*fr*t - faza) * np.exp(-dušenje*2*np.pi*fr*t)
    plt.plot(t, f)
    plt.ylim(-5, 5)
    plt.show()
```

Gremo nazaj na medvrstično uporabo in kličemo funkcijo za izris slike (privzeti argumenti):

```python
%matplotlib inline
slika()
```

Če funkcijo za izris slike pošljemo v funkcijo `interact`, slednja poskrbi za interaktivne gumbe, s katerimi lahko spreminajmo parametre klicanja funkcije `slika`:

```python
interact(slika);
```

Razpon parametov lahko tudi sami definiramo:

```python
interact(slika, amplituda=(1, 5, 1), dušenje=(0, 1, 0.05), fr=(10, 100, 1), faza=(0, 2*np.pi, np.pi/180));
```

### Napredna uporaba

#### Prikaz več funkcij

Poglejmo si preprost primer prikaza več funkcij (najprej, opcijsko, definirajmo fonte za izpis v pdf):

```python
from matplotlib import rc
rc('font',**{'family':'serif'})
plt.rcParams['pdf.fonttype'] = 42
```

```python
x = np.linspace(0, 10, 100)

y1 = np.sin(x)
y2 = np.sin(x+1)
y3 = np.sin(x**1.2)

plt.plot(x, y1, '-', label='$\sin(x)$ - to je LaTeX izpis', linewidth = 2);
plt.plot(x, y2, '.', label='sin(x+1) - to ni', linewidth = 2);
plt.plot(x, y3, '--', label='$\sin(x^{1.2})$ - to spet je: čšž', linewidth = 2);
plt.legend(loc=(1.01,0)); 
plt.savefig('data/prvi plot.pdf')
```

#### Prikaz več slik

Več slik prikažemo s pomočjo metode `subplot`, ki definira mrežo in lego naslednjega izrisala.
```python
plt.subplot(2, 2, 1)
plt.plot(x, y1, 'r')
plt.subplot(2, 2, 2)
plt.plot(x, y2, 'g')
```
V primeru zgoraj `plt.subplot(2, 2, 1)` pomeni: mreža naj bo `2 x 2`, riši v sliko 1 (levo zgoraj). Naslednjič se kliče `plt.subplot(2, 2, 2)` kar pomeni mreža `2 x 2`, riši v sliko 2 (desno zgoraj) in tako naprej. Opazimo, da se indeks slik začne z 1; lahko bi rekli, da gre za nekonsistentnost v Pythonu, razlog pa je v tem, da je bil `matplotlib` rojen v ideji, da bi uporabnikom Pythona uporabil čimbolj podoben način izrisa, kakor so ga poznali v Matlabu (kjer se indeks začne z 1).

Delujoči primer je:

```python
plt.subplot(2, 2, 1)
plt.plot(x, y1, 'r')
plt.subplot(2, 2, 2)
plt.plot(x, y2, 'g')
plt.subplot(2, 2, 3)
plt.plot(x, y2*y3, 'b')
plt.subplot(2, 2, 4)
plt.plot(x, y2+y3, 'k', linewidth=5);
plt.grid()
```

#### Histogram

Generirajmo 10000 normalno porazdeljenih vzorcev in jih prikažimo v obliki histograma:

```python
np.random.seed(0)
x = np.random.normal(size=10000)
plt.hist(x);
```

### Uporaba primerov iz ``matplotlib.org``

Primere iz [galerije](https://matplotlib.org/stable/gallery/index.html) lahko uvozimo s pomočjo magične funkcije `%load`.

Poskusite:

* `%load http://matplotlib.org/mpl_examples/lines_bars_and_markers/fill_demo.py`
* `%load http://matplotlib.org/examples/widgets/slider_demo.py`

# Dodatno: 
## `Numba`

Paket `numba` se v zadnjem obdobju zelo razvija in lahko numerično izvajanje še dodatno pohitri (tudi v povezavi z grafičnimi karticami oz. GPU procesorji). 

Za zgled tukaj uporabimo `jit` ([just-in-time compilation](http://numba.pydata.org/numba-doc/dev/reference/jit-compilation.html)) iz paketa `numba`:

```python
import numpy as np
from numba import jit
```

`jit` uporabimo kot [dekorator](https://docs.python.org/3/glossary.html#term-decorator) funkcije, ki jo želimo pohitriti:

```python
@jit
def skalarni_produkt(a, b):
    c = 0
    for i in range(N):
        c += a[i] * b[i]
    return c
```

Sedaj definirajmo vektorja:

```python
N = 1000
a = np.arange(N)
b = np.arange(N)
```

Preverimo hitrost `numpy` skalarnega produkta:

```python
%%timeit -n1000
a @ b
```

Preverimo še hitrost `numba` pohitrene verzije. Prvi klic izvedemo, da se izvede kompilacija, potem merimo čas:

```python
skalarni_produkt(a, b)
```

```python
%%timeit -n1000
skalarni_produkt(a, b)
```

Vidimo, da smo še izboljšali hitrost!

## `matplotlib`: animacije, povratni klic, XKCD stil

Z `matplotlib` lahko pripravimo tudi **animacije**. Dva primera lahko najdete tukaj:

* [Preprosta animacija](./moduli/matplotlib_animacija.py),
* [Dvojno nihalo](./moduli/animacija_dvojno_nihalo.py).

Več v [dokumentaciji](https://matplotlib.org/api/animation_api.html#animation).

Slika s **povratnim klicem** (angl. *call back*):

* [Risanje črte](./moduli/matplotlib_klik.py).

Pripravite lahko tudi *na roko* narisane slike (**XKCD stil**):

* [XKCD stil](./moduli/xkcd_stil.py).

## Za najbolj zagrete

1. Naučite se še kaj novega na [chrisalbon.com](http://chrisalbon.com/).

---

# Vprašanja za vaje

---

## Modul ``numpy``

**Vprašanje 1:** Uvozite modul ``numpy`` s krajšim imenom, ``np``.

Poljuben seznam števil pretvorite v ``numpy`` numerično polje, prikažite vpliv argumenta ``dtype``.

**Vprašanje 2:** z uporabo funkcij ``numpy.arange`` in ``numpy.linspace`` pripravite numerično polje celih števil, večjih ali enakih 0 in manjših od 10.

**Vprašanje 3:** Z uporabo funkcije modula ``numpy`` pripravite matriko (dvodimenzionalno numerično polje) ničel, dimenzij $5 \times 6$, in jo shranite v spremenljivko ``A``. 

Z uporabo ustreznega  ``numpy`` ukaza izpišite število vrstic in število stolpcev matrike ``A``.

**Vprašanje 4:** Prvi stolpec pripravljene matrike ``A`` zapolnite z vrednosti iz poljubnega numeričnega polja ustrezne dolžine.

**Vprašanje 5:** Iz pripravljene matrike ``A`` z rezanjem numeričnih polj pripravite kvadratno matriko ``B``, tako, da izpustite prvi stolpec.

**Vprašanje 6:** V matriki ``A`` prvo vrstico zamenjajte z enicami. Izpišite matriko ``B`` in komentirajte.

### Operacije nad numeričnimi polji

**Vprašanje 7:** Pripravite numerično polje z vrednostmi maksimalnih elementov vsake od vrstic pripravljene matrike A.

**Vprašanje 8:** Na pripravljenem numeričnem polju opazujte uporabo osnovnih matematičnih operacij nad numeričnimi polji (``*, /, //, sin, cos, sqrt``...). 

Komentirajte razliko v primerjavi s seznami!

### Linearna algebra z ``numpy``

**Vprašanje 9:** Pripravite enosko matriko ``I``. Pomnožite jo s poljubnim vektorjem ustrezne dolžine.

**Vprašanje 10:** Izračunajte inverz matrike ``M`` in pravilnost rezultata preverite z  operacijo množenja matrik.

```python
# Podatek
M = np.array([[1, 5, 5], [2,1,3], [7.2, 7, 1.2]])
```

**Vprašanje 11:** Z uporabo ``numpy`` orodij rešite sistem linearnih enačb ``Mx = b``.

```python
# Podatek
M = np.array([[1, 5, 5], [2,1,3], [7.2, 7, 1.2]])
b = np.array([1,2,3])
```

-------

## Modul ``matplotlib``

```python
from matplotlib import pyplot as plt
```

**Vprašanje 12:** Z uporabo ``matplotlib.pyplot`` na istem grafu izrišite spodaj definiranje krivulje. Prikažite uporabo različnih tipov in barv črt.

```python
# Podatek
t = np.linspace(1,10,300)
x = 2*np.sin(t+2) -1
y = 2*np.sin(t)
z = 1.5*np.cos(t**2)
```

**Vprašanje 13:** Na zgoraj izrisanem grafu prikažite uporabo legende, naslova in oznak koordinatnih osi, ter izrišite koordinatno mrežo.
