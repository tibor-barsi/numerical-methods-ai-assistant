# Aproksimacija

## Uvod

V strojniški praksi se pogosto srečamo s tabelo podatkov, ki so lahko obremenjeni z merilnimi ali numeričnimi napakami.

Oglejmo si primer meritve (linearne) vzmeti ($x$ je raztezek, $y$ je pomerjena sila):

```python
import numpy as np

x = np.array([0.1,  1.1, 2.05, 3.2, 3.9  ])
y = np.array([0.22, 18.15, 44.33, 75.59, 105.63])
```

Poglejmo si podatke na sliki, najprej uvozimo potrebne pakete:

```python
import matplotlib.pyplot as plt
%matplotlib inline
plt.plot(x, y, 'o');
plt.xlabel('Pomik $x$')
plt.ylabel('Sila $y$');
```

Za konkreten primer bi bilo, glede na poznavanje fizikalnega ozadja linearne vzmeti, primerno, da bi meritve poskušali popisati z linearno funkcijo:

$$f(x) = a_0\,x+ a_1$$

Poznamo tabelo $n$ podatkov $x_i, y_i$ za $i=0,1,\dots,n-1$; teh je več, kot jih potrebujemo za določitev dveh konstant $a_0$ in $a_1$, zato imamo torej predoločen sistem linearnih enačb: 

$$y_i=a_0\,x_i+a_1\quad\textrm{za}\quad i=0,1,\dots,n-1.$$

Iščemo taki vrednosti konstanti $a_0$ in $a_1$, da se bo funkcija $f(x)$ v znanih točkah $x_i$ *najbolje* ujemala z $y_i$.

Najprej torej potrebujemo kriterij za *najboljše* ujemanje.

Za vrednosti iz tabele $x_i, y_i$ bi lahko iskali vrednosti $a_0$ in $a_1$, pri katerih bi bila vsota absolutne vrednosti odstopkov $S$ najmanjša:

$$P(a_0, a_1) = \sum_{i=0}^{n-1} |y_i - (a_0\,x_i+a_1)|.$$

Ker pa taka funkcija $P(a_0, a_1)$ ni zvezno odvedljiva,raje uporabimo **metodo najmanjših kvadratov**:

$$S(a_0, a_1) = \sum_{i=0}^{n-1} \left(y_i - (a_0\,x_i+a_1)\right)^2.$$

Takšna funkcija $S(a_0, a_1)$ je zvezna in zvezno odvedljiva. S parcialnim odvajanjem po parametrih $a_0$ in $a_1$ lahko najdemo stacionarno točko (parcialna odvoda sta enaka 0). Postopek si bomo za linearno funkcijo pogledali v naslednjem poglavju.

## Metoda najmanjših kvadratov za linearno funkcijo

```python
from IPython.display import YouTubeVideo
YouTubeVideo('8-oixY6GG9E', width=800, height=300)
```

Poiskati moramo konstanti $a_0$, $a_1$, da bo vsota kvadratov razlik med funkcijo in tabelirano vrednostjo ($x_i, y_i$, kjer $i=0,1,\dots, n-1$ in je $n$ število tabeliranih podatkov):

$$S(a_0, a_1) = \sum_{i=0}^{n-1} \left(y_i - (a_0\,x_i+a_1)\right)^2$$

najmanjša. Vrednost bo najmanjša v stacionarni točki, ki jo določimo s parcialnim odvodom po parametrih $a_0$ in $a_1$.

Najprej izvedemo parcialni odvod po parametru $a_0$:

$$\frac{\partial S(a_0, a_1)}{\partial a_0} = 2\,\sum_{i=0}^{n-1} \left(y_i - a_0\,x_i-a_1\right)\,(-x_i)$$

Izraz uredimo:

$$\frac{\partial S(a_0, a_1)}{\partial a_0} =-2\left(\sum_{i=0}^{n-1} y_i\,x_i -a_0\,\sum_{i=0}^{n-1} x_i^2- a_1\,\sum_{i=0}^{n-1} x_i\right)$$

Podobno postopamo še za $a_1$:

