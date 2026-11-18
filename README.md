# Detekcija vatre i dima na slikama (D-Fire)

Projektni zadatak iz predmeta **Prepoznavanje oblika i obrada slike** (Elektrotehnički fakultet Sarajevo, Odsjek za računarstvo i informatiku, ak. god. 2026/2027).

**Tim Vatrogasci:** Danijal Alibegović, Aldin Velić, Zijad Doglod, Dino Burić, Haris Macić, Adnan Šemić.

---

## O projektu
Cilj projekta je razvoj i evaluacija modela računarskog vida za automatsku detekciju i razlikovanje prisutnosti **vatre** (`fire`) i **dima** (`smoke`) na digitalnim slikama, uz smanjenje lažnih alarma u scenama s vizuelno sličnim pojavama (magla, oblaci, odsjaj).

Koristi se skup podataka **D-Fire** (21.527 slika i 26.557 anotacija u YOLO formatu; klasa `0: smoke`, klasa `1: fire`), a okosnicu eksperimentalnog dijela čine jednostepeni detektori iz porodice **YOLO** (Ultralytics).

---

## Struktura repozitorija

```text
izvjestaj/
├── data/                            # [GIT-IGNORED] Izvorni D-Fire skup (train/ i test/)
├── dfire_prepared/                  # [GIT-IGNORED] Automatski generisani train/val/test split + data.yaml
├── poglavlja/                       # Zasebni .tex fajlovi po poglavljima izvještaja
│   ├── lv2.tex                      # Pregled i analiza naučnoistraživačkih radova
│   ├── lv3.tex                      # Analiza i profiliranje skupa podataka D-Fire
│   └── lv4.tex                      # Definicija problema, ciljevi i plan rada
├── main.tex                         # Glavni LaTeX dokument projektne dokumentacije
├── preamble.tex                     # Zajednički paketi, stilovi i pravila formatiranja
├── projekt.ipynb                    # Glavni Jupyter Notebook (obrada, trening, evaluacija, demo)
├── .gitignore                       # Pravila za ignorisanje velikih podataka i pomoćnih LaTeX fajlova
└── README.md                        # Upute za tim i pokretanje okruženja
```

---

## Preuzimanje i priprema skupa podataka (Obavezno prije pokretanja koda)

Zbog veličine skupa (~3 GB) ni izvorni folder `data/` ni obrađeni `dfire_prepared/` **ne nalaze se na GitHub-u**. Svaki član tima koji pokreće eksperimente lokalno ili na Google Colabu radi sljedeće:

1. **Preuzimanje:** Skinuti D-Fire arhivu s [Kaggle distribucije](https://www.kaggle.com/datasets/sayedgamal99/smoke-fire-detection-yolo).
2. **Raspakivanje u `data/`:** Raspakovati arhivu tako da unutar `izvjestaj/data/` budu direktno smješteni podfolderi `train/` i `test/` (svaki sa svojim `images/` i `labels/` podfolderima).
3. **Generisanje `dfire_prepared/`:** U `projekt.ipynb` pokrenuti ćeliju za pripremu podataka (Korak 5). Skripta će automatski kreirati folder `dfire_prepared/`, izvršiti stratifikovanu podjelu izvornog trening skupa na `train` (80%) i `val` (20%) uz `SEED = 42`, te generisati `dfire_prepared/data.yaml`.
4. **Reproducibilnost u timu:** Zahvaljujući fiksnom `SEED = 42`, svaki član tima pokretanjem te ćelije dobija **identičan** `train/val/test` raspored slika bez potrebe za slanjem velikih arhiva.

*(Upozorenje: Nikada ne uklanjati `data/` i `dfire_prepared/` iz `.gitignore` fajla!)*

---

## Način rada tima i LaTeX dokumentacija

Dokumentacija je modularno podijeljena kako bi svih 6 članova tima moglo raditi paralelno bez konflikata:

1. **Pravila pisanja poglavlja:**
   * Svako novo poglavlje piše se u zasebnom `.tex` fajlu unutar foldera `poglavlja/` (npr. `lv5.tex` ili `05_implementacija.tex`) i uključuje u `main.tex` naredbom `\poglavlje{poglavlja/naziv_fajla}`.
   * Fajl **ne smije** sadržavati `\documentclass`, `\begin{document}` niti preambulu — počinje direktno sa `\section{Naziv poglavlja}`.
   * Potpisi (`\caption` i `\label`) za slike i tabele idu **ispod** same slike/tabele.
   * Nove reference dodaju se isključivo na kraj bloka `\begin{thebibliography}` u fajlu `main.tex`.

2. **Kompajliranje (Lokalno vs. Overleaf):**
   * **Lokalno (VS Code / TeX Live / MiKTeX):** Kompajlirati `main.tex` koristeći `pdfLaTeX` **dva puta** zaredom (zbog ažuriranja sadržaja i referenci na tabele/slike).
   * **Overleaf (bez lokalne instalacije):** Uploadovati `main.tex`, `preamble.tex` i folder `poglavlja/` u Overleaf projekat (Compiler: `pdfLaTeX`). Nakon uređivanja svog poglavlja na Overleafu, preuzeti samo taj `.tex` fajl iz `poglavlja/` i ubaciti ga u Git repozitorij.

3. **Git workflow:**
   * Prije početka rada obavezno povući posljednje izmjene (`git pull`).
   * Ne mijenjati `preamble.tex` bez dogovora s ostatkom tima.

---

## Python okruženje i pokretanje Notebook-a

Za rad na kodu koristi se **Python 3.10+** (lokalno u VS Code-u ili na Google Colab / Kaggle platformi zbog GPU podrške).

### 1. Priprema okruženja (opciono `venv` ili globalno)
Ako želite izolovano okruženje (preporučeno ako imate više projekata):

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# Linux / macOS:
source .venv/bin/activate
```

*(Ako koristite globalni Python environment, ovaj korak možete preskočiti.)*

### 2. Instalacija potrebnih biblioteka

```bash
pip install ultralytics opencv-python-headless scikit-learn matplotlib seaborn pandas pyyaml pillow jupyter
```

*(Napomena: Ako trenirate lokalno na NVIDIA GPU-u, instalirajte PyTorch verziju s CUDA podrškom sa [pytorch.org](https://pytorch.org/get-started/locally/) prije ili nakon instalacije `ultralytics` paketa.)*

### 3. Pokretanje Notebook-a
* Otvorite `projekt.ipynb` u VS Code-u (uz instalirane *Python* i *Jupyter* ekstenzije) ili ga uploadujte na Google Colab.
* Ćelije za provjeru podataka i generisanje `dfire_prepared/` su samostalne i mogu se pokrenuti nezavisno čim se raspakuje `data/` folder.
* Sve eksperimente pokretati sa fiksnim `SEED = 42` radi uporedivosti rezultata.