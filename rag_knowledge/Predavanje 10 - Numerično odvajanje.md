# Numerično odvajanje

## Uvod

Vsako elementarno funkcijo lahko analitično odvajamo. Definicija odvoda je:

$$
f'(x)=\lim_{\Delta x \rightarrow 0}\frac{f(x+\Delta x)-f(x)}{\Delta x}.
$$

Neposredna uporaba zgornje enačbe vodi v odštevanje zelo podobnih funkcijskih vrednostih ($f(x+\Delta x)$, $f(x)$), obremenjenih z zaokrožitveno napako, ki jih delimo z majhno vrednostjo $\Delta x$; posledično ima odvod bistveno manj signifikantnih števk kakor pa funkcijske vrednosti. Numeričnemu odvajanju se izognemo, če imamo to možnost; je pa v nekaterih primerih (npr. reševanje diferencialnih enačb) nepogrešljivo orodje!

Pri numeričnem odvajanju imamo dva, v principu različna, pristopa:

1. najprej izvedemo **interpolacijo/aproksimacijo**, nato pa na podlagi znanih interpolacijskih/aproksimacijskih funkcij izračunamo odvod (o tej temi smo že govorili pri interpolaciji oz. aproksimaciji) in 
2. računanje odvoda **neposredno iz vrednosti iz tabele**.

