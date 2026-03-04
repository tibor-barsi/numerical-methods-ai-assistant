# Uvod v Python

## Kaj je ekosistem Pythona?

Marsikaterega bralca beseda *ekosistem* zmoti/zmede. [Slovar slovenskega knjižnega jezika](http://www.fran.si/130/sskj-slovar-slovenskega-knjiznega-jezika/3539971/ekosistem?page=4&Query=sistem&All=sistem&FilteredDictionaryIds=130&View=1) danes pozna samo rabo v povezavi z ekologijo. V računalništvu pa se v zadnjih desetletjih beseda ekosistem uporablja tudi v kontekstu t. i. digitalnega ekosistema; v primeru Pythona to pomeni vso množico različih paketov (samo [pypi.org](https://pypi.org) trenutno hrani več kot 685 tisoč različnih paketov), ki temeljijo na programskem jeziku Python in so med seboj povezani.

**Ekosistem Pythona je veliko več kot pa samo programski jezik Python!** Ta knjiga se v okviru prvih štirih predavanj osredotoča na tisti del tega ekosistema, ki je zanimiv in koristen predvsem za inženirske poklice.

Python je **visokonivojski** programski jezik, z visokim nivojim abstrakcije. **Nizkonivojski** programski jeziki, v nasprotju, imajo nizek nivo abstrakcije (npr. strojna koda).

Poglemo si izračun [Fibonaccijevega števila](http://sl.wikipedia.org/wiki/Fibonaccijevo_%C5%A1tevilo), ki sledi definiciji: 

$$F_1 = F_2 = 1\qquad\textrm{in}\qquad F_n = F_{n-1} + F_{n-2}.$$

Izračun $F_n$ v [strojni kodi](http://en.wikipedia.org/wiki/Low-level_programming_language):
![Strojna koda](./fig/strojna_koda.png)

MASM koda
![MASM koda](./fig/mash_koda.png)

C
```c
int Fibonacci(int n)
{
   if ( n == 0 )
      return 0;
   else if ( n == 1 )
      return 1;
   else
      return ( Fibonacci(n-1) + Fibonacci(n-2) );
} 
```

Python
```python
def fib(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib(n-1) + fib(n-2)
```

```python
števila = [1., 2, 3, 4, 5] # seznam števil
[število**2 for število in števila if število>3]
```

Python omogoča visok nivo abstrakcije in zato abstraktno kodo. Sedaj si bomo pogledali primer kode Python:
```python
števila = [1, 2, 3, 4, 5] # seznam števil
[število for število in števila if število%2]
```

Čeprav v tem trenutku še nimamo dovolj znanja, poskusimo razumeti/prebrati:

1. prva vrstica definira seznam števil; vse kar sledi znaku # pa predstavlja komentar

2. rezultat druge vrstice je seznam, kar definirata oglata oklepaja; znotraj oklepaja beremo:

    * vrni `število` za vsako (**for**) `število` v (**in**) seznamu `števila`, če (**if**) je pri deljenju `število` z 2 ostanek 1.

Vidimo, da je nivo abstrakcije res visok, saj smo potrebovali veliko besed, da smo pojasnili vsebino!

Poglejmo rezultat:

```python
števila = [1, 2, 3, 4, 5] # seznam števil
[število for število in števila if število%2]
```

Kratek pregled začetkov nekaterih programskih jezikov:
* 1957 Fortran, * 1970 Pascal, * 1972 C
* 1980 C++, * 1984 Matlab, * 1986 Objective-C
* **1991 Python**, * 1995 Java, * 1995 Delphi
* 2006 Rust, 2009 Go, 2012 Julija

## Kdo se uči Python, zakaj ga uporabljati?

**ZDA** 

[Python je jezik, ki se ga najpogosteje učijo na najboljših Ameriških univerzah.](http://goo.gl/mJC1P8)
![Top](./fig/Top39-700.4.png)

Na univerzi Berkeley v [začetku študijskega leta 2017/18](https://youtu.be/xuNj5paMuow?t=13m18s) (več kot 1200 študentov pri predmetu *Data science*, ki temelji na tehnologiji Jupyter notebook).

```python
%%html
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">First day of class in Foundations of <a href="https://twitter.com/hashtag/Datascience?src=hash">#Datascience</a> for <a href="https://twitter.com/BerkeleyDataSci">@BerkeleyDataSci</a> - all materials at <a href="https://t.co/yyoJbaKlnL">https://t.co/yyoJbaKlnL</a> <a href="https://t.co/J6DLd0VQTl">pic.twitter.com/J6DLd0VQTl</a></p>&mdash; Cathryn Carson (@CathrynCarson) <a href="https://twitter.com/CathrynCarson/status/900408152348270592">August 23, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
```

Python v zadnjih 10 letih pospešeno pridobiva na popularnosti in je danes eden od najboljših ([IEEE Spectrum](https://spectrum.ieee.org/top-programming-languages/#toggle-gdpr).) in tudi najpopularnejših programskih jezikov ([Why is Python Growing so Quickly?](https://stackoverflow.blog/2017/09/14/python-growing-quickly/) in [Python's explosive growth](https://www.techrepublic.com/article/programming-languages-pythons-growth-is-absolutely-explosive-says-anaconda-ceo-and-not-slowing-down/)).

## Uporaba Pythona?

Nekatera mednarodna podjetja: *Nasa, Google, Cern*.

Nekatera podjetja v Sloveniji: *Gorenje, Kolektor, Iskra Mehanizmi, Hidria, Mahle, Domel, ebm-papst Slovenia, Efaflex...*

Čedalje več programov, ki imajo vgrajeno skriptno podporo za Python: *Abaqus, Ansys Workbench, MSC (Adams, SimXpert), MS Excel, ParaView, LMS Siemens, AVL.*

Nekateri komercialni programi v Pythonu:  *BitTorrent, Dropbox.*

Obširni seznam je [tukaj](http://en.wikipedia.org/wiki/List_of_Python_software).

## Python vs Matlab

Matlab je bil v preteklosti pogosto poučevan na univerzah in imel tudi veliko uporabnikov v industriji. Že vrsto let je ekosistem Pythona za večino opravil  bistveno močnejše orodje. Na spletu boste našli ogromno primerjav, npr.: [Python vs Matlab](http://www.pyzo.org/python_vs_matlab.html).

Kratka primerjave glede na [vir](http://www.southampton.ac.uk/~fangohr/training/python/pdfs/Python-for-Computational-Science-and-Engineering-slides.pdf):

![Primerjava py](./fig/py_comparison.png)

Sintaksa je načeloma zelo podobna. Pomembna razlika je, da se indeksi pri **Matlabu** začnejo z 1, v **Pythonu** pa z 0.

**Če ste mlajši od 25-30 let, preostanek te celice verjetno lahko preskočite.**

Če niste, ste se verjetno učili Matlab in se sprašujete zakaj je v vašem klubu čedalje manj članov. Verjetno je čas za prehod na Python; tukaj prispevek, ki vam lahko pomaga: [MATLAB vs Python: Why and How to Make the Switch](https://realpython.com/matlab-vs-python/).

## Namestitev Pythona

Pojdite na [www.python.org](https://www.python.org/downloads/) ter namestite Python; *pazite, da izberete verzijo 3.10 ali novejšo* (64 bit). Pri tem sledite pripravljenim [video navodilom](https://youtu.be/PWSombIVVDw).

### Uporaba githuba repozitorija in posodobitve

Ta knjiga se vedno dopolnjuje, zadnja verzija je dosegljiva na:

* v izvorni obliki na naslovu [github.com/jankoslavic/pynm](https://github.com/jankoslavic/pynm/),
* v spletni obliki na naslovu [https://jankoslavic.github.io/pynm](https://jankoslavic.github.io/pynm/).

Najbolje je za sinhronizacijo s spletnim repozitorijem uporabljati kakšnega od Git klientov, npr. [GitHub Desktop](https://desktop.github.com) ali znotraj Visual Studio Code (predsatvljen v nadaljevanju). Git je sicer zelo močno orodje in skupini razvojnikov omogoča relativno enostavno delo na isti kodi in kontrolo nad različnimi verzijami datotek.

### Visual Studio Code

![VSC](./fig/vsc.png)

Pri tem predmetu bomo v glavnem programirali v Jupyter Lab/Notebooku znotraj [Visual Studio Code - VSC](https://code.visualstudio.com/), kar nam omogoča zelo učinkovito programiranje ob uporabi polno formatiranega besedila in interaktivnih prikazov.
Za namestitev VSC sledite [pripravljenim video navodilom](https://youtu.be/PWSombIVVDw&t=40).

 Kadar želimo programirati obsežnejše in bolj splošno uporabne pakete ter namenske programe, pa se ponavadi poslužimo še drugih funkcij, ki nam jih razvojno okolije (angl.  *integrated development environment* - IDE) VSC ponuja. V zadnjem obdobju VSC, zaradi hitrosti, enostavnosti uporabe in razširljivosti, pridobiva na priljubljenosti. Kako nam lahko VSC pomaga pri programiranju odlično prikaže video posnetek na temo [produktivnosti z VSC](https://www.youtube.com/watch?v=6YLMWU-5H9o).

### Nameščanje dodatkov in posodobitev

Na [pypi.org](https://pypi.org) se nahaja veliko različnih paketov! Namestimo jih iz ukazne vrstice\* (angl. *command prompt*) z ukazom **pip**.

\* Ukazno vrstico prikličete v operacijskem sistemu Windows tako, da kliknite `"Start"` in napišite `"command prompt"` ter pritisnete `"enter"` ali pa odprete terminal znotraj VSC ([video](https://youtu.be/PWSombIVVDw&t=146)).

Podrobnosti nameščanja dodatkov si bomo pogledali naslednjič. Da boste imeli zadnjo verzijo paketov, izvedite naslednje:

1. Odprite ukazno vrstico,
2. sprožite ukaz za posodobitev paketov, ki jih bomo potrebovali tekom predmeta:
    ```python
    pip install numpy scipy matplotlib sympy requests jupyter --upgrade
    ```

## Jupyter notebook (Jupyter lab)

[Jupyter notebook](https://jupyter.org/) je  interaktivno okolje, ki deluje kot spletna aplikacija in se lahko uporablja z različnimi programskimi jeziki (npr. Python, Julia, R, C, Fortran ... iz prvih treh je nastalo tudi ime JuPyt[e]R). 

Jupyter notebook je najlažje preizkusiti tukaj: [try.jupyter.org](https://try.jupyter.org/) ali [colab.research.google.com](https://colab.research.google.com).

*Jupyter notebook* zaženemo (v poljubni mapi) iz *ukazne vrstice* ali iz *Raziskovalca/Explorerja* z ukazom:
``jupyter notebook``,

![Jupyter notebook](./fig/jupyter_notebook.png)

ali znotraj VSC kot je predsatvljeno v [posnetku](https://youtu.be/PWSombIVVDw&t=227).

Opombe: 
* s sprožitvijo ``jupyter lab`` pridemo do nadgradnje `jupyter notebook` okolja (rahlo bolj zahtevno okolje, vendar bistveno bolj napredno okolje),
* tukaj predpostavimo, da je pot do datoteke jupyter[.exe] dodana v spremenljivke okolja (in jo lahko prožite iz ukazne vrstice v poljubni mapi),
* če želite ukazno vrstico sprožiti v poljubni mapi, potem v *explorerju* pridržite ``shift`` in kliknite mapo z desnim gumbom; nato izberite *Odpri ukazno vrsticu tukaj / Open command window here*.

Do Jupyter strežnika dostopamo prek spletnega brskalnika. Najprej se vam prikaže seznam map in datotek do katerih lahko dostopate v okviru mape, v kateri ste sprožili ukaz `jupyter notebook`. Nato poiščite v desnem kotu zgoraj ikono *new* in nato kliknite *Python 3*. S tem boste zagnali vaš prvi Jupyter notebook (v okviru jedra Python). Najdite ukazno vrstico in na desni strani kliknite na *Help* in nato *User interface tour*. Poglejte si še *Help/Keyboard shortcuts*.

Jupyter notebook je sestavjen iz t. i. *celic* (angl. cell). 

Celica ima dve stanji:

* **Ukazno stanje / Command mode**: v to stanje se vstopi s pritiskom [escape],
* **Stanje za urejanje / Edit mode** v to stanje se vstopi s pritiskom [enter].

V obeh primerih se celico izvrši s pritiskom [shift + enter].

Celice so lahko različnega tipa; tukaj bomo uporabljali dva tipa:

* **Code**: v tem primeru celica vsebuje programsko kodo (bližnjica v ukaznem stanju je tipka [y]),
* **Markdown**: v tem primeru celica vsebuje besedilo oblikovano po standardu *Markdown* (bližnjica v ukaznem stanju je tipka [m]).

### Markdown

Jupyter notebook omogoča zapis v načinu *markdown*, ki ga je v letih 2003-2004 predstavil [John Gruber](https://daringfireball.net/projects/markdown/). Namen markdowna je, da se pisec osredotoča na vsebino, pri tem pa za oblikovanje uporablja nekaj preprostih pravil. Pozneje se markdown lahko prevede v spletno obliko, v pdf (npr. prek LaTeX-a) itd. Bistvena pravila (vezana predvsem na uporabu na Githubu) so [podana tukaj](https://help.github.com/articles/basic-writing-and-formatting-syntax/); spodaj jih bomo na kratko ponovili.

Naslove definiramo z znakom #:

* \# pomeni naslov prve stopnje,
* \#\# pomeni naslov druge stopnje itd*.

\* Če pogledate kodo opazite, da se znak \ pojavi pred znakom \# zato, da se *prikaže* sam znak \# in ne uporabi njegovo funkcijo za naslov prve stopnje. Podobno je tudi pri drugih posebnih znakih.

Oblikovanje besedila definirajo posebni znaki; če želimo besedo (ali več besed) v *poševni* obliki, moramo tako besedilo vstaviti med dva znaka \*: tekst v stanju za urejanje v obliki \*primer\* bi se prikazal kot *primer*.

Pregled simbolov, ki definirajo obliko:

* *poševno*:\*
* **krepko**: \*\*
* ~~prečrtano~~: \~\~
* `simbol`: \`
* ``funkcije``: \`\`
* ```blok kode```: \`\`\`.

Opomba: za primer bloka kode Python glejte uvodno poglavje, primer Fibonacci.

V okviru markdowna lahko uporabljamo tudi LaTeX sintakso. LaTeX je zelo zmogljiv sistem za urejanje besedil; na Fakulteti za strojništvo, Univerze v Ljubljani, so predloge za diplomske naloge pripravljene v [LaTeXu](http://lab.fs.uni-lj.si/ladisk/?what=incfl&flnm=latex.php) in Wordu. Tukaj bomo LaTeX uporabljali samo za pisanje matematičnih izrazov.

Poznamo dva načina:

* vrstični način (angl. *inline*): se začne in konča z znakom \\$, primer: $a^3=\sqrt{365}$,
* blokovni način (angl. *block mode*): se začne in konča z znakoma \\$\\$, primer: 
$$\sum F=m\,\ddot{x}$$

Zgoraj smo že uporabljali sezname, ki jih začnemo z znakom \* v novi vrstici:

* lahko pišemo sezname
     - z več nivoji
         - 3$.$ nivo
     - 2$.$ nivo
* lahko pišemo *poševno*
* lahko pišemo **krepko**.

Če želimo oštevilčene sezname, začnemo z "1." nato pa nadaljujemo z "1.":

1. prvi element
1. drugi
1. naslednji ...

V dokument lahko vključimo tudi *html* sintakso (angl. *Hypertext Markup Language* ali po slovensko jezik za označevanje nadbesedila). Kot html najenostavneje vključimo slike.

Uporabimo:
```html
![Ladisk (to je opcijski opis slike)](./fig/ladisk.png)
```

Za prikaz:
![Ladisk](./fig/ladisk.png)

Vključimo lahko tudi zunanje vire (npr. video posnetek iz Youtube). Ker gre za morebitno varnostno tveganje, tega ne naredimo v celici tipa *markdown* kakor zgoraj, ampak v celici tipa *code*, pri tem pa uporabimo t. i. magični ukaz (angl. *magic*) ``%%html``, ki definira, da bo celoten blok v obliki html sintakse (privzeto so celice tipa *code* Python koda):

```python
%%html 
<iframe width="560" height="315" src="https://www.youtube.com/embed/VLcPd07-gnk" frameborder="0" allowfullscreen></iframe>
```

Magičnih ukazov je v notebooku še veliko (glejte [dokumentacijo](http://ipython.readthedocs.io/en/stable/interactive/magics.html)) in nekatere bomo spoznali pozneje, ko jih bomo potrebovali!

Doslej smo že večkrat uporabili sklicevanje na zunanje vire, to naredimo tako:

``[Ime povezave](naslov do vira)``

Primer: ``[Spletna stran laboratorija Ladisk](www.ladisk.si)`` se oblikuje kot: [Spletna stran laboratorija Ladisk](http:\\www.ladisk.si).

V okviru markdowna lahko pripravimo preproste tabele. Priprava je relativno enostavna: med besede enostavno vstavimo navpične znake | tam, kjer naj bi bila navpična črta. Po naslovni vrstici sledi vrstica, ki definira poravnavo v tabeli. 

Primer: 

```
| Ime   | Masa [kg]  | Višina [cm]  |
|:-|-:|:-:|
| Lakotnik  | 180  | 140  |
| Trdonja | 30  | 70  |
| Zvitorepec  | 40  | 100  |
```

Rezultira v:

| Ime | Masa [kg]  | Višina [cm]  |
|-----|-----------:|:------------:|
| Lakotnik  | 180  | 140  |
| Trdonja | 30  | 70  |
| Zvitorepec  | 40  | 100  |

Opazimo, da dvopičje definira poravnavo teksta v stolpcu (levo `:-`, desno `-:` ali sredinsko `:-:`).

## Prvi program in PEP

Sedaj smo pripravljeni na prvi program napisan v Pythonu:
```python
a = 5.0
print(a)
```
Program lahko zapišemo v poljubnem urejevalniku tekstovnih datotek (npr. *Beležka*/*Notepad*) in ga shranimo v datoteko `ime.py`.

Primer [kode](./moduli/prvi_program.py) v okolju *Visual Studio Code*:
![Prvi program](./fig/vsc_prvi_program.png)

To datoteko nato poženemo v ukazni vrstici (predpostavimo, da je pot do datoteke python.exe dodana v spremenljivke okolja, sicer glejte tale [vir](https://docs.python.org/3/using/windows.html#configuring-python)):
```
python ime.py
>>> 5.0
```
Z znaki `>>>` označimo rezultat, ki ga dobimo v ukazni vrstici.

Zgornji primer izvajanja Python programa predstavlja t. i. *klasični način poganjanja programov* od začetka do konca. Tukaj bomo bolj pogosto uporabljali t. i. *interaktivni način*, ko kodo izvajamo znotraj posamezne celice tipa *code*:

```python
a = 5.0  # to je komentar
print(a) # izpiše vrednost a
```

### PEP - Python Enhancements Proposal

Python ima visoke standarde glede kakovosti in estetike. Že zgodaj v razvoju jezika se je uveljavil princip predlaganja izboljšav prek t. i. *Python Enhancements Proposal* (PEP na kratko).

#### PEP8
Eden od bolj pomembnih je [PEP8](https://www.python.org/dev/peps/pep-0008/), ki definira stil:

* zamik: 4 presledki,
* en presledek pri operatorju `=`, torej: ``a = 5`` in ne ``a=5``,
* spremenljivke in imena funkcij naj bodo opisne, pišemo jih s podčrtajem: ``širina_valja = 5``,
* za razrede uporabljamo t. i. format CamelCase.

* pri aritmetičnih operaterjih postavimo presledke po občutku: ``x = 3*a + 4*b`` ali ``x = 3 * a + 4 * b``,
* za oklepajem in pred zaklepajem ni presledka, prav tako ni presledka pred oklepajem ob klicu funkcije: ``x = sin(x)``, ne pa ``x = sin( x )`` ali ``x = sin (x)``,
* presledek za vejico: ``range(5, 10)`` in ne ``range(5,10)``,
* brez presledkov na koncu vrstice ali v prazni vrstici,

* znotraj funkcije lahko občasno dodamo po *eno* prazno vrstico,
* med funkcije postavimo dve prazni vrstici,
* vsak modul uvozimo (`import`) v ločeni vrstici,
* najprej uvozimo standardne pythonove knjižnice, nato druge knjižnice (npr. numpy) in na koncu lastne.

Opomba: moderna razvojna orodja nam pomagajo pri oblikovanju kode, ki ustreza PEP8 (v VSC pritisnite `ctrl`+`shift`+`P` ter napišite *"Format document"* in pritisnite `enter`)!

#### PEP20

[PEP20](https://www.python.org/dev/peps/pep-0020/) je naslednji pomemben PEP, ki se imenuje tudi *The zen of Python*, torej *zen Pythona*; to so vodila, ki se jih poskušamo držati pri programiranju. Tukaj navedimo samo nekatera vodila:

* Lepo je bolje kot grdo.
* Preprosto je bolje kot zakomplicirano.
* Premočrtno je bolje kot gnezdeno.
* Berljivost je pomembna.
* Posebni primeri niso dovolj posebni, da bi prekršili pravilo
* Če je implementacijo težko pojasniti, je verjetno slaba ideja; če jo je lahko, je mogoče dobra.

## Osnove Pythona

### Osnovni podatkovni tipi in operatorji

#### Dinamično tipiziranje

Python	je	dinamično	tipiziran jezik. Tipi	spremenljivk	se	ne	preverjajo. Posledično nekatere operacije niso mogoče na nekaterih tipih oz. so prilagojene tipu podatka (pozneje bomo na primer pogledali množenje niza črk). Tip se določi pri dodelitvi vrednosti, poglejmo primer:

```python
a = 1 # to je komentar: a je celo število (integer)
a
```

Tip prikažemo z ukazom `type`:

```python
type(a)
```

Vrednost `a` lahko prepišemo z novo vrednostjo:

```python
a = 1.0  # to je sedaj število s plavajočo vejico (float)
a        # če `a` tako zapišemo v zadnji vrstici, se bo izpisala vrednost
```

```python
type(a)
```

Za izpis vrednosti sicer lahko uporabimo tudi funkcijo `print()`, ki jo bomo podrobneje spoznali pozneje:

```python
print(a)
```

Dokumentacijo funkcije lahko kličemo s pritiskom [shift + tab]. Npr: napišemo `pri` in pritisnemo [shift + tab] ter se bo v pojavnem oknu prikazala pomoč; če pridržimo [shift], se z dodatnim pritiskom na [tab] prikaže razširjena pomoč, z nadaljnjim dvojnim pritiskom [tab] (skupaj torej 4-krat) se pomoč prikaže v novem oknu.

Do pomoči dostopamo tudi, tako, da pred funkcijo postavimo `?` (za pomoč) ali `??` (prikaže tudi izvorno kodo, če je ta na voljo).

Primer:
```python
?print
??print
```

#### Logični operatorji

Logični operatorji so ([vir](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not)):
* `or`, primer: `a or b`; opomba: drugi element se preverja samo, če prvi ni res,
* `and`, primer: `a and b`; opomba: drugi element se preverja samo, če prvi je res,
* `not`, primer: `not a`; opomba: ne-logični operatorji imajo prednost: `not a == b` se interpretira kot `not (a == b)`.

#### Primerjalni operatorji

Primerjalni operatorji ([vir](https://docs.python.org/3/library/stdtypes.html#comparisons)):
* `<` manj kot,
* `<=`	manj ali enako,
* `>` večje kot,
* `>=` večje ali enako,
* `==` enako,
* `!=` neenako,
* `is` istost objekta primerjanih operandov,
* `is not` ali operanda nista isti objekt.

Primer:

```python
5 < 6
```

Lahko naredimo niz logičnih operatorjev:

```python
1 < 3 < 6 < 7
```

preverimo istost (ali gre za isti objekt):

```python
a = 1
a == 1
```

Uporabimo še logični operator:

```python
1 < 3 and 6 < 7
```

```python
1 < 3 and not 6 < 7
```

Tukaj velja izpostaviti, da je v logičnih izrazih poleg `False` neresnična tudi vrednost `0`, `None`, prazen seznam (ali niz, terka, ...); **vse ostalo je `True`**.

Poglejmo nekaj primerov:

```python
1 and True
```

```python
0 and True
```

```python
(1,) and True
```

#### Podatkovni tipi za numerične vrednosti

Pogledali si bomo najbolj pogoste podatkovne tipe, ki jih bomo pozneje uporabili ([vir](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)):

* `int` za zapis poljubno velikega celega števila,
* `float` za zapis števil s plavajočo vejico,
* `complex` za zapis števil v kompleksni obliki.

Bolj podrobno bomo podatkovne tipe (natančnost zapisa in podobno) spoznali pri uvodu v numerične metode.

Primeri:

```python
celo_število = -2**3000         # `**` predstavlja potenčni operator
racionalno_število = 3.141592
kompleksno_število = 1 + 3j     # `3j` predstavlja imaginarni del
```

#### Operatorji numeričnih vrednosti

Podatkovni tipi razvrščeni po naraščajoči prioriteti ([vir](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)):

1. `x + y` vsota,	 	 
1. `x - y`razlika,	 	 
1. `x * y` produkt,	 	 
1. `x / y` deljenje,	 	 
1. `x // y` celoštevilsko deljenje (rezultat je celo število zaokroženo navzdol),
1. `x % y` ostanek pri celoštevilskem deljenju,	 
1. `-x` negiranje,	 
1. `+x` nespremenjen, 	 	 
1. `abs(x)` absolutna vrednost,
1. `int(x)` celo število tipa `int`,
1. `float(x)` racionalno število tipa `float`,
1. `complex(re, im)` compleksno število tipa `complex`,
1. `c.conjugate()` kompleksno konjugirano število,	 	 
1. `divmod(x, y)` vrne par `(x // y, x % y)`,
1. `pow(x, y)` vrne `x` na potenco `y`,
1. `x ** y`	vrne `x` na potenco `y`.

Pri aritmetičnih operatorjih velja izpostaviti različico, pri kateri se rezultat priredi operandu ([vir](https://docs.python.org/3/reference/simple_stmts.html#augmented-assignment-statements)). To imenujemo *razširjena dodelitev* (angl. *augmented assignement).

Poglejmo si primer. Namesto:

```python
a = 1
a = a + 1 # prištejemo vrednost 1
a
```

lahko zapišemo:

```python
a = 1
a += 1 
a
```

#### Operatorji na nivoju bitov

Operatorje na nivoju bitov bomo v inženirski praksi redko uporabljali, vseeno jih navedimo po naraščajoči prioriteti ([vir](https://docs.python.org/3/library/stdtypes.html#bitwise-operations-on-integer-types)):

* `x | y` *ali* na nivoju bitov `x` in `y`,
* `x ^ y` *ekskluzivni ali* (samo en, ne oba) na nivoju bitov `x` in `y`,
* `x & y` bitni *in* na nivoju bitov `x` in `y`,	 
* `x << n` premik bitov `x` za `n` bitov levo,
* `x >> n` premik bitov `x` za `n` bitov desno,
* `~x` negiranje bitov `x`.

Poglejmo si primer ([vir](https://www.tutorialspoint.com/python/bitwise_operators_example.htm)):

```python
a = 21  # 21 v bitni obliki: 0001 0101 
b = 9   # 9 v bitni obliki:  0000 1001 

c = a & b; # 0000 0001 v bitni obliki predstavlja število 1 v decimalni
c
```

Prikažimo število v bitni obliki:

```python
bin(c)
```

### Sestavljene podatkovne strukture

#### Niz (string)

Niz je sestavljen iz znakov po standardu *Unicode*. V podrobnosti se ne bomo spuščali; pomembno je navesti, da je Python od verzije 3 naprej naredil velik napredek ([vir](https://docs.python.org/3/howto/unicode.html)) in da lahko poljubne znake po standardu *Unicode* uporabimo tako v tekstu, kakor tudi v programskih datotekah .py.

Niz se začne in zaključi z dvojnim " ali enojnim ' narekovajem.

```python
b = 'tekst' # b je niz (string)
b
```

Pri tem ni težav z imeni, ki smo jih bolj vajeni:

```python
π = 3.14
```

Znak $\pi$ se v notebooku zapiše, da napišemo `\pi` (imena so ponavadi ista kot se uporabljajo pri zapisu v LaTeXu) in nato pritisnemo [tab].

Nekatere operacije nad nizi:

```python
print('kratek ' + b) # seštevanje
print(3 * 'To je množenje niza. ')      # množenje niza
print('Celo število:', int('5') + int('5'))      # pretvorba niza '5' v celo število
print('Število s plavajočo vejico:', float('5') + float('5'))  # pretvorba niza '5' v število s plavajo vejico
```

#### Terka (tuple)

Tuples ([vir](https://docs.python.org/tutorial/datastructures.html#tuples-and-sequences)) ali po slovensko terke so seznami poljubnih **objektov**, ki jih ločimo z vejico in pišemo znotraj okroglih oklepajev:

```python
terka_1 = (1, 'programiranje', 5.0)
```

```python
terka_1
```

Večkrat smo že uporabili besedo *objekt*. Kaj je objekt? Objekte in razrede si bomo podrobneje pogledali pozneje; zaenkrat se zadovoljimo s tem, da je objekt več kot samo *ime* za določeno vrednost; objekt ima tudi metode.
Npr. drugi element je tipa `str`, ki ima tudi metodo `replace` za zamenjavo črk:

```python
terka_1[1].replace('r', '8')
```

Če ima terka samo en objekt, potem jo moramo zaključiti z vejico (sicer se ne loči od objekta v oklepaju):

```python
terka_2 = (1,)
type(terka_2)
```

Terke lahko zapišemo tudi brez okroglih oklepajev:

```python
terka_3 = 1, 'brez oklepajev', 5.
```

```python
terka_3
```

Do objektov v terki dostopamo prek indeksa (**se začnejo z 0**):

```python
terka_1[0]
```

Terke **ni mogoče spreminjati** (angl: *immutable*), elementov ne moremo odstranjevati ali dodajati. Klicanje spodnjega izraza bi vodilo v napako:

```python
terka_1[0] = 3
>>> TypeError: 'tuple' object does not support item assignment
```

#### Seznam (list)

Seznami ([vir](https://docs.python.org/tutorial/datastructures.html#more-on-lists)) se zapišejo podobno kot terke, vendar se uporabijo oglati oklepaji:

```python
seznam = [1., 2, 'd']
```

```python
seznam
```

Za razliko od terk, se sezname lahko spreminja:

```python
seznam[0] = 10
seznam
```

Seznami so spremenljivi (angl. *mutable*) in treba se je zavedati, da **ime** v bistvu **kaže** na mesto v pomnilniku računalnika:

```python
b = seznam
b
```

Sedaj tudi ``b`` kaže na isto mesto v pomnilniku. 

Poglejmo, kaj se zgodi s ``seznam``, če spremenimo element seznama ``b``:

```python
b[0] = 9
b
```

```python
seznam
```

Če želimo narediti kopijo podatkov, potem moramo narediti tako:

```python
b = seznam.copy()
b[0] = 99
b
```

```python
seznam
```

Primer seznama seznamov:

```python
matrika = [[1, 2, 3],
           [4, 5, 6],
           [7, 8, 9]]
```

```python
matrika
```

Več o matrikah bomo izvedeli pozneje.

#### Izbrane operacije nad seznami

Tukaj si bomo pogledali nekatere najbolj pogoste operacije, za več, glejte [dokumentacijo](https://docs.python.org/tutorial/datastructures.html).

Najprej si poglejmo dodajanje elementa:

```python
seznam = [0, 1, 'test']
seznam.append('dodajanje elementa')
seznam
```

Potem vstavljanje na določeno mesto (z indeksom 1, ostali elementi se zamaknejo):

```python
seznam.insert(1,'na drugo mesto')
seznam
```

Potem lahko določeni element odstranimo. To naredimo s `seznam.pop(i)` ali `del seznam[2]`. Prvi način vrne odstranjeni element, drugi samo odstrani element:

```python
odstranjeni_element = seznam.pop(2) # odstani element z indeksom 2 in ga vrne 
## podoben učinek bi imel ukaz: del seznam[2]
print('seznam:', seznam)
print('odstranjeni element:', odstranjeni_element)
```

Z metodo `.index(v)` najdemo prvi element vrednosti `v`:

```python
seznam
```

```python
seznam.index('test')
```

#### Zakaj uporabiti terko in ne seznama?

Terk ne moremo spreminjati, zato je njihova numerična implementacija bistveno bolj lahka (v numeričnem smislu) in spomin se lahko bolje izrabi. Posledično so terke **hitrejše** od seznamov; poleg tega jih ne moretmo po nerodnosti **spremeniti**. Za širši odgovor glejte [vir](http://getpython3.com/diveintopython3/native-datatypes.html#tuples).

#### Množice (Sets)

Množice so v principu podobne *množicam* kot jih poznamo iz matematike. Množice v Pythonu ([dokumentacija](https://docs.python.org/tutorial/datastructures.html#sets)) za razliko od ostalih sestavljenih struktur:

* **ne dovoljujejo podvojenih elementov** in 
* **nimajo urejenega vrstnega reda** (pomembno, npr. za pravilno uporabo v zankah).

Vredno je poudariti, da sta dodajanje in ugotavljanje pripadnosti elementa množici zelo hitri operaciji.

Kreiramo jih z zavitimi oklepaji  ``{}`` ali ukazom ``set()``:

```python
A = {'a', 4, 'a', 2, 'ž', 3, 3}
A
```

Če se želimo v seznamu znebiti podvojenih elementov, je zelo enostaven način, da kreiramo množico iz seznama:

```python
B = set([6, 4, 6, 6])
B
```

Uporabimo lahko tipične matematične operacije nad množicami:

```python
B - A   # elementi v B brez elementov, ki so tudi v A
```

```python
A | B   # elementi v A ali B
```

```python
A & B   # elementi v A in B
```

```python
A ^ B   # elementi v A ali B, vendar ne v obeh
```

#### Slovar (dictionary)

Slovar ([dokumentacija](https://docs.python.org/tutorial/datastructures.html#dictionaries)) lahko definiramo eksplicitno s pari *ključ : vrednost*, ločenimi z vejico znotraj zavitih oklepajev:

```python
parametri = {'višina': 5.2, 'širina': 43, 'g': 9.81}
parametri
```

Do vrednosti (angl. *value*) elementa dostopamo tako, da uporabimo ključ (angl. *key*):

```python
parametri['višina']
```

Pogosteje uporabljamo metode:
* `values()`, ki vrne vrednosti slovarja,
* `keys()`, ki vrne ključe slovarja,
* `items()`, ki seznam terk (ključ, vrednost); to bomo pogosto rabili pri zankah.

Primer:

```python
parametri.items()
```

Slovarje lahko spreminjamo; npr. dodamo element:

```python
parametri['lega_težišča'] = 99999
parametri
```

Elemente odstranimo (podobno kot zgoraj `pop` odstrani in vrne vrednost `del` samo odstrani):

```python
parametri.pop('lega_težišča')
```

```python
parametri
```

#### Operacije nad sestavljenimi podatkovnimi strukturami

Večina sestavljenih podatkovnih struktur dovoljuje sledeče operacije ([dokumentacija](https://docs.python.org/library/stdtypes.html#common-sequence-operations)):


* `x in s` vrne `True`, če je kateri element iz `s` enak `x`, sicer vrne `False`,
* `x not in s` vrne `False`, če je kateri element iz `s` enak `x`, sicer vrne `True`,
* `s + t` sestavi `s` in `t`,
* `s * n` ali `n * s` rezultira v `n`-krat ponovljen `s`,
* `s[i]` vrne `i`-ti element iz `s`,
* `s[i:j]` vrne `s` od elementa `i` (vključno) do `j` (ni vključen),
* `s[i:j:k]` rezanje `s` od elementa `i` do `j`	po koraku `k`,
* `len(s)` vrne dolžino `s`,
* `min(s)` vrne najmanjši element iz `s`,	 
* `max(s)` vrne največji element iz `s`,
* `s.index(x[, i[, j]])` vrne indeks prve pojave `x` v `s` (od indeksa `i` do `j`),
* `s.count(x)` vrne število elementov.

Nekateri primeri:

```python
'a' in 'abc' # in primerja ali je objekt v nizu, seznamu,...
```

Zgoraj smo imeli seznam `parametri`:
```python
{'g': 9.81, 'višina': 5.2, 'širina': 43}
```
Preverimo, ali vsebuje element s ključem `širina`:

```python
'širina' in parametri
```

#### Štetje in rezanje sestavljenih podatkovnih struktur

Predpostavimo spodnji niz:

```python
abeceda = 'abcčdefghijklmnoprsštuvzž'
```

**V Pythonu začnemo šteti indekse z 0**! Na spletu lahko najdete razprave o tem ali je prav, da se indeksi začnejo z 0 ali 1. Zakaj začeti z 0 zelo lepo pojasni prof. dr. Janez Demšar v knjigi Python za programerje ([vir](https://ucilnica.fri.uni-lj.si/file.php/166/Python%20za%20programerje.pdf), stran 28) ali pa tudi prof. dr. Edsger W. Dijkstra v zapisu na [spletu](http://www.cs.utexas.edu/users/EWD/transcriptions/EWD08xx/EWD831.html).

Rezanje imenujemo operacijo, ko iz seznama izrežemo določene člene.

Splošno pravilo je:
```python
 s[i:j:k]
```

kar pomeni, da se seznam `s` razreže od elementa `i` do `j`	po koraku `k`. Če katerega elementa ne podamo potem se uporabi (logična) vrednost:

* `i` je privzeto 0, kar pomeni prvi element,
* `j` je privzeto -1, kar pomeni zadnji element,
* `k` je privzeto 1.

Prvi znak je torej:

```python
abeceda[0]
```

Prvi trije znaki so:

```python
abeceda[:3]
```

Od prvih 15 znakov, vsak tretji je:

```python
abeceda[:15:3]
```

Zadnjih petnajst znakov, vsak tretji:

```python
abeceda[-15::3]
```

Ko želimo obrniti vrstni red, lahko to naredimo tako (beremo od začetka do konca po koraku -1, kar pomeni od konca proti začetku s korakom 1:)):

```python
abeceda[::-1]
```

### Kontrola toka programa

Pogledali si bomo nekatera osnovna orodja za kontrolo toka izvajanja programa ([dokumentacija](https://docs.python.org/tutorial/controlflow.html)).

#### Stavek `if`

Tipična oblika stavka `if` ([dokumentacija](https://docs.python.org/tutorial/controlflow.html#if-statements)):

```python
if pogoj0:
    print('Izpolnjen je bil pogoj0')
elif pogoj1:
    print('Izpolnjen je bil pogoj1')
elif pogoj2:
    print('Izpolnjen je bil pogoj2')
else:
    print('Noben pogoj ni bil izpolnjen')
```

Primer:

```python
a = 6
if a > 5:
    print('a je več od 5') # se izvede, če je pogoj res. 
    print(13*'-')
```

Zgoraj smo funkcijo ``print`` zamaknili, saj pripada bloku kode, ki se izvede v primeru ``a > 0``. Nekateri programski jeziki kodo znotraj bloka dajo v zavite ali kakšne druge oklepaje in dodatno še zamikajo. Pri Pythonu se samo zamikajo. Zamik je tipično **4 presledke**. Lahko je tudi **tabulator** (manj pogosto), vendar pa smemo v eni datoteki .py uporabljati le en način zamikanja.

Stavke lahko zapišemo tudi v eno vrstico, vendar je taka uporaba odsvetovana, saj zmanjšuje preglednost kode:

```python
if a > 5: print('taka oblika je odsvetovana'); print(40*'-'); print('uporaba podpičja namesto nove vrstice')
```

#### Izraz `if`

Poleg stavka ``if`` ima Python tudi izraz ``if`` (angl. *ternary operator*, glejte [dokumentacijo](https://docs.python.org/faq/programming.html#is-there-an-equivalent-of-c-s-ternary-operator)).

Sintaksa je:

```python
[izvedi, če je True] if pogoj else [izvedi, če je False]
```

Preprost primer:

```python
'Je res' if 5==3 else 'Ni res'
```

If izrazi so zelo uporabni in jih bomo pogosto uporabljali pri izpeljevanju seznamov!

#### Zanka `while`

Sintaksa zanke while ([dokumentacija](https://docs.python.org/reference/compound_stmts.html#the-while-statement)) je:

```python
while pogoj:
    [koda za izvajanje]
else:
    [koda za izvajanje]
```

Pri tem je treba poudariti, da je del `else` opcijski in se izvede ob izhodu iz zanke `while`. Ukaz `break` prekine zanko in ne izvede `else` dela. Ukaz `continue` prekine izvajanje trenutne kode in gre takoj na testiranje pogoja.

Preprost primer:

```python
a = 0
while a < 3:
    print(a)
    a += 1
else:
    print('--')
```

#### Zanka `for`

V Pythonu bomo **zelo pogosto** uporabljali zanko `for` ([dokumentacija](https://docs.python.org/3/tutorial/controlflow.html#for-statements)). Sintaksa je:

```python
for element in seznam_z_elementi:
    [koda za izvajanje]
else:
    [koda za izvajanje]
```

Podobno kakor pri `while` je tudi tukaj `else` del opcijski.

Poglejmo primer:

```python
imena = ['Jaka', 'Miki', 'Luka', 'Anja']
for ime in imena: # tukaj se skriva prirejanje; ``ime`` vsakič pridobi novo vrednost
    print(ime)
```

Zelo uporabno je zanke delati čez slovarje:

```python
print('Slovar je:', parametri)
for key, val in parametri.items():
    print(key, '=', val)
```

#### Izpeljevanje seznamov

Pri programiranju se pogosto srečujemo s tem, da moramo izvajati operacije na posameznem elementu seznama. Predhodno smo že spoznali zanko `for`, ki jo lahko uporabimo v takem primeru.

Oglejmo si primer, kjer izračunamo kvadrat števil:

```python
seznam = [1, 2, 3] # izvorni seznam
rezultat = [] # pripravimo prazen seznam v katerega bomo dodajali rezultate
for element in seznam:
    rezultat.append(element**2)
rezultat
```

Mnogo enostavneje lahko isti rezultat dosežemo s t. i. **izpeljevanjem seznamov** (angl. *list comprehensions*, glejte [dokumentacijo](https://docs.python.org/tutorial/datastructures.html#list-comprehensions)).

Zgornji primer bi bil:

```python
seznam = [1, 2, 3] # izvorni seznam
rezultat = [element**2 for element in seznam] # <<< na desni strani = je izpeljevanje seznamov
rezultat
```

Izpeljevanje seznamov predstavlja koda `[element**2 for element in seznam]`, ki v bistvu pove to, kar je napisano: izračunaj kvadrat za vsak element v seznamu. Da je rezultat seznam, nakazujejo oglati oklepaji.

Izpeljevanje seznamov ima sledečo sintakso:
```python
[koda for element in seznam]
```

in ima predvsem dve prednosti:

* **preglednost / kompaktnost** kode in
* malo **hitrejše** izvajanje.

Preglednost/kompaktnost smo lahko že presodili. Kako preverimo hitrost? Najlažje to naredimo s t. i. magičnim ukazom `%%timeit`, torej **meri čas** (glejte [dokumentacijo](http://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit)); spodaj bomo uporabili blokovni magični ukaz (označuje ga `%%`):
```
%%timeit -n1000
```
kjer parameter `-n1000` omejuje merjenje časa na 1000 ponovitev.

Najprej daljši način:

```python
seznam = range(1000)
```

```python
%%timeit -n1000
rezultat = []
for element in seznam:
    rezultat.append(element**2)
```

Nato izpeljevanje seznamov (bolj pregledno in malenkost hitreje):

```python
%%timeit -n1000
rezultat = [element**2 for element in seznam]
```

Poglejmo si še dva uporabna primera:

* uporabo izraza `if` za izračun nove vrednosti,
* uporabo izraza `if`, če se nova vrednost sploh izračuna.

Pri izračunu nove vrednosti izraz `if` vstavimo v del pred `for`:

```python
seznam = [1, 2, 3]
[el+10 if el<2 else el**2 for el in seznam]
```

Pogosto pa za določene elemente izračuna sploh ne želimo izvesti, v tem primeru izraz `if` vstavimo za seznam:

```python
[el**2 for el in seznam if el >1]
```

#### Funkcija `zip`

Ko želimo kombinirati več seznamov v zanki, si pomagamo s funkcijo `zip` ([dokumentacija](https://docs.python.org/library/functions.html#zip)); primer:

```python
x = [1, 2, 3]
y = [10, 20, 30]

for xi, yi in zip(x, y):
    print('Pari so: ', xi, 'in', yi)
```

#### Funkcija `range`

Pogosto bomo uporabljali zanko ``for`` v povezavi s funkcijo ``range`` ([dokumentacija](https://docs.python.org/tutorial/controlflow.html#the-range-function)). Sintaksa je:

```python
range(stop)
range(start, stop[, step])
```
Če torej funkcijo kličemo z enim parametrom, je to `stop`: **do** katere številke naštevamo. V primeru klica z dvema parametroma `start, stop`, naštevamo od, do! Tretji parameter je opcijski in definira korak `step`.

Poglejmo primer:

```python
for i in range(3):
    print(i)
```

#### Funkcija `enumerate`

Zanko `for` pogosto uporabljamo tudi s funkcijo `enumerate`, ki elemente oštevilči ([dokumentacija](https://docs.python.org/library/functions.html#enumerate)).

Poglejmo primer:

```python
for i, ime in enumerate(imena):
    print(i, ime)
```

## Za konec: plonk list :)

Veliko jih je; tukaj je [povezava](http://perso.limsi.fr/pointal/_media/python:cours:mementopython3-english.pdf) do enega.

Še nekaj branja: [automatetheboringstuff.com](https://automatetheboringstuff.com/).

In en twit:

```python
%%html
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">The most important slide from my <a href="https://twitter.com/PyData">@PyData</a> talk. <a href="https://t.co/Hc8j4ZpHV4">pic.twitter.com/Hc8j4ZpHV4</a></p>&mdash; Almar Klein (@almarklein) <a href="https://twitter.com/almarklein/status/708909917684568064">March 13, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
```

### Vključevanje lokalnega video posnetka s transformacijami

```python
from IPython.core.display import HTML
```

```python
HTML('<style>video{\
  -moz-transform:scale(0.75) rotate(-90deg);\
  -webkit-transform:scale(0.75) rotate(-90deg);\
  -o-transform:scale(0.75) rotate(-90deg);\
  -ms-transform:scale(0.75) rotate(-90deg);\
  transform:scale(0.75) rotate(-90deg);\
}</style>')
```

<video controls src="./fig/vodni-top.mkv"/></video>

(Zgornje celice se na github-u ne prikaže pravilno)

---

# Vprašanja za vaje

---

## Niz (string)

**Vprašanje 1**: V obliki niza zapišite svoje ime in priimek. Niz pretvorite v seznam in ga izpišite.

**Vprašanje 2:** Zadnji znak v pripravljenem seznamu dvajsetkrat izpišite.

## Seznam (list), terka (tuple)

**Vprašanje 3:** V Jupyter notebooku pošičite pomoč za funkcijo ``tuple``. Definirajte seznam s petimi različnimi števili s plavajočo vejico in ga pretvorite v terko.

**Vprašanje 4:** Ustvarjeni terki poskusite dodati nov element. Komentirajte, terko pretvorite v seznam...

**Vprašanje 5:** Poiščite indeks najvišjega elementa v seznamu in ta element odstranite. Če je treba poiščite pomoč za funkciji ``max``, ``list.index``.

**Vprašanje 6:** Pripravite nov seznam, tako da seznam iz prejšnje naloge uredite po velikosti in prve (najmanjše) tri shranite v nov seznam.

### Funkcija zip

**Vprašanje 7:** Pripravite seznam treh nizov: `['najmanjši', 'srednji', 'največji']`.

Z uporabo funkcije ``zip`` in ``list`` pripravite seznam terk iz seznama nizov in seznama iz prejšnjega vprašanja.

## Slovarji

**Vprašanje 8:** Iz seznama terk iz prejšnje naloge tvorite slovar. Dodajte mu nov ključ 'negativen' in vanj shranite poljubno negativno število.

## Stavek `if`, `for` in `while` zanke

**Vprašanje 9:** Pripravite seznam vseh lihih deliteljev poljubnega naravnega števila ``N``, večjega od 100, ki ni praštevilo.

**Vprašanje 10:** Za vsa naravna števila ``i``, manjša ali enaka 20, ki jih ni na seznamu deliteljev iz prejšnje naloge, v nov seznam zapišite ostanek pri celoštevilskem deljenju ``N`` z ``i``.

**Vprašanje 11:** Iz zgornjega seznama ostankov odstranite podvojene elemente.

**Vprašanje 12:** Za vse elemente seznama iz prejšnje naloge izpišite par elementa in njegovega indeksa v seznamu, v obliki ``indeks: element``.

**Vprašanje 13:** Z uporabo ``while`` zanke delite število 1000 z 2, tolikokrat, da bo rezultat manjši od $10^{-3}$. 

Izpišite število potrebnih operacij deljenja.
