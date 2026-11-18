# Detekcija vatre i dima na slikama (D-Fire)

Projektni zadatak iz predmeta **Prepoznavanje oblika i obrada slike** (Elektrotehnički fakultet Sarajevo, Odsjek za računarstvo i informatiku, ak. god. 2026/2027).

**Tim Vatrogasci:** Danijal Alibegović, Aldin Velić, Zijad Doglod, Dino Burić, Haris Macić, Adnan Šemić.

---

## O projektu
Cilj projekta je razvoj i evaluacija modela računarskog vida za automatsku detekciju i razlikovanje prisutnosti **vatre** (`fire`) i **dima** (`smoke`) na digitalnim slikama, uz smanjenje lažnih alarma u scenama s vizuelno sličnim pojavama (magla, oblaci, odsjaj).

Koristi se skup podataka **D-Fire** (21.527 slika i 26.557 anotacija u YOLO formatu), a okosnicu eksperimentalnog dijela čine jednostepeni detektori iz porodice **YOLO** (Ultralytics).

---

## Struktura repozitorija

```text
├── main.tex                         # Glavni LaTeX dokument projektne dokumentacije
├── preamble.tex                     # Zajednički paketi, stilovi i pravila formatiranja
├── poglavlja/                       # Zasebni .tex fajlovi po poglavljima
│   ├── lv2.tex                      # Definicija problema, ciljevi i plan rada
│   ├── lv3.tex                      # Analiza i profiliranje skupa D-Fire
│   └── lv4.tex                      # Pregled i analiza naučnoistraživačkih radova
├── projekt.ipynb                    # Glavni Jupyter Notebook za obradu, trening i evaluaciju
└── README.md                        # Upute za tim i pokretanje okruženja
```

*(Napomena: Sam skup slika D-Fire se ne push-a na Git zbog veličine; preuzima se lokalno s [Kaggle distribucije](https://www.kaggle.com/datasets/sayedgamal99/smoke-fire-detection-yolo).)*

---

## Način rada tima i LaTeX dokumentacija

Dokumentacija je modularno podijeljena kako bi svih 6 članova tima moglo raditi paralelno bez konflikata:

1. **Pravila pisanja poglavlja:**
   * Svako novo poglavlje piše se u zasebnom `.tex` fajlu unutar foldera `poglavlja/`. Poglavlja će pratiti rad laboratorijskih vježbi (npr. `lv5.tex`).
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
pip install ultralytics opencv-python-headless scikit-learn matplotlib seaborn pandas pyyaml jupyter
```

### 3. Pokretanje i podjela rada u kodu
* Otvorite `projekt.ipynb` u VS Code-u (uz instalirane *Python* i *Jupyter* ekstenzije) ili ga uploadujte na Google Colab.
* U ćeliji za učitavanje podataka podesite putanju `DATASET_DIR` prema lokaciji vašeg preuzetog D-Fire foldera.
* Sve eksperimente pokretati sa fiksnim `SEED = 42` kako bi rezultati bili reproducibilni kod svih članova tima.