V okviru tega poglavja se bomo seznanili s tem, kako numerično izračunamo odvod funkcije $f(x)$; pri tem so vrednosti funkcije $f(x)$ podane tabelarično (pari $x_i$, $y_i$), kakor je prikazano na sliki:
![Odvajanje](./fig/odvajanje_tabela.png' width=400>
Najprej se bomo osredotočili na ekvidistantno, s korakom $h$, razporejene vrednosti $x_i$; vrednosti funkcije pa bodo $y_i=f(x_i)$.

Glede na zgornjo definicijo odvoda, bi prvi odvod (za mesto $i$) lahko zapisali:

$$
y_i'=\frac{y_{i+1}-y_{i}}{h},
$$
kjer je $h=x_{i+1}-x_{i}$. S preoblikovanjem enačbe:

$$
y_i'=-\frac{y_{i}}{h}+\frac{y_{i+1}}{h},
$$

lahko tudi rečemo, da za prvi odvod funkcije na mestu $i$, **utežimo** funkcijsko vrednost pri $i$ z $-1/h$ in funkcijsko vrednost pri $i+1$ z $+1/h$.

V nadaljevanju si bomo pogledali teoretično ozadje kako določimo ustrezne **uteži** za različne stopnje odvodov, katere možnosti pri tem imamo in kako to vpliva na red natančnosti.

## Aproksimacija prvega odvoda po metodi končnih razlik

Za uvod si oglejmo spodnji video:

```python
from IPython.display import YouTubeVideo
YouTubeVideo('YYuGL-VP2BE', width=800, height=300)
```

Odvod $f'(x)$ lahko aproksimiramo na podlagi razvoja Taylorjeve vrste. To metodo imenujemo **metoda končnih razlik** ali tudi **diferenčna metoda**.

Razvijmo **Taylorjevo vrsto naprej** (naprej, zaradi člena $+h$):

$$
f{\left (x + h \right )} =\sum_{n=0}^{\infty}\frac{h^n}{n!}\frac{d^n}{dx^n}f(x)= f{\left (x \right )} + h\, f'\left (x \right ) + \underbrace{\frac{h^2}{2}\,f''(x)+\cdots}_{\mathcal{O}\left(h^{2}\right)}
$$

Člen $\mathcal{O}\left(h^{2}\right)$ označuje napako drugega reda. Če iz enačbe izrazimo prvi odvod:

$$
f'{\left (x \right )}=\frac{1}{h}\left(f{\left (x + h \right )} - f{\left (x \right )}\right) - \underbrace{\frac{h}{2}\,f''(x)+\cdots}_{\mathcal{O}\left(h^{1}\right)}
$$

Ugotovimo, da lahko ocenimo prvi odvod v točki $x_i$ (to je: $f_o'(x_i)$) na podlagi dveh zaporednih funkcijskih vrednosti:

$$
f_o'(x_i)=\frac{1}{h}\left(y_{i+1}-y_i\right)
$$

in pri tem naredimo **napako metode**, ki je prvega reda $\mathcal{O}\left(h^{1}\right)$. 

Uporabili smo $y_i=f(x_i)$ (glejte sliko zgoraj).

Napaka je:

$$e=-\frac{h}{2}\,f''(\xi),$$

kjer je $\xi$ neznana vrednost na intervalu $[x_i, x_{i+1}]$ in smo zanemarili višje člene.

Velja torej izraz:

$$f'(x_i)=f_o'(x_i)+e$$

Sedaj si poglejmo, kako pridemo do istega rezultata s strojno izpeljavo; najprej uvozimo ``sympy``:

```python
import sympy as sym
sym.init_printing()
```

Definirajmo simbole:

```python
f = sym.Function('f')
x, h = sym.symbols('x, h')
```

Nato nadaljujemo z razvojem **Taylorjeve vrste naprej** (angl. *forward Taylor series*):

```python
f(x+h).series(h, n=2)
```

Člen $\mathcal{O}\left(h^{2}\right)$ vsebuje člene drugega in višjega reda. V zgornji enačbi je uporabljena začasna spremenljivko za odvajanje $\xi_1$; izvedmo odvajanje in vstavimo $\xi_1=x$:

```python
f(x+h).series(h, n=2).doit()
```

Zapišemo enačbo:

```python
enačba = sym.Eq(f(x+h), f(x+h).series(h, n=2).doit())
enačba
```

Rešimo jo za prvi odvod $f'(x)$:

```python
f1_naprej_točno = sym.solve(enačba, f(x).diff(x))[0]
f1_naprej_točno
```

V kolikor odvoda drugega in višjih redov ne upoštevamo, smo naredili torej napako:

```python
f1_naprej_O = f1_naprej_točno.expand().getO()
f1_naprej_O
```

Napaka $\mathcal{O}\left(h^{1}\right)$ je torej prvega reda in če ta člen zanemarimo, naredimo *napako metode* in dobimo oceno odvoda:

```python
f1_naprej_ocena = f1_naprej_točno.expand().removeO()
f1_naprej_ocena
```

Ugotovimo, da gre za isti izraz, kakor smo ga izpeljali zgoraj, torej je:

$$y_i'=\frac{1}{h}\left(-y_i+y_{i+1}\right).$$

Uteži torej so:

|    Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$   | $y_{i}$   |$y_{i+1}$|
|:----------:|:----------:|:----------:|
|$y_i'=\frac{1}{h}\cdot$| -1 | 1 |

## Centralna diferenčna shema

### Odvod $f'(x)$

Najprej si poglejmo razvoj **Taylorjeve vrste nazaj** (angl. *backward Taylor series*):

```python
f(x-h).series(h, n=3).doit()
```

Ugotovimo, da se pri razliki vrste naprej in nazaj odštevajo členi sodega reda; definirajmo:

```python
def razlika(n=3):
    return f(x+h).series(h, n=n).doit()-f(x-h).series(h, n=n).doit()
razlika(n=3)
```

Izvedemo sledeče korake:

1. Taylorjevo vrsto nazaj odštejemo od vrste naprej, sodi odvodi se odštejejo,
2. rešimo enačbo za prvi odvod,
3. določimo napako metode,
4. določimo oceno odvoda.

Izvedimo zgornje korake:

```python
f1_cent_točno = sym.solve(
           sym.Eq(f(x+h) - f(x-h), razlika(n=3)), # 1 korak
           f(x).diff(x))[0]                       # 2.korak
f1_cent_O = f1_cent_točno.expand().getO()         # 3.korak
f1_cent_ocena = f1_cent_točno.expand().removeO()  # 4.korak
```

Ocena 1. odvoda torej je:

```python
f1_cent_ocena
```

Ali:

$$y_i'=\frac{1}{2h}\left(-y_{i-1}+y_{i+1}\right)$$

Uteži torej so:

|    Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$   |$y_{i-1}$ | $y_{i}$   |$y_{i+1}$|
|:--------:|:-------------------:|:----------:|:----------:|
|$y_i'=\frac{1}{2h}\cdot$| -1 | 0 | 1 |

Napaka metode pa je torej drugega reda:

```python
f1_cent_O
```

### Zgled: $\exp(-x)$

Poglejmo si zgled eksponentne funkcije $f(x)=\exp(-x)$ in za točko $x=1,0$ izračunajmo prvi odvod $f'(x)=-\exp(-x)$ pri koraku $h_0=1$ in $h_1=0,1$. 

Najprej pripravimo tabelo numeričnih vrednosti in točen rezultat:

```python
import numpy as np
x0 = np.array([0., 1., 2.]) # korak h=1
y0 = np.exp(-x0)
h0 = x0[1]-x0[0]

x1 = np.array([0.9, 1.0, 1.1]) # korak h=0.1
y1 = np.exp(-x1)
h1 = x1[1]-x1[0]

f1_točno0 = - np.exp(-1) # točen rezultat
f1_točno1 = - np.exp(-1) # točen rezultat
```

Potem uporabimo shemo naprej:

```python
f1_naprej_ocena # da se spomnimo
```

```python
f1_naprej0 = (y0[1:]-y0[:-1])/h0   # korak h_0
f1_naprej1 = (y1[1:]-y1[:-1])/h1   # korak h_1
```

Izračunajmo napako pri $x=1,0$:

```python
f1_točno0 - f1_naprej0[1]
```

```python
f1_točno1 - f1_naprej1[1] # korak h1
```

Potrdimo lahko, da je napaka pri koraku $h/10$ res približno 1/10 tiste pri koraku $h$.

Poglejmo sedaj še napako za centralno diferenčno shemo, ki je drugega reda:

```python
f1_cent_ocena
```

```python
f1_cent0 = (y0[2:]-y0[:-2])/(2*h0) # korak h_0
f1_cent1 = (y1[2:]-y1[:-2])/(2*h1) # korak h_1
```

Analizirajmo napako:

```python
f1_točno0 - f1_cent0[0] # korak h0
```

```python
f1_točno1 - f1_cent1[0] # korak h1
```

Potrdimo lahko, da je napaka pri koraku $h/10$ res približno 1/100 tiste pri koraku $h$.

### Odvod $f''(x)$

Če Taylorjevo vrsto naprej in nazaj seštejemo, se odštejejo lihi odvodi:

```python
def vsota(n=3):
    return f(x+h).series(h, n=n).doit() + f(x-h).series(h, n=n).doit()
vsota(n=4)
```

Določimo drugi odvod:

```python
f2_cent_točno = sym.solve(
           sym.Eq(f(x+h) + f(x-h), vsota(n=4)),   # 1 korak
           f(x).diff(x,2))[0]                     # 2.korak
f2_cent_O = f2_cent_točno.expand().getO()         # 3.korak
f2_cent_ocena = f2_cent_točno.expand().removeO()  # 4.korak
```

Ocena drugega odvoda je:

```python
f2_cent_ocena
```

Ali:

$$y_i''=\frac{1}{h^2}\left(y_{i-1}-2\,y_{i}+y_{i+1}\right)$$

Uteži torej so:

|    Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$   |$y_{i-1}$ | $y_{i}$   |$y_{i+1}$ |
|:--------:|:-------------------:|:----------:|:----------:|
|$y_i''=\frac{1}{h^2}\cdot$ | 1 | -2 | 1 |

Napaka metode pa je ponovno drugega reda:

```python
f2_cent_O
```

### Odvod $f'''(x)$

Če želimo določiti tretji odvod, moramo Taylorjevo vrsto razviti do stopnje 5:

```python
eq_h = sym.Eq(f(x+h)-f(x-h), razlika(n=5))
eq_h
```

Uporaba 1. odvoda, ki smo ga izpeljali zgoraj, nam ne bi koristila, saj je red napake $\mathcal{O}\left(h^{2}\right)$, kar pomeni, da bi v zgornji pri deljenju s $h^3$ dobili $\mathcal{O}\left(h^{-1}\right)$.

Uporabimo trik: ponovimo razvoj, vendar na podlagi dodatnih točk, ki sta od $x$ oddaljeni za $2h$ in $-2h$:

```python
eq_2h = eq_h.subs(h, 2*h)
eq_2h
```

Sedaj imamo dve enačbi in dve neznanki; sistem bomo rešili po korakih:

1. enačbo `eq_h` rešimo za prvi odvod,
2. enačbo `eq_2h` rešimo za prvi odvod,
3. enačimo rezultata prvih dveh korakov in rešimo za tretji odvod,
4. določimo napako metode,
5. določimo oceno odvoda.

Izvedimo navedene korake:

```python
f3_cent_točno = sym.solve(
        sym.Eq(sym.solve(eq_h, f(x).diff(x))[0],  # 1. korak
        sym.solve(eq_2h, f(x).diff(x))[0]),       # 2. korak
        f(x).diff(x,3))[0]                        # 3. korak
f3_cent_O = f3_cent_točno.expand().getO()         # 4.korak
f3_cent_ocena = f3_cent_točno.expand().removeO()  # 5.korak
```

Ocena 3. odvoda je:

```python
f3_cent_ocena
```

Ali:

$$y_i'''=\frac{1}{h^3}\left(-y_{i-2}/2+y_{i-1}-y_{i+1}+y_{i+2}/2\right)$$

Uteži torej so:

|    Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$   |$y_{i-2}$|$y_{i-1}$ | $y_{i}$   |$y_{i+1}$ |$y_{i+2}$|
|:--------:|:-------------------:|:----------:|:----------:|:----------:|:----------:|
|$y_i'''=\frac{1}{h^3}\cdot$| -0.5 | 1 | 0 | -1 | 0.5|

Potrdimo, da je napaka metode drugega reda:

```python
f3_cent_O
```

### Odvod $f^{(4)}(x)$

Ponovimo podoben postopek kot za 3. odvod, vendar za 4. odvod seštevamo Taylorjevo vrsto (do stopnje 6) naprej in nazaj:

```python
eq_h = sym.Eq(f(x+h)+f(x-h), vsota(n=6))
eq_h
```

Pripravimo dodatno enačbo na podlagi točk, ki sta od $x$ oddaljeni za $2h$ in $-2h$:

```python
eq_2h = eq_h.subs(h, 2*h)
eq_2h
```

Iz dveh enačb določimo 4. odvod:

```python
f4_cent_točno = sym.solve(
        sym.Eq(sym.solve(eq_h, f(x).diff(x,2))[0],  # 1. korak
        sym.solve(eq_2h, f(x).diff(x,2))[0]),       # 2. korak
        f(x).diff(x,4))[0]                          # 3. korak
f4_cent_O = f4_cent_točno.expand().getO()           # 4.korak
f4_cent_ocena = f4_cent_točno.expand().removeO()    # 5.korak
```

Ocena 4. odvoda je:

```python
f4_cent_ocena
```

Ali:

$$y_i^{(4)}=\frac{1}{h^4}\left(y_{i-2}-4\,y_{i-1}+6\,y_i-4\,y_{i+1}+y_{i+2}\right)$$

Uteži torej so:

|    Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$   |$y_{i-2}$|$y_{i-1}$ | $y_{i}$   |$y_{i+1}$ |$y_{i+2}$|
|:--------:|:-------------------:|:----------:|:----------:|:----------:|:----------:|
|$y_i^{(4)}=\frac{1}{h^4}\cdot$| 1 | -4 | 6 | -4 | 1|

Potrdimo, da je napaka metode drugega reda:

```python
f4_cent_O
```

### Povzetek centralne diferenčne sheme

Zgoraj smo izpeljali prve štiri odvode z napako metode 2. reda. Bistvo zgornjih izpeljav je, da nam dajo **uteži**, s katerimi moramo množiti funkcijske vrednosti, da izračunamo približek določenega odvoda. Iz tega razloga bomo tukaj te uteži zbrali. Če ste neučakani, lahko skočite na tabelo spodaj. Z branjem nadaljujte, če pa želite spoznati, kako predhodno izpeljane izraze strojno uredimo.

Najprej zberimo vse ocene odvodov v seznam:

```python
odvodi = [f1_cent_ocena, f2_cent_ocena, f3_cent_ocena, f4_cent_ocena]
odvodi
```

Na razpolago imamo 5 funkcijskih vrednosti (pri legah $x-2h, x-h, x, x+h, x+2h$), ki jih damo v seznam:

```python
funkcijske_vrednosti = [f(x-2*h), f(x-h), f(x), f(x+h), f(x+2*h)]
```

Utež prvega odvoda za funkcijsko vrednosti $f(x-h)$ izračunamo:

```python
f1_cent_ocena.expand().coeff(funkcijske_vrednosti[1])
```

Sedaj posplošimo in izračunajmo uteži za vse funkcijske vrednosti in za vse ocene odvodov:

```python
centralna_diff_shema = [[odvod.expand().coeff(fv) for fv in funkcijske_vrednosti] \
                        for odvod in odvodi]
centralna_diff_shema
```

Zgornje povzetke lahko tudi zapišemo v tabelarični obliki:

|    Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$   |$y_{i-2}$|$y_{i-1}$ | $y_{i}$   |$y_{i+1}$ |$y_{i+2}$|
|:--------:|:-------------------:|:----------:|:----------:|:----------:|:----------:|
|$y_i'=\frac{1}{h}\cdot$| 0     | -0.5 | 0 | 0.5 | 0|
|$y_i''=\frac{1}{h^2}\cdot$| 0 | 1 | -2 | 1 | 0|
|$y_i'''=\frac{1}{h^3}\cdot$| -0.5 | 1 | 0 | -1 | 0.5|
|$y_i^{(4)}=\frac{1}{h^4}\cdot$| 1 | -4 | 6 | -4 | 1|

Prikazana centralna diferenčna shema ima napako 2. reda $\mathcal{O}(h^{2})$.

Opomba: v kolikor vas zanima posplošitev diferenčne metode, prosim glejte paket [findiff](https://github.com/maroba/findiff).

### Izboljšan približek - Richardsonova ekstrapolacija

Če je točen odvod je izračunan kot:

$$f'(x_i)=f_o'(x_i)+e,$$

kjer je $f_o'(x_i)$ numerično izračunan odvod v točki $x_i$ in $e$ ocena napake.

Za metodo reda točnosti $n$: $\mathcal{O}(h^{n})$ pri koraku $h$ velja:

$$f'(x_i)=f_o'(x_i, h)+K\,h^n,$$

kjer je $K$ neznana konstanta. 

Če korak razpolovimo in predpostavimo, da se $K$ ne spremeni, velja:

$$f'(x_i)=f_o'\left(x_i, \frac{h}{2}\right)+K\,\left(\frac{h}{2}\right)^n.$$

Iz obeh enačb izločimo konstanto $K$ in določimo izboljšan približek:

$$\overline{f}'(x_i)=\frac{2^n\,f_o'\left(x_i, \frac{h}{2}\right)-f_o'(x_i, h)}{2^n-1}$$

#### Zgled 

Poglejmo si zgled $f(x)=\sin(x)$ (analitični odvod je: $f'(x)=\cos(x)$):

```python
x = np.linspace(0, 2*np.pi, 9)
y = np.sin(x)
```

Pri koraku $h$ imamo funkcijske vrednosti definirane pri:

```python
x
```

Numerični odvod pri $x=\pi$:

```python
x[4]
```

in koraku $h$ je

```python
h = x[1] - x[0]
f_ocena_h = (-0.5*y[3] + 0.5*y[5])/h
f_ocena_h
```

Izračun pri koraku $2h$:

```python
h2 = x[2] - x[0]
f_ocena_2h = (-0.5*y[2] + 0.5*y[6])/h2
f_ocena_2h
```

Izračunajmo izboljšano oceno za $x=\pi$:

```python
f_ocena_izboljšana = (2**2 * f_ocena_h - f_ocena_2h)/(2**2-1)
f_ocena_izboljšana
```

Vidimo, da je izboljšana ocena najbližje teoretični vrednosti $\cos(\pi)=-1$.

## Necentralna diferenčna shema

Centralna diferenčna shema, ki smo jo spoznali zgoraj, je zelo uporabna in relativno natančna. Ker pa je ne moremo vedno uporabiti (recimo na začetku ali koncu tabele), si moramo pomagati z **necentralnimi diferenčnimi shemami** za računanje odvodov.

Poznamo:

* **diferenčno shemo naprej**, ki odvod točke aproksimira z vrednostmi funkcije v naslednjih  točkah in 
* **diferenčno shemo nazaj**, ki odvod točke aproksimira z vrednostmi v predhodnih točkah.

Izpeljave so podobne, kakor smo prikazali za centralno diferenčno shemo, zato jih tukaj ne bomo obravnavali in bomo prikazali samo končni rezultat.

### Diferenčna shema naprej

Diferenčna shema naprej z redom napake $\mathcal{O}(h^{1})$:

|  Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$        |$y_{i}$|$y_{i+1}$ | $y_{i+2}$   |$y_{i+3}$ |$y_{i+4}$|
|:--------:|:-------------------:|:----------:|:----------:|:----------:|:----------:|
|$y_i'=\frac{1}{h}\cdot$| -1     | 1 | 0 | 0 | 0|
|$y_i''=\frac{1}{h^2}\cdot$| 1 | -2 | 1 | 0 | 0|
|$y_i'''=\frac{1}{h^3}\cdot$| -1 | 3 | -3| 1 | 0|
|$y_i^{(4)}=\frac{1}{h^4}\cdot$| 1 | -4 | 6 | -4 | 1|

Diferenčna shema naprej z redom napake $\mathcal{O}(h^{2})$:

|  Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$        |$y_{i}$|$y_{i+1}$ | $y_{i+2}$   |$y_{i+3}$ |$y_{i+4}$|$y_{i+5}$|
|:--------:|:-------------------:|:----------:|:----------:|:----------:|:----------:|:----------:|
|$y_i'=\frac{1}{2h}\cdot$| -3 | 4 | -1| 0 | 0|  0| 
|$y_i''=\frac{1}{h^2}\cdot$| 2 | -5 | 4 | -1| 0| 0| 
|$y_i'''=\frac{1}{2h^3}\cdot$| -5 | 18| -24| 14| -3| 0| 
|$y_i^{(4)}=\frac{1}{h^4}\cdot$| 3 | -14 | 26 | -24 | 11| -2|

### Diferenčna shema nazaj

Diferenčna shema nazaj z redom napake $\mathcal{O}(h^{1})$:

| Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$         |$y_{i-4}$|$y_{i-3}$ | $y_{i-2}$   |$y_{i-1}$ |$y_{i}$|
|:--------:|:-------------------:|:----------:|:----------:|:----------:|:----------:|
|$y_i'=\frac{1}{h}\cdot$| 0 | 0| 0 | -1| 1|
|$y_i''=\frac{1}{h^2}\cdot$| 0 | 0 | 1 | -2| 1|
|$y_i'''=\frac{1}{h^3}\cdot$| 0 | -1| 3| -3| 1|
|$y_i^{(4)}=\frac{1}{h^4}\cdot$| 1 | -4 | 6 | -4 | 1|

Diferenčna shema nazaj z redom napake $\mathcal{O}(h^{2})$:

| Odvod $\downarrow$ $\backslash$ Vrednosti $\rightarrow$         |$y_{i-5}$|$y_{i-4}$|$y_{i-3}$ | $y_{i-2}$   |$y_{i-1}$ |$y_{i}$|
|:--------:|:-------------------:|:----------:|:----------:|:----------:|:----------:|:----------:|
|$y_i'=\frac{1}{2h}\cdot$| 0 | 0| 0| 1 |-4| 3| 
|$y_i''=\frac{1}{h^2}\cdot$| 0 | 0 | -1| 4|-5| 2| 
|$y_i'''=\frac{1}{2h^3}\cdot$| 0 | 3| -14| 24|-18| 5| 
|$y_i^{(4)}=\frac{1}{h^4}\cdot$| -2| 11| -24| 26 | -14| 3|

## Uporaba ``numpy.gradient``

Za izračun numeričnih odvodov (centralna diferenčna shema 2. reda) lahko uporabimo tudi ``numpy.gradient()`` ([dokumentacija](http://docs.scipy.org/doc/numpy/reference/generated/numpy.gradient.html)):

```python
gradient(f, *varargs, **kwargs)
```

kjer `f` predstavlja tabelo vrednosti (v obliki numeričnega polja) funkcije, katere odvod iščemo. `f` je lahko ene ali več dimenzij. Pozicijski parametri `varargs` definirajo razdaljo med vrednostmi argumenta funkcije `f`; privzeta vrednost je 1. Ta vrednost je lahko skalar, lahko pa tudi seznam vrednosti neodvisne spremenljivke (ali tudi kombinacija obojega). Gradientna metoda na robovih uporabi shemo naprej oziroma nazaj; parameter `edge_order` definira red sheme, ki se uporabi na robovih (izbiramo lahko med 1 ali 2, privzeta vrednost je 1).

Rezultat funkcije `gradient` je numerični seznam (ali seznam numeričnih seznamov) z izračunanimi odvodi.

Za podrobnosti glejte [dokumentacijo](http://docs.scipy.org/doc/numpy/reference/generated/numpy.gradient.html).

Za odvajanje funkcije (in ne tabele vrednosti), glejte: [`scipy.differentiate`](https://docs.scipy.org/doc/scipy/reference/differentiate.html).

### Zgled

Pogledali si bomo zgled, kako uporabimo **uteži**, funkcijo gradient in posebnosti na robovih. Najprej pripravimo tabelo podatkov:

```python
x, h = np.linspace(0, 1, 20, retstep=True)
y = np.sin(2*np.pi*x)
```

Uteži diferenčih shem:

```python
centralna = np.array([-0.5, 0, 0.5]) # bi lahko tudi pridobili prek central_diff_weights(3,1)
naprej = np.array([-3/2, 2, -1/2])
nazaj = np.array([1/2, -2, 3/2])
```

Sedaj izvedemo odvod notranjih točk (prvi način je z izpeljevanjem seznamov, drugi je vektoriziran):

```python
odvod_notranje = np.array([y[i-1:i+2] @ centralna/h for i in range(1, len(x)-1)]) # izpeljevanje seznamov
odvod_notranje = np.convolve(y, centralna[::-1], mode='valid') / h # vektoriziran
```

Na robovih uporabimo diferenčno shemo naprej oziroma nazaj:

```python
odvod_prva = y[:len(naprej)] @ naprej / h  # naprej
odvod_zadnja = y[-len(nazaj):] @ nazaj / h # nazaj
```

Sestavimo rezultat:

```python
odvod_cel = np.hstack([odvod_prva, odvod_notranje, odvod_zadnja])
```

Prikažemo rezultat skupaj z rezultatom funkcije `np.gradient`:

```python
import matplotlib.pyplot as plt
%matplotlib inline
plt.plot(x, odvod_cel, 'ko', lw=3, label='lastna implementacija')
plt.plot(x, np.gradient(y, h), 'g.', label='np.gradient, edge_order=1')
plt.plot(x, np.gradient(y, h, edge_order=2), 'r.', label='np.gradient, edge_order=2')
plt.legend();
```

### Uporaba `numpy.diff`

Dajmo tukaj predstaviti še pogosto uporabljeno funkcijo `numpy.diff` ([documentacija](https://numpy.org/doc/stable/reference/generated/numpy.diff.html)) izračuna `n`-to diskretno razliko vzdolž podane osi. Prva razlika je podana kot $a[i+1] - a[i]$. To je osnovna operacija za numerično odvajanje  (gre za odvod prve stopnje natančnosti), vendar moramo rezultat deliti s korakom $h$.

Pri uporabi `np.diff` se dolžina polja zmanjša za 1.

```python
x_d = np.linspace(0, 2*np.pi, 20)
y_d = np.sin(x_d)
h_d = x_d[1] - x_d[0]

# Prvi odvod z np.diff (naprej diferenca)
dy = np.diff(y_d) / h_d
x_diff = x_d[:-1] # x os se zmanjša

plt.figure()
plt.plot(x_d, np.cos(x_d), label='Točen odvod')
plt.plot(x_diff, dy, 'o', label='np.diff aproksimacija')
plt.legend()
plt.title('Uporaba np.diff za odvajanje')
plt.show()
```

## Zaokrožitvena napaka pri numeričnem odvajanju

Zgoraj smo se osredotočili na napako metode. Pri numeričnem odvajanju pa moramo biti zelo pozorni tudi na **zaokrožitveno** (ali tudi *upodobitveno*) napako! Poglejmo si prvi odvod (po centralni diferenčni shemi) zapisan z napako metode ($k\,h^2$) in zaokrožitveno napako $\varepsilon$:

$$y_i'=\frac{1}{2h}\left((-y_{i-1}\pm\varepsilon)+(y_{i+1}\pm\varepsilon)\right) + k\,h^2$$

V najslabšem primeru se zaokrožitvena napaka sešteje in je skupna napaka:

$$n=\frac{\varepsilon}{h}+k\,h^2$$

Ko je $h$ velik prevladuje napaka metode $k\,h^2$; ko pa je $h$ majhen, pa prevladuje zaokrožitvena napaka. Napaka ima minimum, ko velja:

$$n'=-\frac{\varepsilon}{h^2}+2\,k\,h=0.$$

Sledi:

$$h=\sqrt[3]{\frac{\varepsilon}{2\,k}}.$$

### Zgled

Spodaj si bomo pogledali primer, kjer bomo natančnost spreminjali v treh korakih:

1. ``float16`` -
    16-bitni zapis: predznak 1 bit, 5 bitov eksponent, 10 bitov mantisa
2. ``float32`` -
    32-bitni zapis: predznak 1 bit, 8 bitov eksponent, 23 bitov mantisa
3. ``float64`` -
    64-bitni zapis: predznak 1 bit, 11 bitov eksponent, 52 bitov mantisa (to je privzeta natančnost).

Za več o tipih v ``numpy`` glejte [dokumentacijo](http://docs.scipy.org/doc/numpy/user/basics.types.html)

Določimo sedaj osnovno zaokrožitveno napako za posamezni tip:

```python
eps16 = np.finfo(np.float16).eps
eps32 = np.finfo(np.float32).eps
eps64 = np.finfo(np.float64).eps
print(f'Osnovna zaokrožitvena napaka za tipe \
`float16`, `float32` in `float64` je:\n{[eps16, eps32, eps64]}')
```

Kot primer si poglejmo seštevanje: k številu `1.` prištejemo polovico osnovne zaokrožitvene napake `eps16` in pretvorimo v tip `float16`, ugotovimo, da je nova vrednost še vedno enaka vrednosti `1.`:

```python
(1.+eps16/2).astype('float16')
```

Definirajmo najprej funkcijo $\exp(x)$, ki bo dala rezultat natančnosti, ki jo definira parameter `dtype`:

```python
def fun(x, dtype=float): # funkcija
    return np.exp(-dtype(x)).astype(dtype)
```

Definirajmo še funkcijo za analitično določljiv odvod (to bomo pozneje potrebovali za določitev relativne napake):

```python
def f1_fun(x): # "točen" odvod funkcije
    return -np.exp(-x)
```

Podobno kakor zgoraj pri seštevanju, lahko tudi pri vrednosti funkcije ugotovimo, da sprememba vrednosti $x$, ki je manjša od $\epsilon$, vodi v isti rezultat:

```python
fun(1., dtype=np.float16)
```

```python
fun(1+eps16/2, dtype=np.float16)
```

Uporabimo sedaj centralno diferenčno shemo za prvi odvod. Pri tem naj bodo števila zapisana z natančnostjo `dtype`, s pomočjo točnega odvoda pa se izračuna še relativna napaka:

```python
def f1_CDS(fun, x, h, dtype=np.float64):
    f1_ocena = (fun(x+h, dtype=dtype)-fun(x-h,dtype=dtype))/(2*dtype(h))
    f1_točno = f1_fun(x)
    relativna_napaka = (f1_točno - f1_ocena) / f1_točno
    return f1_ocena, relativna_napaka
```

Poglejmo primer odvoda pri $x=1,0$ (Python funkcija vrne vrednost in relativno napako):

```python
f1_CDS(fun, x=1., h=.01, dtype=np.float16)
```

```python
f1_CDS(fun, x=1., h=.01, dtype=np.float64)
```

Definirajmo sedaj korak:

```python
h=0.25**np.arange(30)
h[:10]
```

Izračunamo oceno odvodov za različne natančnosti zapisa (zaradi deljenja z 0 dobimo opozorilo):

```python
f1_16 = f1_CDS(fun, x=1., h=h, dtype=np.float16)
f1_32 = f1_CDS(fun, x=1., h=h, dtype=np.float32)
f1_64 = f1_CDS(fun, x=1., h=h, dtype=np.float64)
```

Izrišemo različne tipe v odvisnosti od velikosti koraka $h$. Najprej uvozimo potrebne knjižnice:

```python
import matplotlib.pyplot as plt
%matplotlib inline
```

Definirajmo sliko:

```python
def fig_ocena():
    plt.semilogx(h, f1_16[0], 'b', lw=3, alpha=0.5, label='float16')
    plt.semilogx(h, f1_32[0], 'r', lw=3, alpha=0.5, label='float32')
    plt.semilogx(h, f1_64[0], 'g', lw=3, alpha=0.5, label='float64=float')
    plt.title('Ocena odvoda za različne tipe natančnosti')
    plt.xlabel('$h$')
    plt.ylabel('Ocena odvoda: $-\\exp(-1)=−0.3678794$')
    plt.ylim(-0.37, -0.365)
    plt.legend();
```

Prikažimo jo:

```python
fig_ocena()
```

Pri relativno velikem koraku $h$ prevladuje napaka metode, pri majhnem koraku pa zaokrožitvnea napaka; optimalni korak lahko ocenimo glede na:

$$h=\sqrt[3]{\frac{\varepsilon}{2\,k}}.$$

V konkretnem primeru velja:

$$k=-\frac{f'''(x)}{6}=-\frac{-e^{-x}}{6}=\frac{1}{6\,e}\qquad(x=1)$$

Sledi:

$$h=\sqrt[3]{3\,\varepsilon\,e}.$$

Izračunamo primeren korak za 16, 32 in 64-bitni zapis:

```python
np.power(3*eps16*np.exp(1),1/3)
```

```python
np.power(3*eps32*np.exp(1),1/3)
```

```python
np.power(3*eps64*np.exp(1),1/3)
```

Najbolje se izkaže 64-bitni zapis, vendar pa tudi pri tem korak manjši od cca `1e-5` ni priporočen!

## Aproksimacija odvoda s kompleksnim korakom

Aproksimacija odvoda s kompleksnim korakom je uporabna takrat, ko imamo definirano katerokoli analitično funkcijo, vendar pa nimamo na voljo analitičnega izraza za prvi odvod, za podrobnosti glejte ([vir](https://nhigham.com/2020/10/06/what-is-the-complex-step-approximation/)). Ideja izhaja iz razvoja funkcije $f(x)$ v Taylorjevo vrsto, vendar se izvede korak v smeri imaginarne osi $i\,h$ ($i=\sqrt(-1)$):

$$f{\left(x + i\,h \right)} = f{\left(x \right)} + i\,h\,\frac{d}{d x} f{\left(x \right)} - \frac{h^{2}}{2} \frac{d^{2}}{d x^{2}} f{\left(x \right)} + O\left(h^{3}\right)$$

Če sedaj predpostavimo, da funkcija $f(x)$ realne vrednosti slika na realno os in da sta $x$ in $h$ realni vrednosti, potem lahko izpeljemo:

$$\mathbf{Im} \left( f{\left(x + i\,h \right)}\right)= h\,\frac{d}{d x} f{\left(x \right)}+ O\left(h^{3}\right)$$

$$\mathbf{Re} \left( f{\left(x + i\,h \right)}\right) = f{\left(x \right)} - \frac{h^{2}}{2} \frac{d^{2}}{d x^{2}} f{\left(x \right)} + O\left(h^{3}\right)\rightarrow
\mathbf{Re} \left( f{\left(x + i\,h \right)}\right) = f{\left(x \right)} + O\left(h^{2}\right)$$

Iz prve enačbe (zgoraj) potem izpeljemo izraz za prvi odvod:

$$f'{\left(x \right)}=\frac{\mathbf{Im} \left( f{\left(x + i\,h \right)}\right)}{h}+ O\left(h^{2}\right)$$

Rezultat je na nek način zelo presenetljiv. Zgoraj smo z eno koračno shemo naprej uspeli pridobiti natančnost $O\left(h^{1}\right)$, tukaj pa $O\left(h^{2}\right)$!


Pomembna prednost metode s kompleksnim korakom je, da **nima težav z odštevanjem skoraj enakih števil** (*ang.* cancellation error), ki pesti klasične diferenčne metode pri zelo majhnih korakih $h$. Zato lahko izberemo izjemno majhen $h$ (npr. $10^{-200}$) in dobimo praktično točen rezultat (do strojne natančnosti).

Če pogledamo sedaj uporabo metoda na primeru od zgoraj $\exp(-x)$, spomnimo se najprej točnega rezultata pri vrednosti $x=1$:

```python
f1_točno0
```

Pri klicu s korakom $h=0.1$ dobimo točni dve (tri) števki (podobno kakor pri centralni diferenčni shemi), zmanjšanjem koraka na desetino pa bi število točnih števk približno podvojili.

```python
h1_kompleksno = 0.1
y1_kompleksno = np.exp(-(1.+1.j*h1_kompleksno))
f1_kompleksno = np.imag(y1_kompleksno)/h1_kompleksno
f1_kompleksno
```

Ker nimamo težav z odštevanjem skoraj enakih števil, lahko korak izjemno zmanjšamo in dobimo točen rezulta (do strojne natančnosti):

```python
h1_kompleksno = 1e-10
y1_kompleksno = np.exp(-(1.+1.j*h1_kompleksno))
f1_kompleksno = np.imag(y1_kompleksno)/h1_kompleksno
f1_kompleksno
```

```python
np.sin(2*x0*1e-5j)
```

```python
np.sin(x0**2+2*x0*1e-5j)
```

Poglejmo si še primer odvoda funkcije `sin(x**2)`. Iz kode in slike spodaj vidimo bistveno hitrejšo konvergenco metode kompleksnega koraka. Zakaj metoda deluje tako dobro? Če vstavimo $x+i\,h$ v $x^2$, dobimo: $(x+i\,h)^2 = x^2 + 2i\,x\,h - h^2$

Ker je $h$ zelo majhen, je $h^2$ zanemarljiv v realnem delu, imaginarni del $2\,x\,h$ pa se linearno prenese skozi sinus. Na tak način dobimo približek teoretičnega odvoda $2\,x\,\cos(x^2)$. Metoda *vidi* odvod v imaginarnem delu brez odštevanja, zato ne pride do napake zaokroževanja.

```python
import matplotlib.pyplot as plt
import numpy as np

# Funkcija: sin(x^2)
def f(x):
    return np.sin(x**2)

# Analitični odvod: 2x * cos(x^2)
def df_analytical(x):
    return 2*x * np.cos(x**2)

x0 = 1.5
točno = df_analytical(x0)

h_vals = np.logspace(-10, 0, 100)
err_fd = []
err_cs = []

for h in h_vals:
    # Končna razlika (Forward)
    fd_approx = (f(x0 + h) - f(x0)) / h
    err_fd.append(abs(fd_approx - točno))
    
    # Kompleksni korak
    # f(x + ih) = sin((x+ih)^2) ≈ sin(x^2 + 2ixh) 
    #           ≈ sin(x^2) + i * 2xh * cos(x^2) 
    # Imag/h nam da točno 2x*cos(x^2)
    cs_approx = f(x0 + 1j*h).imag / h
    err_cs.append(abs(cs_approx - točno))

plt.figure(figsize=(10, 6))
plt.loglog(h_vals, err_fd, 'r-o', label='Končna razlika', markersize=4)
plt.loglog(h_vals, err_cs, 'b--', label='Kompleksni korak', linewidth=2)

plt.xlabel('Korak $h$')
plt.ylabel('Absolutna napaka')
plt.title(r'Napaka pri odvajanju $f(x)=\sin(x^2)$')
plt.grid(True, which="both", alpha=0.5)
plt.legend()
plt.show()
```

# Dodatno

[Video predavanja na temo numeričnega odvajanja](https://www.youtube.com/watch?v=ZJkGI5DZQv8&list=PLYdroRCLMg5OvLx1EtY1ByvveJeTEXQd_&index=18).

---

# Vprašanja za vaje

---

**Vprašanje 1:** Podane so izmerjene vrednosti opravljene poti tekača ``s`` pri $N$ točkah v času. Numerično izračunajte potek hitrosti in pospeška (uporabite lahko poljubno metodo). 

Opazujte in komentirajte kaj se dogaja, ko povečujete število točk $N$.

```python
# Podatki
N = 30
t = np.linspace(0, 12, N)
s_ = 50*(np.cos(2*np.pi*(t-12)/24) +1)
s = s_ + np.random.randn(len(t))
plt.plot(t, s, '-', label='s šumom')
plt.plot(t, s_, 'o', label='brez šuma')
plt.grid()
plt.legend()
```

**Vprašanje 2:** Simbolno je podana funkcija $f(x)$ in njen odvod, $f'(x)$. 

$$x(t) = e^{-t} \Big( \sin(\omega t) - 0.5 \cos(2 \omega t) \Big)$$

$$\omega = 2\pi f \quad \text{in} \quad f=4~\text{Hz}$$

Potek funkcije in njenega odvoda opazujemo pri 100 točkah časa $t\in [0, 1]$.

Pripravi funkciji `x_fun`, `dx_fun` za numerično računanje. Izriši pripravljeni funkciji na intervalu $t\in [0, 1]$ z uporabo paketa `matplotlib`.

```python
# Podatki
import sympy as sym

t_sym = sym.Symbol('t', real=True, positive=True)
w = 2*sym.pi*4
x_sym = sym.exp(-t_sym) * (sym.sin(w*t_sym) - 0.5*sym.cos(2*w*t_sym))
dx_sym = sym.diff(x_sym, t_sym)
```

**Vprašanje 3:** Razdelite opazovan časovni interval $t \in [0,1]$ na 100 ekvidistantnih segmentov. 

Pripravite vektor vrednosti $x(t)$ in odvod $x'(t)$ izračunajte še numerično, z uporabo funkcije ``numpy.gradient(x, h)``.

**Vprašanje 4:** Uporabite funkcijo ``np.gradient`` še za izračun drugega odvoda $x''(t)$. Opazujte vpliv parametra `edge_order` na natančnost odvodov na robovih intervala.

**Vprašanje 5:** Numeričnemu polju $x(t)$ pri 500 ekvidistantnih točkah na intervalu $t \in [0, 1]$ dodajte naključni šum $n(t)$ amplitude $A=0.1$ z uporabo funkcijo ``np.random.randn``. 

$$x_{\text{šum}}(t) = x(t) + A\cdot n(t)$$

Z uporabo funkcije ``np.gradient`` numerično določite vrednost prvega in drugega odvoda šumnega signala.

------

### Uteži centralne diferenčne sheme v ``scipy``

**Primer 6:** Izračunajte vrednosti drugega odvoda $x''(t)$. Uporabite centralno diferenčno shemo drugega odvoda čez 5 točk (red napake 4).

```python
# Podatki
#!pip install findiff
import findiff
findiff.coefficients(deriv=2, acc=4)['center']['coefficients']
```

```python
t, dt = np.linspace(0, 1, 100, retstep=True)
x = sym.lambdify(t_sym, x_sym)(t)
drugi_odvod = findiff.FinDiff(0, dt, 2, acc=4) # axis, dx, derivative, accuracy
plt.plot(t, drugi_odvod(x))
```

**Vprašanje 7:** Na podlagi numeričnih podatkov $x(t)$ pri delitvi intervala $t \in [0,1]$ na 100 točk izračunajte vrednost prvega odvoda, $x'(t)$ po enačbi centralne diferenčne sheme preko treh točk. Ali lahko na ta način določite vrednost odvoda v točkah $t=0$ in $t=1$?

```python
# Podatek
w = np.array(-1/2, 0 1/2)
```

**Vprašanje 8:** Z uporabo diferenčne sheme naprej oz. nazaj izračunajte prvi odvod $x'(t)$ tudi v točkah na robu ($t=0$, $t=1$). Primerjajte dobljene vrednosti z rezultatom funkcije ``np.gradient``, kjer nastavite argument ``edge_order=2``.

$$
\begin{align}
\text{naprej: } & \frac{d}{dx}f(x) = \frac{1}{2h}\Big( -3f(x) + 4f(x+h) -f(x+2h)\Big)\\
\text{nazaj: } & \frac{d}{dx}f(x) = \frac{1}{2h}\Big(f(x-2h) - 4f(x-h) + 3f(x)\Big)
\end{align}$$

(Namig: uporabite lahko funkcijo ``np.dot``)

```python
# Podatki
naprej = np.array([-3/2, 2, -1/2])
nazaj = np.array([1/2, -2, 3/2])
```

--------

### Izboljšanje približka odvoda z Richardsonovo ekstrapolacijo

**Vprašanje 9:** Izračunajte izboljšan približek odvoda funkcije:

$$x(t) = e^{-t} \Big( \sin(8 \pi t) - 0.5 \cos(16 \pi t) \Big)$$

pri vrednosti $t_i = 0.5$, tako, da uporabite zgornjo enačbo za dve različni delitvi, $h=0.05$ in $h/2 = 0.025$. Uporabite centralno diferenčno shemo za 1. odvod za red napake $n=2$.

--------

### Uporaba ``np.convolve``

<img src="fig/vaje/primer_convolve.gif">

**Vprašanje 10:** Uporabite funkcijo ``np.convolve`` in izračunajte vrednosti drugega odvoda $x(t)$ pri delitvi intervala $t \in [0, 1]$ na 100 točk. Uporabite centralno diferenčno shemo drugega odvoda čez 5 točk, uteži izračunajte s pomočjo ``scipy.misc.central_diff_weights``. 

Rezultat grafično prikažite (pazite na število robnih točk!).