$$\frac{\partial S(a_0, a_1)}{\partial a_1} = 2\,\sum_{i=0}^{n-1} \left(y_i  - a_0\,x_i- a_1\right)\,(-1)$$

$$\frac{\partial S(a_0, a_1)}{\partial a_1} = -2\left(\sum_{i=0}^{n-1} y_i -a_0\,\sum_{i=0}^{n-1} x_i- a_1\,\sum_{i=0}^{n-1}1\right)$$

Ker v stacionarni točki velja $\partial S(a_0, a_1)/\partial a_0=0$ in $\partial S(a_0, a_1)/\partial a_1=0$, iz zgornjih izrazov izpeljemo:

$$a_0\,\sum_{i=0}^{n-1} x_i^2 + a_1\,\sum_{i=0}^{n-1} x_i=\sum_{i=0}^{n-1} y_i\,x_i$$

in

$$a_0\,\sum_{i=0}^{n-1} x_i+ a_1\,n=\sum_{i=0}^{n-1} y_i.$$

Dobili smo sistem dveh linearnih enačb za neznanki $a_0$ in $a_1$, ki ga znamo rešiti. Imenujemo ga *normalni sistem* (število enačb je enako številu neznank).

Zapišimo normalni sistem v matrični obliki:

```python
A = [[np.sum(x**2), np.sum(x)], # matrika koeficientov
     [np.sum(x), len(x)]]
b = [np.dot(y,x), np.sum(y)]    # vektor konstant
A = np.asarray(A)
b = np.asarray(b)
print('A:', A)
print('b:', b)
```

Sedaj moramo rešiti linearni sistem:

$$\mathbf{A}\,\mathbf{a}=\mathbf{b},$$

Opomba, tukaj smo vektor neznank zapisali kot $\mathbf{a}=(a_0, a_1)$.

Sistem rešimo:

```python
a0, a1 = np.linalg.solve(A, b)
a0, a1
```

Preverimo še število pogojenosti:

```python
np.linalg.cond(A)
```

Sedaj si bomo pogledali še rezultat. Najprej pripravimo sliko, ki bo vsebovala tudi informacijo o vsoti kvadratov odstopanja $f(x_i)$ od tabeliranih vrednosti $y_i$.

```python
def slika(naklon=a0, premik=a1):
    d=1
    def linearna_f(x, a0, a1):
        return a0*x+a1
    def S(x, y, f):
        return np.sum(np.power(y-f,2))
    plt.plot(x,y,'.', label='Tabela podatkov')
    linearna_f1 = linearna_f(x, naklon, premik)
    linearna_f1_MNK = linearna_f(x, a0, a1)
    plt.plot(x, linearna_f1, '-', label='Izbrani parametri')
    plt.plot(x, linearna_f1_MNK, '-', label='Metoda najmanjših kvadratov')
    napaka = S(x, y, linearna_f(x, naklon, premik))
    sprememba_napake_v_smeri_a0 = (S(x, y, linearna_f(x, naklon+d, premik))-napaka)/d
    sprememba_napake_v_smeri_a1 = (S(x, y, linearna_f(x, naklon, premik+d))-napaka)/d
    title = f'S: {napaka:g}, \
            $\\Delta S/\\Delta a_0$: {sprememba_napake_v_smeri_a0:g}, \
            $\\Delta S/\\Delta a_1$: {sprememba_napake_v_smeri_a1:g}'
    plt.title(title)
    plt.legend()
    plt.ylim(-10,110)
    plt.show()
```

```python
from ipywidgets import interact
interact(slika, naklon=(0, 50, 2), premik=(-10, 10, 1));
```

### Uporaba psevdo inverzne matrike

Do podobnega rezultata lahko pridemo z uporabo psevdo inverzne matrike. Iščemo $y(x)=a_0\,x+a_1$ in nastavimo predoločen sistem $\mathbf{A}\,\mathbf{a}=\mathbf{y}$, kjer je matrika koeficientov $\mathbf{A}$ definirana glede na vrednosti $x_i$ ($i=0,1,\dots,n-1$):

