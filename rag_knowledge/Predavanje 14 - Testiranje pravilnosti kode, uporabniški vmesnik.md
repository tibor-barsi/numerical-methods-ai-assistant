# Testiranje pravilnosti kode, uporabniški vmesnik

## Testiranje pravilnosti kode

Kadar razvijamo bolj obsežno kodo in pri tem sodeluje več oseb, se pojavi potreba po avtomatskem testiranju pravilnosti kode. To pomeni, da poleg kode, ki ima določen rezultat, definiramo tudi testirne funkcije; slednje preverjajo ali prve vračajo pričakovani rezultat.

Preprosti primer je:

```python
## vsebina datoteke test_prvi.py (mapa moduli)
def kvadrat(x):
    return x**2 + 1 # očitna napaka glede na ime funkcije

def test_kvadrat():
    assert kvadrat(2) == 4
```

Če poženemo funkcijo `test_kvadrat()` se testira pravilnost `kvadrat(2) == 4`; v primeru, da je rezultat `True` se ne zgodi nič, če pa je rezultat `False` se sproži `AssertionError()` (glejte [dokumentacijo](https://docs.python.org/reference/simple_stmts.html#assert) za ukaz `assert`). Ker funkcija `kvadrat()` vrne `x**2+1`, bo testiranje neuspešno. 

Verjetno se sprašujete v čem je smisel takega *testiranja pravilnosti*. Ko koda postaja obsežna in na njej dela veliko oseb, postane nujno tudi prepletena. Tako se lahko zgodi, da razvijalec doda novo funkcionalnost in nehote poruši obstoječo. Če so vse funkcionalnosti testirane, bo testiranje zaznalo neustreznost predlagane spremembe.

Večina paketov ima tako dodano testiranje pravilnosti; za primer si lahko pogledate izvorno kodo paketa *NumPy*, ki se nahaja na [githubu](https://github.com/numpy/numpy/tree/master/numpy/); če pogledate [vsebino](https://github.com/numpy/numpy/tree/master/numpy/linalg) podmodula `numpy.linalg` za linearno algebro:

![Numpy linalg](./fig/github_numpy_linalg.png)

opazimo podmapo `tests`. Slednja je v celoti namenjena testiranju in vsebuje množico Python datotek, ki preverjajo ustreznost podmodula.

Obstaja več možnosti testiranja kode, pogosto se uporabljajo sledeče:

* [`unittest`](https://docs.python.org/library/unittest.html) (je vgrajen v Python),
* [`nose`](http://nose.readthedocs.io/en/latest/),
* [`pytest`](https://docs.pytest.org/).

Vsi trije pristopi imajo podobno funkcionalnost. V zadnjem obdobju pa se najpogosteje uporablja `pytest` (npr. NumPy/SciPy), katerega osnove si bomo tukaj pogledali.

### pytest

Modul `pytest` namestimo z ukazom `pip`.

Nekatere lastnosti:

* vrne opis testa, ki ni bil uspešen,
* samodejno iskanje testnih modulov in funkcij,
* lahko vključuje `unittest` in `nose` teste.

Če v ukazni vrstici poženete `pytest`, bo program sam poiskal trenutno mapo in vse podmape za datoteke oblike `test_*.py` ali `*_test.py` (napredno iskanje je navedeno v [dokumentaciji](https://docs.pytest.org/en/latest/goodpractices.html#test-discovery)).

Če v ukazni vrstici poženemo ukaz:
```
pytest
```
bomo dobili tako poročilo:

```
============================= test session starts =============================
platform win32 -- Python 3.6.2, pytest-3.2.1, py-1.4.34, pluggy-0.4.0
rootdir: c:\pypinm\moduli, inifile:
collected 4 items

test_orodja.py ...
test_prvi.py F

================================== FAILURES ===================================
________________________________ test_kvadrat _________________________________

    def test_kvadrat():
>       assert kvadrat(2) == 4
E       assert 5 == 4
E        +  where 5 = kvadrat(2)

test_prvi.py:5: AssertionError
===================== 1 failed, 3 passed in 0.65 seconds ======================
```

Iz poročila vidimo, da je program našel dve datoteki (`test_orodja.py` in `test_prvi.py`) in da je prišlo do napake pri eni funkciji (`test_kvadrat`), tri funkcije pa so uspešno prestale test.

Za osnovno uporabo pri numeričnih izračunih je treba še izpostaviti modul `numpy.testing` ([dokumentacija](http://docs.scipy.org/doc/numpy/reference/routines.testing.html#)), ki nudi podporo za testiranje numeričnih polj. 

Izbrane funkcije so:

* `assert_allclose(dejansko, pričakovano[, rtol, ...])` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.testing.assert_allclose.html#numpy.testing.assert_allclose)) preveri enakost do zahtevane natančnosti,
* `assert_array_less(x, y[, err_msg, verbose])` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.testing.assert_array_less.html#numpy.testing.assert_array_less)) preveri, ali so elementi `x` manjši od elementov `y`,
* `assert_string_equal(dejansko, pričakovano)` ([dokumentacija](https://docs.scipy.org/doc/numpy/reference/generated/numpy.testing.assert_string_equal.html#numpy.testing.assert_string_equal)) preveri enakost niza.

#### Zgled: `test_orodja.py`:

V datoteki `test_orodja.py` so pripravljene funkcije za testiranje modula `orodja.py`. 

Tako so najprej pripravljeni podatki:
```python
zacetna = np.asarray([[1, 2, 3],
                      [4, 5, 6],
                      [7, 8, 9]])
zamenjana_0_1_stolpca = np.asarray([[2, 1, 3],
                                    [5, 4, 6],
                                    [8, 7, 9]])
```

Potem je definirana funkcija, ki kliče funkcijo `orodja.zamenjaj_stolpca()` in rezultat primerja s pričakovanim `zamenjana_0_1_stolpca`:

```python
def test_stolpec():
    a = zacetna.copy() # naredimo kopijo podatkov
    b = orodja.zamenjaj_stolpca(a, 0, 1) # b (in tudi a) imata zamenjane stolpce
    np.testing.assert_allclose(b, zamenjana_0_1_stolpca)
```

Če funkcija `orodja.zamenjaj_stolpca()` deluje pravilno, se klicanje `pytest` v ukazni vrstici uspešno zaključi. Za preostale teste poglejte datoteko `test_orodja.py` in [dokumentacijo](https://docs.pytest.org/) paketa `pytest`.

## Grafični uporabniški vmesnik

Uporabniški vmesnik se uporablja za interakcijo uporabnika s programsko kodo. Uporabniški vmesniki so lahko preko ukazne vrstice ali grafični. Za preproste uporabniške vmesnike preko ukazne vrstice je najbolje, da uporabimo kar [`argparse`](https://docs.python.org/3/library/argparse.html), sicer pa se bomo tukaj predvsem osredotočili na **grafični uporabniški vmesnik**.

### Grafični uporabniški vmesnik znotraj brskalnika
V zadnjem obdobju je opaziti veliko napora pri podpori razvoja grafičnih vmesnikov znotraj okolja *Jupyter notebook* ali *Jupyter lab*. Takšen pristop ima predvsem to prednost, da je nadgradnja v obliki spletne aplikacije relativno enostavna, glejte npr.: [Dashboarding with Jupyter Notebooks, Voila and Widgets | SciPy 2019 | M. Breddels and M. Renou](https://www.youtube.com/watch?v=VtchVpoSdoQ).

### Grafični uporabniški vmesnik znotraj operacijskega sistema
v okviru tega poglavja si bomo pogledali klasične uporabniške vmesnike, ki jih poganjamo v okviru določenega operacijskega sistema. Tudi za programiranje grafičnega uporabniškega vmesnika obstaja veliko različnih načinov/modulov. Nekaj najpogosteje uporabljenih:

* [PyQt](https://riverbankcomputing.com/software/pyqt/intro) temelji na [Qt](https://www.qt.io/), ki predstavlja najbolj razširjeno platforma za uporabniške vmesnike,
* [PySide](https://wiki.qt.io/PySide) podobno kakor *PyQt*, vendar s širšo licenco [LGPL](http://qt-project.org/wiki/PySide), a žal tudi slabšo podporo (tukaj je dobra knjiga: [PySide GUI Application Development - SE](https://www.packtpub.com/application-development/pyside-gui-application-development-second-edition)),
* [Kivy](http://kivy.org/) za hiter razvoj modernih uporabniških vmesnikov (ni tako zrel kakor npr. Qt),
* [wxWidgets](https://www.wxwidgets.org/) široko uporabljena prosta platforma za izdelavo uporabniških vmesnikov.

Med vsemi naštetimi si bomo podrobneje pogledali *PyQt*, ki je verjetno najboljša in tudi najbolj dozorela izbira (za komercialno uporabo pa žal ni brezplačna). 

Velja omeniti, da uporabniški vmesnik lahko:

* *rišemo* s [QtDesignerjem](http://doc.qt.io/qt-5/qtdesigner-manual.html) (glejte [video](https://www.youtube.com/watch?v=GLqrzLIIW2E)), ali 
* *kodiramo*.

Tukaj bomo uporabniški vmesnik *kodirali*.

### Zgled

Najprej uvozimo paket za interakcijo in z grafičnimi objekti:

```python
from PyQt6 import QtCore     # za interakcijo 
from PyQt6 import QtWidgets  # grafični objekti
```

Potrebovali bomo tudi modul `sys` za poganjanje programa:

```python
import sys
```

Uporabniški vmesnik tipično gradimo na razredu `QtWidgets.QMainWindow` ([dokumentacija](http://doc.qt.io/qt-5/qmainwindow.html)), ki ima  spodaj prikazano strukturo:

![Window layout](http://doc.qt.io/qt-5/images/mainwindowlayout.png)

Bistveni grafični elementi, ki jih pri takem uporabniškem vmesniku uporabimo, so:

* *Menu Bar*,
* *Toolbars*,
* *Dock Widgets*,
* *Central Widget*,
* *Status bar*.

Ni potrebno definirati vseh; spodaj si bomo pogledali primer, ko bomo definirali *Status bar*, *Central Widget* in *Menu Bar*. Ponavadi vse elemente definiramo pri inicializaciji instance razreda (metoda `__init__`). 

Tukaj izpostavimo, da grafični vmesnik temelji na t. i. *Widgetih*, (za na primer gumb, tabelo, datum itd.). Prikaz nekaterih možnosti je prikazan v:

* [Qt6 galeriji](https://doc.qt.io/qt-6/gallery.html).

Zelo enostaven uporabniški vmesnik (z vrstičnimi komentarji kode) je prikazan spodaj.

```python
import sys
from PyQt6 import QtCore
from PyQt6 import QtGui
from PyQt6 import QtWidgets

class GlavnOkno(QtWidgets.QMainWindow):
    """ Glavno okno podeduje `QtWidgets.QMainWindow`
    """

    def __init__(self):
        """ Konstruktor GlavnoOkno objekta
        """
        QtWidgets.QMainWindow.__init__(self) # konstruktor starša
        self.setWindowTitle('Naslovno okno programa')
 
        # status bar
        self.moj_status_bar = QtWidgets.QStatusBar() # novi widget
        self.moj_status_bar.showMessage('Ta tekst po 10s izgine', 10000)
        self.setStatusBar(self.moj_status_bar) # tukaj se self.moj_status_bar priredi predpripravljenemu v QMainWindow

        # central widget
        self.gumb1 = QtWidgets.QPushButton('Gumb 1') # Gumb z napisom 'Gumb 1'
        self.gumb1.pressed.connect(self.akcija_pri_pritisku_gumba1) # povežemo pritisk s funkcijo za akcijo
        self.setCentralWidget(self.gumb1) # dodamo gumb v `CentralWidget`
        
        # menuji
        self.moj_izhod_akcija = QtGui.QAction('&Izhod', 
                                             self, shortcut=QtGui.QKeySequence.StandardKey.Close,
                                             statusTip="Izhod iz programa",
                                             triggered=self.close)
        self.moj_datoteka_menu = self.menuBar().addMenu('&Datoteka') # menu 'Datoteka'
        self.moj_datoteka_menu.addAction(self.moj_izhod_akcija) # povezava do zgoraj definirane akcije `self.moj_izhod_akcija`
        
    def akcija_pri_pritisku_gumba1(self):
        QtWidgets.QMessageBox.about(self,
                                'Naslov :)',
                                'Tole se sproži pri pritisku gumba.')
```

Program potem poženemo znotraj stavka `try`:

```python
try:
    app = QtWidgets.QApplication(sys.argv)
    mainWindow = GlavnOkno()
    mainWindow.show()
    app.exec()
    sys.exit(0)
except SystemExit:
    print('Zapiram okno.')
except:
    print(sys.exc_info())
```

Za naprednejši zgled glejte datoteki:

* [preprosti_uporabniski_vmesnik.py](./moduli/preprosti_uporabniski_vmesnik.py),
* [uporabniski_vmesnik.py](./moduli/uporabniski_vmesnik.py)

Nekaj komentarjev na ``uporabniski_vmesnik.py``:

1. Poglejte prepis dogodka ``mouseDoubleClickEvent`` in prepišite podedovan dogodek ``keyPressEvent``, ki naj ob pritisku katerekoli tipke zapre program (če se nahajate v TextEdit polju, potem seveda pritisk tipke izpiše vrednost te tipke).
2. Dodajte še kakšen *Widget* s [seznama](http://doc.qt.io/qt-6/gallery-windows.html).
3. Spremenite program, da se bo vedno izrisovala funkcija sinus, v vpisno polje ``function_text`` pa boste zapisali število diskretnih točk (sedaj je točk 100).  Povežite polje z ustreznimi funkcijami.
4. Uredite lovljenje napak pri zgornji spremembi.

## Nekaj vprašanj za razmislek!

1. V VisualStudioCode pripravite modul, ki bo imel dve funkciji:
    * za množenje matrike in vektorja,
    * za množenje dveh matrik.
2. Za modul zgoraj pripravite skripto za testiranje (uporabite ``numpy.testing``).
3. V ``uporabniski_vmesnik_simple.py`` inicializijski metodi ``__init__`` zakomentirajte vse klice na metode ``self.init ...`` razen na metodo: ``self.init_status_bar()``. Poženite program v navadnem načinu. Nastavite *break* točko na ``self.setGeometry(50, 50, 600, 400)`` in poženite program v *debug* načinu.
4. Nadaljujte prejšnjo točko in poiščite bližnjico za pomikanje po vrsticah:
    * s preskokom vrstice,
    * z vstopom v vrstico.

    Vstopite v ``init_status_bar(self)`` in se ustavite pri vrstici ``self.setStatusBar(self.status_bar)``. Odprite konzolo (*console*) in prek ukazne vrstice spremenite vrednost ``self.status_bar.showMessage()``.
5. Odkomentirajte prej (zgoraj) zakomentirane vrstice. Dodajte tretji gumb, ki naj program zapre.
6. Dodajte še kakšen *Widget* s [seznama](https://doc.qt.io/qt-6/qtwidgets-index.html).
