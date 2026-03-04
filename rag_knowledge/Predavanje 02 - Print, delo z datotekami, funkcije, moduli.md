# Funkcije, izjeme, moduli

## Funkcije

### Funkcija print

`print` predstavlja eno od najbolj pogosto uporabljenih funkcij. Vse podane argumente izpiše, mednje da presledek, na koncu pa gre v novo vrstico.

Poglejmo primer:

```python
print('prvi argument', 'drugi', '=', 5.)
```

Za bolj splošno uporabo glejmo [dokumentacijo](https://docs.python.org/library/functions.html#print):

```python
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False),
```

kjer so argumenti:

* `*objects` predstavlja argumente, ki jih želimo izpisati (celovitost zvezdice bo jasna pri obravnavi *funkcij*, spodaj,
* `sep` predstavlja delilni niz (angl. *sep*arator),
* `end` predstavlja zaključni niz,
* `file` predstavlja *datoteko* izpisa (`sys.stdout` predstavlja *standard output*, v konkretnem primeru to pomeni *zaslon* oz. ukazna vrstica; pozneje bomo to spremenili),
* `flush` v primeru `True` izpis zaključi (sicer lahko zaradi optimizacije ostane v medpolnilniku, angl. *buffer*).

Pri argumentu `end` smo zgoraj uporabili t. i. **izhodni znak** (angl. *escape character*): **\\**. 

Namen izhodnega znaka je, da sledečemu znaku da poseben pomen, nekatere pogoste uporabe so:
* **\\n** vstavi *prelomi vrstico*,
* **\\t** vstavi *tabulator*,
* **\\b** izbriše zadnji znak *backspace*,
* **\\'** izpiše enojni narekovaj ',
* **\\"** izpiše dvojni narekovaj ",
* **\\\\** prikaže *izhodni znak* (angl. *backslash*),
* **\\** nova vrstica v večvrstičnem nizu.

Primer (kako ponavadi ne programiramo):

```python
print('pet','et', sep='-', end='=')
print('p:)')
```

In še primer z izhodnimi znaki:

```python
print('Nizi v Pythonu (po PEP8) naj ne bi bili daljši od 80 znakov. \
Razlog je v tem, da nam ni treba pomikati okna levo in desno. \
Da pa vseeno lahko napišemo nize, ki so daljši od 80 znakov \
Uporabimo enojni delilni znak \\ (bodite pozorni na to, \
kako se izpiše: \\)',)
```

### Oblikovanje nizov

Pri oblikovanju/formatiranju nizov imamo na voljo več orodij:

* **f-niz** (angl. *f-string*, *f* zaradi "formatirani"), [dokumentacija](https://docs.python.org/reference/lexical_analysis.html#f-strings),
* **metoda `.format()`**, [dokumentacija](https://docs.python.org/library/stdtypes.html#str.format),
* **%** oblikovanje, [dokumentacija](https://docs.python.org/library/stdtypes.html#string-formatting-operations).

Poglejmo si primere:

```python
a = 3.14
print(f'pi = {a}')                # f-niz
print('pi = {a}'.format(a = a))   # .format
print('pi = %(a)3.2f' % {'a': a}) # %
```

*f-niz* je implementiran od Python verzije 3.6 naprej ([PEP 498](https://www.python.org/dev/peps/pep-0498/)) in predstavlja najbolj enostaven način oblikovanja. Ostala načina navajamo zgolj zato, da se bralec seznani z njimi in da naredimo povezavo na dokumentacijo.

*f-niz* se vedno začne s **f** pred prvim narekovajem in omogoča zelo splošno oblikovanje (glejte [dokumentacijo](https://docs.python.org/reference/lexical_analysis.html#f-strings)).

Vrednost, ki jo želimo znotraj niza oblikovati, damo v zavite oklepaje:
```python
f'besedilo... {ime_arg1:fmt1} ... besedilo...'
```

V zavitih oklepajih je:
* pred `:` podamo ime vrednosti, ki jo želimo oblikovati
* za `:` oblikujemo izpis spremenljivke; najpogosteje bomo uporabili *fmt* oblike:
    * w.d**f** - predstavitev s plavajočo vejico (**f**loat)
    * w.d**e** - predstavitev z eksponentom (**e**xponent)
    * w.d**g** - splošni format (**g**eneral)
    * w**s** - niz znakov (**s**tring).

``w`` predstavlja (minimalno) skupno širino, ``d`` pa število mest za decimalno piko.

Poglejmo si primer:

```python
višina = 1.84
ime = 'Marko'
tekst = f'{ime} je visok {višina} m ali tudi: {višina:e} m, {višina:7.3f} m.'
tekst
```

Podobno bi lahko naredili z metodo *format* in se sklicali na indeks argumenta:

```python
tekst = '{1} je visok {0} m ali tudi: {0:e} m, {0:7.3f} m.'.format(višina, ime)
tekst
```

Primer z uporabo slovarja:

```python
parametri = {'visina': 5., 'gostota':100.111, 'ime uporabnika': 'Janko'}
```

```python
f'višina = {parametri["visina"]:7.3f}, gostota = {parametri["gostota"]:7.3e}'
```

Oblikovanje s *fmt* (angl. *Format Specification Mini-Language*) je  izredno bogato. Del za dvopičjem je v  splošnem (glejte [dokumentacijo](https://docs.python.org/library/string.html#formatspec)):

```
[[fill]align][sign][#][0][width][grouping_option][.precision][type]
```

kjer so v oglatih oklepajih opcijski parametri (vsi so opcijski):

* `fill` je lahko katerikoli znak,
* `align` označuje poravnavo, možnosti: `"<"` | `">"` | `"="` | `"^"`,
* `sign` označuje predznak, možnosti: `"+"` | `"-"` | `" "`,
* `width` definira minimalno skupno širino,
* `grouping_option` možnosti združevanja, možnosti:  `"_"` | `","`
* `precision` definira število števk po decimalnem ločilu,
* `type` definira, kako naj bodo podatki prikazani, možnosti:  `"b"` | `"c"` | `"d"` | `"e"` | `"E"` | `"f"` | `"F"` | `"g"` | `"G"` | `"n"` | `""o"` | `"s"` | `"x"` | `"X"` | `"%"`

Besedili poravnavamo levo z `"<"`, desno z `">"` in sredinsko `"^"`. Primer sredinske/desne poravnave (širina 20 znakov):

```python
f'{višina:*^20}' # pred ^ je znak s katerim zapolnimo levo in desno stran
```

```python
f'{parametri["visina"]:_>20}'
```

```python
c = 4-2j
f'Kompleksno število {c} je sestavljeno iz realnega ({c.real})\
in imaginarnega ({c.imag}) dela.'
```

Oblikovanje datuma, ure si lahko pogledate v dodatku spodaj.

### Delo z datotekami

Delo z datotekami je pomembno, saj podatke pogosto shranjujemo v datoteke ali jih beremo iz njih. Datoteko, iz katere želimo brati ali vanjo pisati, moramo najprej odpreti. To izvedemo z ukazom `open` ([dokumentacija](https://docs.python.org/library/functions.html#open)):
```python
open(file, mode='r', buffering=-1, encoding=None,
    errors=None, newline=None, closefd=True, opener=None)
```

Od vseh argumentov je nujen samo `file`, ki definira pot do datoteke.

Argument ``mode`` je lahko:

* `'r'` za branje (*read*, privzeto),
* `'w'` za pisanje (*write*, datoteka se najprej pobriše),
* `'x'` za ekskluzivno pisanje; če datoteka že obstaja vrne napako,
* `'a'` za dodajanje na koncu obstoječe datoteke (*append*),
* `'b'` binarna oblika zapisa,
* `'t'` tekstovna oblika zapisa (privzeto),
* `'+'` datoteka se odpre za branje in pisanje.

Zadnji trije zanki (`'b'`, `'t'`, `'+'`) se uporabljajo v kombinaciji s prvimi tremi (`'r'`, `'w'`, `'x'`). Primer: `mode='r+b'` pomeni branje in pisanje binarne datoteke.

Podrobneje bomo spoznali samo branje in pisanje tekstovnih datotek.

Ostali (opcijski) argumenti funkcije `open` so podrobno opisani v [dokumentaciji](https://docs.python.org/library/functions.html#open).

Privzeto je `mode='r'` (oz. `'rt'`, ker je privzet tekstovni način), kar pomeni, da se datoteko odpre za branje v tekstovni obliki. 

Poglejmo si najprej pisanje v (tekstovno) datoteko (`mode='w'`):

```python
datoteka = open('data/prikaz vpisa.txt', mode='w')
```

`'data/prikaz vpisa.txt'` predstavlja relativno pot (glede na mesto, kjer se poganja *Jupyter notebook*) do datoteke.

Zapišimo prvo vrstico s pomočjo metode `write`:

```python
datoteka.write('test prve vrstice\n')
```

Metoda `write` vrne število zapisanih zankov. Če boste v tem trenutku pogledali vsebino datoteke, je velika možnost, da je še prazna. Razlog je v tem, da je vsebina še v medpolnilniku, ki se izprazni na disk samo občasno ali ob zaprtju datoteke (razlog za tako delovanje je v hitrosti zapisa na disk; za podrobnosti glejte dokumentacijo argumenta `buffering` funkcije `open`).

Zapišimo drugo vrstico:

```python
datoteka.write('druga vrstica\n', )
```

Vpišimo sedaj še nekaj številčnih vrednosti:

```python
for d in range(5):
    datoteka.write(f'{d:7.2e}\t {d:9.4e}\n')
```

Datoteko zapremo:

```python
datoteka.close()
```

Sedaj datoteko odpremo za branje in pisanje? `mode='r+'`:

```python
datoteka = open('data/prikaz vpisa.txt', mode='r+')
```

Preverimo vse vrstice s `for` zanko; v spodnji kodi nam `datoteka` v vsakem klicu vrne eno vrstico:

```python
for line in datoteka:
    print(line, end='')
```

Pri uvodu v funkcijo `print` smo omenili argument `file`, ki definira datoteko, v katero se piše (privzet je standardni izhod oz. *zaslon*). Če kot argument `file` posredujemo datoteko, ki je odprta za pisanje:

```python
print('konec', file=datoteka)
datoteka.close()
```

se vsebina zapiše v datoteko. Rezultat lahko preverite v datoteki!

Tukaj je lepa priložnost, da spoznamo stavek `with` ([dokumentacija](https://docs.python.org/reference/compound_stmts.html#with)). Celovitost stavka `with` presega namen tega predmeta, je pa zelo preprosta uporaba pri delu z datotekami, zato si poglejmo primer:

```python
with open('data/prikaz vpisa.txt') as datoteka:
    for line in datoteka:
        print(line, end='')
```

Stavek `with` rezultat funkcije `open` shrani v (*as*) ime `datoteka`, potem pa za vsako vrstico izpišemo vrednosti. Pri izhodu iz stavka `with` se samodejno pokliče metoda `datoteka.close()`. Stavek `with` nam torej omogoča pisanje pregledne in kompaktne kode.

### Funkcije v splošnem

V Pythonu je veliko funkcij že vgrajenih (glejte [dokumentacijo](https://docs.python.org/library/functions.html)); funkcije pa lahko tudi napišemo sami ali jih uvozimo iz t. i. modulov (zgoraj smo uvozili `datetime`, več si bomo pogledali pozneje).

Generični primer funkcije je ([dokumentacija](https://docs.python.org/tutorial/controlflow.html#defining-functions)):
```python
def ime_funkcije(parametri):
    '''docstring'''
    [koda]
    return vsebina
```

Pri tem izpostavimo:
* `def` označuje definicijo funkcije,
* `ime_funkcije` **imena funkcije po PEP8 pišemo z malo, večbesedna povežemo s podčrtajem**,
* `parametri` parametri funkcije, več bomo povedali spodaj,
* `docstring` (opcijsko) dokumentacija funkcije,
* `[koda]` koda funkcije,
* `return` (opcijsko) ukaz za izhod iz funkcije, kar sledi se vrne kot rezultat (če ni `return` ali po return ni ničesar, se vrne `None`)

Primer preproste funkcije:

```python
def površina(dolžina, širina):
    S = dolžina * širina
    return S
```

```python
površina(1, 20)
```

### Posredovanje argumentov v funkcije

Python pozna dva načina posredovanja argumentov:
* glede na **mesto** (*positional*)
* glede na **ime**.

Poglejmo si oba načina na primeru:

```python
površina(3, 6) # pozicijsko
```

```python
površina(širina = 3, dolžina = 6) # glede na ime
```

Priporočeno je, da argumente posredujemo z *imenom*; s tem zmanjšamo možnost napake!

Če ne podamo imena, potem se privzame ime glede na vrstni red argumentov; primer:

```python
površina(3, širina=4)
```

V kolikor mešamo *pozicijsko* in *poimensko* posredovanje argumentov, morajo **najprej biti navedeni pozicijski argumenti, šele nato poimenski**. Pri tem poimenski ne sme ponovno definirati pozicijskega; ker je `dolžina` prvi argument, bi klicanje take kode:
```python
površina(3, dolžina=4)
```
povzročilo napako.

#### Primer dobre prakse pri definiranju argumentov

Pogosto funkcije dopolnjujemo in dodajamo nove argumente ali pa število argumentov funkciji sploh ne moremo vnaprej definirati!

Python s tem nima težav. Argumente, ki niso eksplicitno definirani obvladamo z:

* `*[ime0]` terka `[ime0]` vsebuje vse **pozicijske argumente**, ki niso eksplicitno definirani,
* `**[ime1]` slovar `[ime1]` vsebuje vse **poimenske argumente**, ki niso eksplicitno definirani.

Poglejmo primer:

```python
def neka_funkcija(student, izpit = True):
    ''' Prva verzija funkcije'''
    print(f'Študent {student} se je prijavil na izpit: {izpit}.')
```

```python
neka_funkcija('Janez', izpit=True)
```

Sedaj pa pripravimo drugo, nadgrajeno, verzijo (omogoča enako uporabo kot prva verzija):

```python
def neka_funkcija(*terka, **slovar):
    ''' Druga verzija funkcije '''
    student = terka[0]
    izpit = slovar.get('izpit', True) # True je privzeta vrednost
    izpisi_sliko = slovar.get('izpisi_sliko', False) # False je privzeta vrednost
    
    print(f'Študent {student} se je prijavil na izpit: {izpit}.')
    if izpisi_sliko:
        print('slika:)')
```

```python
neka_funkcija('Janez', izpit=True)
```

```python
neka_funkcija('Janez', izpit=True, izpisi_sliko=True, neobstoječi_argument='ni')
```

#### Posredovanje funkcij kot argument

Tukaj bi želeli izpostaviti, da so argumenti lahko tudi druge funkcije.

Poglejmo si primer:

```python
def oblika_a(vrednost):
    return f'Prikaz vrednosti: {vrednost}'

def oblika_b(vrednost):
    return f'Prikaz vrednosti: {vrednost:3.2f}'

def izpis(oblika, vrednost):
    print(oblika(vrednost))
```

Imamo torej dve funkciji `oblika_a` in `oblika_b`, ki malenkost drugače oblikujeta numerično vrednost.

Poglejmo uporabo:

```python
izpis(oblika_a, 3)
```

```python
izpis(oblika_b, 3)
```

### Docstring funkcije

*docstring* je neobvezen del definicije funkcije, ki dokumentira funkcijo in njeno uporabo. Iz tega stališča je zelo priporočeno, da ga uporabimo. 

[Dokumentacija](https://docs.python.org/tutorial/controlflow.html#documentation-strings) za docstring navaja bistvene elemente:

* prva vrstica naj bo kratek povzetek funkcije,
* če je vrstic več, naj bo druga prazna (da se vizualno loči prvo vrstico od preostalega teksta),
* uporabimo tri narekovaje (dvojne " ali enojne '), ki označujejo večvrstični niz črk.

Primer dokumentirane funkcije:

```python
def površina(dolžina = 1, širina = 10):
    """ Izračun površine pravokotnika
    
    Funkcija vrne vrednost površine.
    
    Argumenti:
    dolžina: dolžina pravokotnika
    širina: dolžina pravokotnika
    """
    return dolžina * širina
```

Primer klica s privzetimi argumenti (med klicem funkcije lahko s pritiskom [shift]+[tab] dostopamo do pomoči):

```python
površina()
```

### Lokalna/globalna imena

Preprosto vodilo pri funkcijah je: **funkcija vidi ven, drugi pa ne vidijo noter**. Funkcija ima notranja imena, ki se ne prekrivajo z istimi imeni v drugih funkcijah.

Poglejmo si primer:

```python
zunanja = 3
def prva():
    notranja = 5
    print(f'Tole je zunanja vrednost: {zunanja}, tole pa notranja: {notranja}')
```

```python
prva()
```

Ker ime `notranja` zunaj funkcije `prva` ni definirano, bi klicanje:
```python
notranja
```
zunaj funkcije vrnilo napako (v funkcijo ne vidimo).

Tukaj velja omeniti, da je **posredovanje zunanjih imen v funkcijo mimo argumentov funkcije odsvetovano**.

### `return`

Ukaz `return` vrne vrednosti iz funkcije. Doslej smo spoznali rezultate v obliki ene spremenljivke; ta spremenljivka pa je lahko tudi terka.

Poglejmo si primer:

```python
def vrnem_terko():
    return 1, 2, 3 # to je ekvivalenten zapis za terko: (1, 2, 3)
```

```python
vrnem_terko()
```

Rezultat funkcije lahko ujamemo tako:

```python
rezultat = vrnem_terko()
rezultat
```

Ali tako, da vrednosti razpakiramo (to sicer velja v splošnem za terke):

```python
i1, i2, i3 = vrnem_terko()
i1 # izpis samo ene vrednosti
```

### Anonimna funkcija/izraz

Anonimna funkcija (glejte [dokumentacijo](https://docs.python.org/tutorial/controlflow.html#lambda-expressions)) je funkcija, kateri ne damo imena (zato je *anonimna*), ampak jo definiramo z ukazom `lambda`. Tipična uporaba je:

```python
lambda [seznam parametrov]: izraz
```

`lambda` funkcijo si bomo pogledali na primeru razvrščanja:

```python
seznam = [-4, 3, -8, 6]
```

od najmanjše do največje vrednosti. To lahko izvedemo z vgrajeno metodo `sort` ([dokumentacija](https://docs.python.org/3/library/stdtypes.html#list.sort)):

```python
sort(*, key=None, reverse=False)
```

Metoda ima dva opcijska argumenta:

* `key` ključ, po katerem razvrstimo elemente
* `reverse`, če je `False`, se med vrednostmi ključa uporabi `<` sicer `>`.

Opomba: `list.sort()` izvede razvrščanje na obstoječem seznamu in ne vrne seznama. Funkcija `sorted()` (glejte [dokumentacijo](https://docs.python.org/3/library/functions.html#sorted)) pa vrne seznam urejenih vrednosti.

Poglejmo primer:

```python
seznam = [-4, 3, -8, 6]
seznam.sort()
seznam
```

Sedaj uporabimo ključ, kjer se bo za primerjavo vrednosti razvrščanja uporabila kvadratna vrednost:

```python
seznam.sort(key = lambda a: a**2)
seznam
```

## Obravnavanje izjem

Pri programiranju se relativno pogosto pojavijo izjeme; na primer: deljenje z nič. 
```python
1/0

---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-50-05c9758a9c21> in <module>()
----> 1 1/0

ZeroDivisionError: division by zero
```

V primeru izjeme Python opozori na to in prekine izvajanje. Ni dobro, da se program prekine ali zruši; iz tega razloga bi zgornjo situacijo programer začetnik verjetno reševal s stavkom `if`. Takšen pristop pa ni preveč uporaben niti splošen in zato je v večini modernih programskih jezikih uveljavljeno obravnavanje izjem.

Pogledali si bomo nekatere osnove, ki bodo študentom predvsem olajšale branje in razumevanje kod drugih avtorjev.

Izjeme obvladujemo s stavkom `try`:
```python
try:
    [koda0]
except:
    [koda1]
finally:
    [koda2]
```

V stavki `try` se `except` izvede, če se v kodi `[koda0]` pojavi izjema, `finally` je opcijski in  se izvede v vsakem primeru ob izhodu iz stavka `try`.

Poglejmo primer, ko enostavno nadaljujemo z izvajanjem:

```python
try:
    1/0
except:
    pass
```

Ukaz `pass` uporabimo, ko sintaksa zahteva stavek (kodo) za izvajanje, vendar ne želimo nobenega ukrepa (glejte [dokumentacijo](https://docs.python.org/tutorial/controlflow.html#pass-statements)).

S pomočjo `except` lahko lovimo tudi različne izjeme, to naredimo tako:

```python
try:
    1/0 
except ZeroDivisionError:
    print('Deljenje z ničlo!') # ali kakšna druga akcija
except:
    print('Nepričakovana napaka.') # odsvetovano; dobro je predvideti napako!
```

Sedaj uporabimo isto kodo za primer deljenja števila z nizom:

```python
try:
    1/'to pa ne gre'
except ZeroDivisionError:
    print('Deljenje z ničlo!') # ali kakšna druga akcija
except:
    print('Nepričakovana napaka.') # odsvetovano; dobro je predvideti napako!
```

### Proženje izjem

Izjeme pa lahko tudi sami prožimo; to naredimo z ukazom `raise` ([dokumentacija](https://docs.python.org/tutorial/errors.html#raising-exceptions)). 

Primer proženja napake je:
```python
raise Exception('Opis izjeme')
```

preprost, vendar celovit, primer, ko bi pričakovali, da je `ime_osebe` tipa `str`:

```python
ime_osebe = 1
try:
    if type(ime_osebe) != str:
        raise Exception(f'Ime ni pričakovanega tipa `str`.')
except Exception as izjema:
    print(izjema)
```

## Osnove modulov

Z moduli lahko na enostaven način uporabimo že napisano kodo. Preprosto povedano, moduli nam v Pythonu omogočajo relativno enostavno ohranjanje preglednosti in reda. Ponavadi v modul zapakiramo neko zaključeno funkcijo, skupino funkcij ali npr. objekt (pozneje bomo spoznali, kaj je objekt).

Za podroben opis uvažanja modulov glejte [dokumentacijo](https://docs.python.org/reference/import.html#the-import-system).

V mapi `moduli` se nahaja datoteka `prvi_modul.py`, ki ima sledečo vsebino:
```python
def kvadrat(x=1):
    return x**2
```

**Pomembno**: pogosto module hierarhično razporejamo po direktorijih. **Samo** če je v določenem direktoriju datoteka (lahko tudi prazna) **\_\_init\_\_.py**, potem se tak direktorij obnaša kot modul. V zgornjem primeru taka datoteka obstaja in posledično se mapa `moduli` obnaša kot *modul*, v tem modulu je pa *podmodul* `prvi_modul` (datoteka pa je `prvi_modul.py`).

Module tipično uvažamo na dva načina:

**Prvi način**:
```python
from moduli import prvi_modul
```
*v tem primeru uvozimo imenski prostor `prvi_modul`*; to pomeni, da funkcijo v `kvadrat()` kličemo tako: `prvi_modul.kvadrat()`.

**Drugi način uvažanja modula**:
```python
import moduli
```
Uvozimo *imenski prostor `moduli`*; to pomeni, da funkcijo `kvadrat()` kličemo tako: `moduli.prvi_modul.kvadrat()`.

V kolikor želimo uvoziti funkcije v modulu `prvi_modul`, to naredimo tako:

```python
from moduli import prvi_modul
```

Sedaj lahko funkcijo `kvadrat` kličemo tako:

```python
prvi_modul.kvadrat(5)
```

Z ukazom `from` lahko uvozimo samo del modula. Primer:

```python
from moduli.prvi_modul import kvadrat
```

Sedaj kličemo neposredno funkcijo `kvadrat`:

```python
kvadrat(6)
```

Funkcije ali module lahko tudi preimenujemo. Primer:

```python
from moduli.prvi_modul import kvadrat as sqr
sqr(7)
```

**Kje Python išče module?**

1. V trenutni mapi (to je tista, v kateri ste prožili `jupyter notebook`),
1. v mapah, definiranih v `PYTHONPATH`,
1. v mapah, kjer je nameščen Python (poddirektorij `site-packages`).

### Uvoz modulov in Jupyter notebook

V tej knjigi bomo, zaradi pedagoških razlogov, module vedno uvažali takrat, ko jih bomo prvič potrebovali. **Pravila dobre prakse in pregledne kode priporočajo, da se uvoze vseh modulov izvede na vrhu Jupyter notebooka!**

### Modul ``pickle``

Modul `pickle` bomo velikokrat uporabljali za zelo hitro in kompaktno shranjevanje in branje podatkov.
`pickle` vrednosti spremenljivke shrani v taki obliki, kot so zapisane v spominu. V primeru racionalnih števil tako  **ne izgublja na natančnosti** (zapis v tekstovno obliko izgublja natančnost, saj definiramo število izpisanih števk, ki pa je lahko manjše od števila decimalnih mest v spominu). Poleg tega pa je zapis v **spominu ponavadi manj zahteven** kakor zapis v tekstovni obliki. Ker vrednosti ni treba pretvarjati v/iz tekstovno/e obliko/e, je pisanje/branje tudi **zelo hitro**! Tukaj si bomo pogledali osnovno uporabo, celovito je `pickle` opisan v  [dokumentaciji](https://docs.python.org/library/pickle.html).

`pickle` ima dve pomembni metodi:

* `dump(obj, file, protocol=None, *, fix_imports=True)` za shranjevanje objekta `obj`,
* `load(file, *, fix_imports=True, encoding="ASCII", errors="strict")` za brane datoteke `file`.

Uporabo si bomo pogledali na primeru. Najprej uvozimo modul:

```python
import pickle
```

Zapišimo vrednosti slovarja `a` v datoteko `data/stanje1.pkl`:

```python
a = {'število': 1, 'velikost': 10}

with open('data/stanje1.pkl', 'wb') as datoteka:
    pickle.dump(a, datoteka, protocol=-1) # pazite: uporabite protocol=-1, da boste uporabili zadnjo verzijo
```

Opazimo, da so datoteko odprli za pisanje v binarni obliki `wb`. Pomembno je tudi, da uporabimo zadnjo verzijo protokola (parameter `protocol`); privzeta ni optimirana za hitrost in ne zapiše v binarni obliki.

Sedaj vrednosti preberimo nazaj v `b` in ga prikažemo:

```python
with open('data/stanje1.pkl', 'rb') as datoteka:
    b = pickle.load(datoteka)

b
```

Zgoraj smo trdili, da je pristop zelo hiter; preverimo to.

Najprej pripravimo podatke (seznam 50000 vrednosti tipa `ind`):

```python
data = list(range(50000))
```

Pisanje v tekstovni obliki

```python
%%timeit
datoteka = open('data/data.txt', mode='w')
for d in data:
    datoteka.write(f'{d:7.2e}\n' )
datoteka.close()
```

Pisanje s pomočjo `pickle`:

```python
%%timeit
with open('data/data.pkl', 'wb') as datoteka:
    pickle.dump(data, datoteka, protocol=-1)
```

Gre skoraj za 20 kratno pohitritev!

## Uporaba modulov iz spleta

Poleg vgrajenih modulov in tistih, ki jih pripravimo sami, obstaja še velika množica modulov, ki jih lahko najdemo na spletu in Pythonu dodajajo nove funkcionalnosti. Šele ti moduli naredijo ekosistem Pythona tako uporaben.

Modul je tehnično gledano ena datoteka, kadar nek večji modul vsebuje več modulov, pa lahko začnemo govoriti o *paketih*. Najbolj pomembne pakete na področju inženirskih ved bomo spoznali v okviru te knjige.

Module oz. pakete lahko *posodabljamo* ali nameščamo *nove*, pri tem nam pomagajo t. i. *upravljalniki paketov* (angl. *package manager*). Najbolj pogost upravljalnik paketov je `pip` ([dokumentacija](https://pip.pypa.io/en/stable/)).

### Upravljalnik paketov `pip`

`pip` je upravljalnik paketov z dolgo zgodovino in podpira veliko število paketov ([pypi.org](https://pypi.org)).

Že nameščene pakete najdemo z ukazom v ukazni vrstici ([dokumentacija](https://pip.pypa.io/en/stable/reference/pip_list/)):
```
pip list
```

Namesto izhoda v ukazno vrstico bomo večkrat uporabljali možnost *Jupyter notebooka*, ko s pomočjo klicaja (!) v celici s kodo izvedemo ukaz v ukazni vrstici  (gre za kratko obliko magičnega ukaza `%sx` - shell execute, glejte [dokumentacijo](http://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-sx)). Opomba: dva klicaja bi vrnila celoten rezultat neposredno v notebook (ker je izpis zelo dolg, smo se temu izognili).

Poglejmo primer:

```python
rezultat = !pip list
rezultat[:5]
```

Pakete namestimo z ukazom (v ukazni vrstici):
```python
pip install [ime_paketa]
```

posodobimo z:
```python
pip install [ime_paketa] --upgrade
```

in odstranimo z:
```python
pip uninstall [ime_paketa]
```

Za več glejte [dokumentacijo](https://pip.pypa.io/en/stable/getting-started/).

#### Primer namestitve paketa

Sicer pa pakete najpogosteje najdemo na spletu ([pypi.org](https://pypi.org)). Pojdite na omenjeni portal in poiščite pakete na temo snemanja posnetkov iz portala [www.youtube.com](https://www.youtube.com).  Z iskanjem "pytube" najdemo obetaven paket, ki ga namestimo:

Sedaj modul uporabimo (za podrobnosti uporabe glejte [dokumentacijo](https://github.com/pytube/pytube)). Najprej uvozimo celotni paket:

```python
import pytube
yt = pytube.YouTube('https://www.youtube.com/watch?v=P4IrAXbpeyI') # kreiramo instanco objekta (kaj to točno pomeni, spoznamo pozneje)
yt.streams.get_highest_resolution().download() #Prenesemo video
```

# Dodatno
## Oblikovanje datuma in časa

Najprej uvozimo modul za delo z datumom in časom `datetime` in prikažemo trenutni čas z datumom (uporabimo funkcijo [`now()`](https://docs.python.org/3/library/datetime.html#datetime.datetime.now)):

```python
import datetime
sedaj = datetime.datetime.now()
sedaj
```

Podatek o času oblikujemo:

```python
f'{sedaj:%Y-%m-%d %H:%M:%S}'
```

Oblikovanje datuma in ure sledi metodam:

* `strftime` ([dokumentacija](https://docs.python.org/library/datetime.html#datetime.datetime.strftime)) se uporablja za pretvorbo iz `datetime` v niz črk,
* `strptime` ([dokumentacija](https://docs.python.org/library/datetime.html#datetime.datetime.strptime)) se uporablja za pretvorbo iz niza črk v tip `datetime`.

Kako izpisati ustrezno obliko (torej kako uporabiti znake `%Y`, `%m` itd.) je prav tako podrobno pojasnjeno v [dokumentaciji](https://docs.python.org/library/datetime.html#strftime-strptime-behavior).

Poglejmo si primer pretvorbe tipa `datetime` v niz (glede na zgoraj smo dodali še %a, da se izpiše ime dneva):

```python
sedaj.strftime('%Y-%m-%d %H:%M:%S %a')
```

Sedaj v obratno smer, torej iz niza v `datetime`:

```python
datetime.datetime.strptime('2017-09-28 05:31:45', '%Y-%m-%d %H:%M:%S')
```

### Nekateri moduli

1. ``openpyxl`` za pisanje in branje Excel xlsx/xlsm datotek: [vir](http://openpyxl.readthedocs.org/en/latest/index.html#),
* Regularni izrazi (zelo močno orodje za iskanje po nizih): [vir](https://docs.python.org/library/re.html).

### Modul ``sys``

Python ima veliko število vgrajenih modulov (in še veliko se jih lahko prenese s spleta, npr. [tukaj](https://pypi.org/)), ki dodajo različne funkcionalnosti. 

Modul ``sys`` omogoča dostop do objektov, ki jih uporablja interpreter.

Poglejmo si primer; najprej uvozimo modul:

```python
import sys
```

Mape, kjer se iščejo moduli, lahko preverimo z ukazom `sys.path` ([dokumentacija](https://docs.python.org/2/library/sys.html#sys.path)):

```python
sys.path
```

### Modul ``os``

Modul ``os`` je namenjen delu z operacijskim sistemom in skrbi za združljivost z različnimi operacijskimi sistemi (če npr. napišete program na sistemu *Windows*, bo delal tudi na sistemu *linux*).

Uvozimo modul:

```python
import os
```

Pogljemo trenutno mapo:

```python
os.path.curdir
```

ali trenutno mapo v absolutni obliki:

```python
os.path.abspath(os.path.curdir)
```

Kako najdemo vse datoteke in mape v direktoriju?

```python
seznam = os.listdir()
```

Prikažimo prve štiri s končnico `ipynb`:

```python
i = 0
for ime in seznam:
    if os.path.isfile(ime):
        if ime[-5:] == 'ipynb':
            print(f'Našel sem datoteko: {ime:s}')
            i +=1
        if i>3:
            break
```

Za več glejte [dokumentacijo](https://docs.python.org/3.7/library/os.path.html#module-os.path).

---

# Vprašanja za vaje

---

## Funkcija print, formatiranje nizov

**Vprašanje 1:** V poljubno poimenovan niz zapišite svoje ime in priimek. Z uporabo funkcije ``print`` in f-nizov izpišite poljuben tekst, v katerega vključite tudi pripravljen niz.

**Vprašanje 2:** Z vsa naravna števila od 1 do 10 v ``for`` zanki izpišite ``'10 / <število> = <rezultat deljenja>'``. Uporabite funkijo ``print`` in f-nize! Prikažite različne možnosti formatiranja.

**Vprašanje 3:** Z uporabo funkcije ``print`` in f-nizov podatke iz slovarja zapišite v tabelarični obliki,  ``'<ključ> \t <vrednost>'`` (namig: ``slovar.items()``).

```python
podatki = {'a': 0, 'F': 1013, 'k': 0.19, 'H': 55, 'l': 19.22, 'n': 7, 'v': 77.6}
```

## Funkcije

**Vprašanje 4**: Definirajte funkcijo, ki sprejme več nepoimenovanih argumentov in vrne njihovo vsoto.

**Vprašanje 5**: Definirajte funkcijo, ki sprejme več poimenovanih argumentov. Če ji kot argumenta posredujete maso `m` in hitrost `v`, naj vrne kinetično energijo, $\frac{1}{2}m~v^2$.

## Delo z datotekami

**Vprašanje 6:** Po vzoru ene od zgornjih nalog podatke iz slovarja v tabelarini obliki zapišite v tekstovno datoteko.

```python
pot_do_datoteke = './/data//tabela.txt'
podatki
```

**Vprašanje 7:** Preberite prej zapisano tekstovno datoteko, vse vrstice shranite seznam.

```python
pot_do_datoteke
```

**Vprašanje 8:** Definirajte funkcijo ``preberi_vrstice``, ki prebere poljubno tekstovno datoteko in vrne seznam vseh njenih vrstic. Njen argument naj bo pot do datoteke.

### Izpeljevanje seznamov, terk, slovarjev...

**Vprašanje 9:** Številčne vrednosti podatkov, ki so v zgornjem seznamu zapisane v obliki nizov, pretvorite v števila s plavajočo vejico. Uporabite izpeljevanje seznamov.

-------

### Kontrola napak in izjem

**Vprašanje 10:** Za vse vrednosti ``i`` v ``range(-10,10)`` v seznam zapišite rezultat ``10 / i``. Uporabite izpeljevanje seznamov. Lovite in izpisujte izjeme.

**Vprašanje 11** Funkcijo `preberi_vrstice` iz ene od prejšnjih nalog, ki prebere tekstovno datoteko in vrne seznam vseh njenih vrstic, nadgradite tako, da bo v primeru neustrezno podane poti ulovila in izpisala napako.

-------

## Moduli

**Vprašnje 12:** Funkcijo iz prejšnje naloge shranite v mapo ``moj_modul``, v modul ``moja_funkcija.py``. Modul uvozite in funkcijo uporabite.

```python
from moj_modul.moja_funkcija import preberi_vrstice as pv_iz_modula
```