```python
A=np.array([x, np.ones_like(x)]).T
A
```

Vektor konstant smo označili z $\mathbf{b}$, v našem primeru pa je to kar vektor vrednosti $\mathbf{y}$ z elementi $y_i$ ($i=0,1,2,\dots,n-1$):

```python
y
```

Preverimo rang matrike koeficientov:

```python
np.linalg.matrix_rank(A)
```

In še rang razširjene matrike:

```python
Ab = np.hstack((A,np.array([y]).T))
Ab
```

```python
np.linalg.matrix_rank(Ab)
```

Ker rešujemo sistem $m$ linearnih enačbami z $n$ neznankami ter velja $m>n$ in je rang razširjene matrike $n+1$, imamo predoločeni sistem.

Vektor konstant $\mathbf{a}$ določimo z uporabo psevdo inverzne matrike:

$$\mathbf{a}=\mathbf{A}^{+}\,\mathbf{y}$$

```python
np.linalg.pinv(A).dot(y)
```

## Metoda najmanjših kvadratov za poljubni polinom

Linearno aproksimacijo, predstavljeno zgoraj, bomo posplošili za poljubni polinom stopnje $m$:

$$f(a_0,a_1,\dots,a_{m}, x) = \sum_{s=0}^{m}a_s\,\underbrace{x^{m-s}}_{f_s(x)},$$

kjer $f_s(x)=x^{m-s}$ imenujemo bazna funkcija ($s=0,1,2,\dots,m$).

Tabela podatkov naj bo definirana z $x_i, y_i$, kjer je $i=0,1,2,\dots,n-1$. 

Opomba: zaradi kompaktnosti zapisa bomo konstante $a$ zapisali v vektorski obliki $\mathbf{a}=[a_0, a_1,\dots,a_m]$.

Uporabimo metodo najmanjših kvadratov:

$$S(\mathbf{a}) = \sum_{i=0}^{n-1} \left(y_i - f(\mathbf{a}, x_i)\right)^2= \sum_{i=0}^{n-1} \left(y_i - \sum_{s=0}^{m}a_s\,x_i^{m-s}\right)^2.$$

Potreben pogoj za nastop ekstrema funkcije $m+1$ neodvisnih spremenljivk je, da najdemo stacionarno točko za vsak $a_v$, iščemo torej $\partial S(\mathbf{a})/\partial a_v=0$ (namesto $s$ smo uporabili indeks $v$).

Najprej določimo parcialni odvod za izbrani $a_v$:

$$\frac{\partial S(\mathbf{a})}{\partial a_v} = \sum_{i=0}^{n-1} - 2\,\left(y_i - \sum_{s=0}^{m}a_s\,x_i^{m-s}\right)\,x_i^{m-v}$$

Opomba: $\frac{\partial}{\partial a_v}\left(\sum_{s=0}^{m}a_s\,x_i^{m-s}\right)=x_i^{m-v}$.

Ker je parcialni odvod v stacionarni točki enak 0, zgornji izraz preoblikujemo:

$$\sum_{i=0}^{n-1} \left(\sum_{s=0}^{m}a_s\,x_i^{m-s}\right)\,x_i^{m-v}=\sum_{i=0}^{n-1} y_i\,x_i^{m-v}$$

Izraz uredimo:

$$\sum_{i=0}^{n-1} \sum_{s=0}^m a_s\,x_i^{2m-s-v}=\sum_{i=0}^{n-1} y_i\,x_i^{m-v}$$

Zamenjamo vrstni red seštevanja ter izpeljemo:

$$\sum_{s=0}^m \left(a_s \sum_{i=0}^{n-1} \,x_i^{2m-s-v}\right)=\sum_{i=0}^{n-1} y_i\,x_i^{m-v}\quad\textrm{za:}\quad v=0,1,\dots,m$$

Izpeljali smo enačbo $v$ sistema $m+1$ linearnih enačb:

$$\mathbf{A}\,{\mathbf{a}}=\mathbf{b}$$

Element $A_{v,s}$ matrike koeficientov je:

$$A_{v,s}= \sum_{i=0}^{n-1} x_i^{2m-v-s},$$

Element vektorja konstant je:

$$b_{v}= \sum_{i=0}^{n-1} y_i\,x_i^{m-v}$$

### Numerični zgled

Uporabimo podatke iz prve naloge in poskusimo aproksimirati s polinomom 2. stopnje ($m=2$). 

Tabela podatkov je:

```python
x
```

```python
y
```

Izračunajmo matriko koeficientov:

$$A_{v,s}= \sum_{i=0}^{n-1} x_i^{2m-v-s}$$

```python
m = 2 #stopnja
A = np.zeros((m+1,m+1))
for v in range(m+1):
    for s in range(m+1):
        A[v,s] = np.sum(x**(2*m-v-s))
A
```

Izračunajmo še vektor konstant:

$$b_{v}= \sum_{i=0}^{n-1} y_i\,x_i^{m-v}$$

```python
b = np.zeros(m+1)
for v in range(m+1):
    b[v] = np.dot(y,x**(m-v))
b
```

Preverimo število pogojenosti:

```python
np.linalg.cond(A)
```

Rešimo sistem:

```python
a = np.linalg.solve(A, b)
a
```

Glede na definicijo aproksimacijskega polinoma:

$$f(a_0,a_1,\dots,a_{m}, x) = \sum_{v=0}^{m}a_v\,x^{m-v}$$

Kar v konkretnem primeru je aproksimacijski polinom:

$$f(x)=3.18393375\,x^2 + 14.64106847\,x -1.22621065$$

Definirajmo numerično implementacijo:

```python
def apr_polinom(x, a):
    """Vrne vrednosti aproksimacijskega polinoma

    :param x: vrednosti kjer računamo aproksimirani rezultat
    :param a: koeficienti aproksimacijskega polinoma
    """
    m = len(a) - 1
    return np.sum(np.asarray([_*x**(m-v) for v,_ in enumerate(a)]), axis=0)
```

Prikažemo:

```python
x_g = np.linspace(np.min(x), np.max(x), 100) # več točk za prikaz
plt.plot(x, y, 'o', label='Tabela podatkov')
plt.plot(x_g, apr_polinom(x_g, a), lw=5, alpha=0.5, label='Aproksimacija s kvadratno funkcijo')
plt.legend();
```

Poglejmo še napako aproksimacije:
$$e_i=y_i - f(x_i)$$

za $i=0,1,2,\dots,n-1$.

Pri pravilno izvedni aproksimaciji je nekaj $e_i$ pozitivnih in nekaj negativnih. Poglejmo, če je to res v našem primeru:

```python
e = y - apr_polinom(x, a)
e
```

Opomba: višje stopnje polinoma kot uporabimo, večja je verjetnost slabe pogojenosti. Iz tega razloga s stopnjo polinoma ne pretiravamo (v praksi uporabljamo predvsem nizke stopnje)!

### Uporaba `numpy` za aproksimacjo s polinomom

Poglejmo si, kako uporabimo knjižnico `numpy` za polinomsko aproksimacijo. 

Najprej uporabimo funkcijo `numpy.polyfit` ([dokumentacija](https://numpy.org/doc/stable/reference/generated/numpy.polyfit.html)):

```python
polyfit(x, y, deg, rcond=None, full=False, w=None, cov=False)
```

ki zahteva tri parametre: `x` in `y` predstavljata tabelo podatkov (lahko tudi v obliki seznamov vektorjev), `deg` pa stopnjo polinoma. Ostali parametri so opcijski (npr. `w` za uporabo uteži pri aproksimaciji). 

Funkcija `polyfit` vrne seznam koeficientov polinoma (najprej za najvišji red); rezultat je lahko tudi seznam seznamov (če so vhodni podatki seznam vektorjev).

Poglejmo si uporabo za predhodno obravnavani primer:

```python
koef = np.polyfit(x, y, deg=2)
koef
```

```python
a # rezultat lastne implementacije
```

Ko imamo koeficiente, lahko ustvarimo objekt polinoma s klicem ``numpy.poly1d`` ([dokumentacija](https://numpy.org/doc/stable/reference/generated/numpy.poly1d.html#numpy.poly1d)):

```python
poly1d(c_or_r, r=False, variable=None)
```

kjer `c_or_r` predstavlja seznam koeficientov polinoma oz. ničle polinoma v primeru, da je `r=True`. Funkcija vrne instanco objekta, s klicem katere lahko izračunamo vrednosti aproksimacijskega polinoma pri `x`, lahko pa izračunamo tudi druge stvari, kot na primer ničle polinoma.

Poglejmo si primer:

```python
p = np.poly1d(koef) # p je instanca objekta poly1d. Aproksimacijo sedaj dobimo z y = p(x).
```

Izrišimo vrednosti:

```python
plt.plot(x, y, 'o', label='Tabela podatkov')
plt.plot(x_g, p(x_g), lw=5, alpha=0.5, label='Aproksimacija s kvadratno funkcijo')
plt.legend();
```

Izračunajmo ničle polinoma:

```python
p.roots
```

## Aproksimacija s poljubno funkcijo

Pri aproksimaciji nismo omejeni zgolj na polinome. Tabele podatkov lahko aproksimiramo:

* z linearno kombinacijo linearno neodvisnih baznih funkcij ali 
* s funkcijo, v kateri nastopajo parametri v nelinearni zvezi (npr. $a_0\,\sin(a_1\,x+a_2)$).

Za podrobnosti glejte vir J. Petrišič: Uvod v Matlab za inženirje, Fakulteta za strojništvo 2013, str 145. 

Osredotočili se bomo na uporabo `scipy` paketa za aproksimacijo z nelinearno fukcijo, ki temelji na metodi najmanjših kvadratov.

### Aproksimacija s harmonsko funkcijo

Tabela podatkov je definirana kot:

```python
x = np.array([ 0.1, 0.8, 1.7, 2.5, 3.4, 4.2,  5.1])
y = np.array([ 0.01, -1.13, 0.02, 0.92, -0.01, -0.98, 0.1])
```

Prikažimo tabelo podatkov:

```python
plt.plot(x, y, 'o', label='Tabela podatkov')
plt.legend();
```

Aproksimacijo z nelinearno funkcijo bomo izvedli s pomočjo ``scipy.optimize.curve_fit`` ([dokumentacija](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.curve_fit.html)):

```python
curve_fit(f, xdata, ydata, p0=None, sigma=None, absolute_sigma=False, check_finite=True, bounds=(-inf, inf), method=None, jac=None, **kwargs)
```


katera zahteva tri parametre: `f` predstavlja definicijo Python funkcije, s katero želimo aproksimirati, in katere parametre spreminjamo z uporabo metode najmanjših kvadratov. `xdata` in `ydata` predstavljata tabelo podatkov. Priporočeno je tudi, da definiramo približek iskanih parametrov `p0`. Ostali parametri so opcijski.

Funkcija vrne dve numerični polji: `popt`, ki predstavlja najdene parametre ter `pcov`, ki predstavljajo ocenjeno kovarianco `popt`.

Definirajmo najprej Python funkcijo, katere prvi parameter je neodvisna spremenljivka `x`, nato pa sledijo parametri, ki jih želimo določiti:

```python
def func(x, A, ω, ϕ):
    return A*np.sin(ω*x+ϕ)
```

kjer je `A` amplitua, `ω` krožna frekvenca in `ϕ` faza harmonske funkcije. S pomočjo slike lahko ugibamo prve približke: `A=1`, `ω=1`, `ϕ=0`

Sedaj uvozimo `curve_fit` in izvedemo optimizacijski postopek:

```python
from scipy.optimize import curve_fit
```

```python
popt, pcov = curve_fit(func, x, y, p0=[1, 1, 0])
popt
```

Izračunali smo pričakovane vrednosti (glejte zgoraj).

```python
func(x, *popt)
```

```python
x_g = np.linspace(np.min(x), np.max(x), 50)
y_g = func(x_g, *popt) # bodi pozorni kako smo v funkcijo posredovali parametre
plt.plot(x, y, 'o', label='Tabela podatkov')
plt.plot(x_g, y_g, label='Aproksimacija')
plt.legend();
```

# Dodatno

Naredite *.exe* svojega programa:

* https://pypi.org/project/py2exe/
* http://www.pyinstaller.org/

Poglejte [pandas paket](http://pandas.pydata.org/).
![Pandas](https://pandas.pydata.org/static/img/pandas.svg)

### Aproksimacija z zlepki in uporabo ``SciPy``

Tabela podatkov naj bo:

```python
x = np.linspace(-3, 3, 20)
x
```

```python
np.random.seed(0) # seme generatorja naključnih števil
y = np.exp(-x**2) + 0.1 * np.random.normal(scale=.5, size=len(x))
y
```

Poglejmo si objekt ``scipy.interpolate.UnivariateSpline`` ([dokumentacija](https://docs.scipy.org/doc/scipy/reference/generated/scipy.interpolate.UnivariateSpline.html)), ki omogoča tako interpolacijo kot aproksimacijo z zlepki:

```python
UnivariateSpline(x, y, w=None, bbox=[None, None], k=3, s=None, ext=0, check_finite=False)
```

Parametra `x` in `y` predstavljata tabelo podatkov.

Opcijski parameter `s` določa vrednost, katere vsota kvadratov razlik aproksimacijskega zlepka in aproksimacijskih točk ne sme preseči:

```python
sum((w[i] * (y[i]-spl(x[i])))**2, axis=0) <= s
```

`w` so uteži posameznih točk.

Če definiramo `s=0`, zahtevamo interpolacijo. 

Parameter `k` definira stopnjo polinomskega zlepka (privzeto je `k=3`).

Aproksimacijo z zlepki izvedemo tako, da ob tabeli podatkov `x` in `y` definiramo še parameter `s`.  Izvedimo interpolacijo:

```python
from scipy.interpolate import UnivariateSpline
spl = UnivariateSpline(x, y, s=0.)
```

```python
x_g = np.linspace(-3, 3, 100)
plt.plot(x, y, 'o', ms=5, label='Aproksimacijske točke')
plt.plot(x_g, spl(x_g), lw=3, label='Zlepek', alpha=0.6)
plt.legend();
```

Izvedimo še aproksimacijo:

```python
spl_a = UnivariateSpline(x, y, s=.1)
```

```python
plt.plot(x, y, 'o', ms=5, label='Aproksimacijske točke')
plt.plot(x_g, spl(x_g), lw=3, label='Interpolacija (s=0)');
plt.plot(x_g, spl_a(x_g), lw=3, label='Aproksimacija (s=2)');
plt.plot(spl_a.get_knots(), spl_a(spl_a.get_knots()), 'o', label='Vozli pri aproksimaciji');
plt.legend();
```

Dejanski preostanek:

```python
spl_a.get_residual()
```

---

# Vprašanja za vaje

---

**Vprašanje 1:** Prihaja deževno vreme in zanima vas koeficient trenja med avtomobilsko gumo in mokro cesto. V garaži najdete odsluženo letno pnevmatiko, iz katere izrežete vzorec za testiranje. Nanj pritrdite silomer iz katerega odčitavete silo pri vlečenju gume po mokrem asfaltu. Da bi vaša meritev bila zanesljivejša, vzorec gume obtežujete z različnimi utežmi. 

<img src="fig/vaje/trenje.png" style="width:500px">

Veste, da je zveza med silo trenja ter normalno silo na podlago linearna:

$$F_t = \mu F_n$$

Na podlagi podanih meritev želite določiti vrednost koeficienta tranja $\mu$. Pomagate si z aproksimacijo (uporabite lahko poljubno metodo).

```python
# Podatki
F_n = np.array([5, 10, 15, 20, 25, 30]) # N
F_t = np.array([ 1.432,  2.365,  3.91 ,  5.166,  6.468,  7.438]) # N
```

## Metoda najmanjših kvadratov za linearno funkcijo

**Vprašanje 2:** Za podatke iz prejšnje naloge definirajte in rešite sistem linearnih enačb za izračun parametrov linearne aproksimacije. Rezultat prikažite grafično.

**Vprašanje 3:** Uporabite funkcijo ``numpy.polyfit`` in podane točke ``x, y`` aproksimirajte s polinomom prvega, drugega, tretjega in četrtega reda.

```python
# Podatki, ne briši!
x = np.linspace(0, 5, 100)
y = 0.5*x**4 + 3*x**3 - 20*x**2 + 3*x + 10*np.random.randn(len(x))
```

**Vprašanje 4:** Pripravite funkcijo ``napaki(y, y_apr)``, ki izračuna in vrne vsoto kvadratov razlik ter standardno napako (``numpy.std``) za numerični polji podanih vrednosti ``y`` ter rezultata aproksimacije ``y_apr``. 

Uporabite jo za primerjavo aproksimacijskih polinomov različnih stopenj iz prejšnje naloge.

**Vprašanje 5:** Da bi določili koeficient zračnega upora $C_D$ modelčka letala opravljate meritve v vetrovniku. Pri različnih hitrostih toka zraka večkrat pomerite silo zračnega upora $F_D$. Veste, da je odvisnost sile $F_D$ od hitrosti $v$ v vašem primeru podana z enačbo:

$$F_D = 0.005~C_D \cdot v^2$$

Na podlagi podanih vrednosti ``v, F_d`` določite vrednost koeficienta zračnega upora s pomočjo aproksimacije s polinomom druge stopnje.

```python
# Podatki
v = [6.184, 9.843, 15.582, 20.190, 23.509, 28.890, 34.753, 40.500, 45.979, 50.167]
F_d = [0.029, 0.013, 0.072, 0.054, 0.135, 0.204, 0.300, 0.420, 0.513, 0.643]
```

**Vprašanje 6:** Na istem grafu izrišite pomerjene točke, rezultat polinomske aproksimacije in rezultat izraza 

$$F_D = 0.005~C_D \cdot v^2$$

iz prejšnje naloge. 

Pojasnite zakaj prihaja do razlike med izrisanima krivuljama, kako to vpliva na določitev koeficienta $C_D$ in kako bi to napako odpravili (namig: izpišite dobljene koeficiente aproksimacijskega polinoma).

**Vprašanje 7:** Z uporabo ustrezne ``scipy`` funkcije določite koeficient zračnega upora $C_D$ tako, da pomerjene točke ``v, F_d`` aproksimirate s funkcijo:

$$F_D = 0.005~C_D \cdot v^2$$

Dobljeno vrednost primerjajte z rezultatom prejšnje naloge, rezultat grafično prikažite in komentirajte.

**Vprašanje 8:** Na počasnem posnetku smo opazovali odmik konice propelerja od horizontalne osi pri različnih časih. Na podlagi meritev želimo določiti hitrost vrtenja propelerja.

$$y = \sin(2\pi\cdot f~t + \varphi)$$

kjer je $f$ število obratov propelerja v sekundi.

Določite $f$ tako, da podatke aproksimirate s podano funkcijo in določite optimalne vrednosti parametrov $f$ in $\varphi$. Opazujte vpliv začetnih približkov ``p0``, ki jih opcijsko lahko podamo funkciji ``scipy.optimize.curve_fit(fun, x, y, p0)``.

```python
# Podatki
f = 50
phi = np.pi/4
t = np.linspace(0, 0.01, 100)
y = np.sin(2*np.pi*t*f + phi) + 0.1*np.random.randn(len(t))
```

## Aproksimacija z zlepki

**Vprašanje 9:** Podane vrednosti ``x, y`` aproksimirajte s kubičnimi zlepki z uporabo ``scipy.interpolate.UnivariateSpline``. 

Izrišite aproksimacijske krivulje pri treh podanih vrednostih parametra ``s``. Za vsako izpišete število vozlov zlepka (``spl.get_knots()``) in komentirajte dobljeno.

```python
# Podatki
x = np.linspace(-1, 1, 50)
y = 0.5*np.cos(1.7*np.pi*x) + 0.1*np.random.randn(len(x))
s = [0, 0.5, 2]
```
