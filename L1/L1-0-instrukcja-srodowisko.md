# L1, część 0 - środowisko pracy

## 1. Po co osobne środowisko?

Środowisko conda to odizolowany katalog z własnym Pythonem i własnymi bibliotekami. Dzięki temu:

- biblioteki jednego projektu nie psują drugiego (np. różne wersje `numpy`),
- środowisko da się odtworzyć na innym komputerze z jednego pliku (`environment.yml`),
- zepsute środowisko można usunąć i utworzyć od nowa bez reinstalacji Anacondy.

Nie instalujemy niczego w środowisku `base`.

## 2. Utworzenie środowiska

Otwórz **Anaconda Prompt** i przejdź do katalogu z materiałami:

```bash
cd ścieżka\do\L1
```

Utwórz środowisko z pliku (kilka minut, pobiera ok. 1 GB):

```bash
conda env create -f environment.yml
```

Aktywuj je - nazwa w nawiasie na początku wiersza zmieni się z `(base)` na `(gsn)`:

```bash
conda activate gsn
```

Zarejestruj środowisko jako kernel Jupytera, żeby było widoczne na liście kerneli:

```bash
python -m ipykernel install --user --name gsn --display-name "Python (gsn)"
```

## 3. Najważniejsze polecenia

| Polecenie                                     | Co robi                                           |
| --------------------------------------------- | ------------------------------------------------- |
| `conda env list`                            | lista środowisk, gwiazdka przy aktywnym          |
| `conda activate gsn` / `conda deactivate` | wejście do środowiska / wyjście                |
| `pip list`                                  | zainstalowane pakiety (w aktywnym środowisku)    |
| `pip install nazwa`                         | doinstalowanie pakietu                            |
| `conda env export --from-history > env.yml` | zapis środowiska do pliku                        |
| `pip freeze > requirements.txt`             | lista pakietów z wersjami (wymagana w projekcie) |
| `conda env remove -n gsn`                   | usunięcie środowiska                            |

W pliku `environment.yml` z conda instalujemy tylko Pythona, a biblioteki przez `pip`. Mieszanie `conda install` i `pip install` dla tych samych pakietów to najczęstsza przyczyna zepsutych środowisk - w `gsn` doinstalowujemy wyłącznie przez `pip`.

### Karta graficzna (opcjonalnie)

`environment.yml` instaluje PyTorch w wersji CPU, która działa wszędzie. Jeśli masz kartę NVIDIA, po utworzeniu środowiska podmień PyTorch na wersję z CUDA (aktualne polecenie: [https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/)), np.:

```bash
pip install --force-reinstall torch torchvision --index-url https://download.pytorch.org/whl/cu124
```

## 4. Trzy sposoby pracy z notebookiem

|                    | Jupyter Lab                                                   | VS Code                                            | Google Colab                                                                       |
| ------------------ | ------------------------------------------------------------- | -------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Gdzie działa kod  | Twój komputer                                                | Twój komputer                                     | serwer Google                                                                      |
| Uruchomienie       | `jupyter lab` w Anaconda Prompt (po `conda activate gsn`) | otwórz folder, kliknij plik`.ipynb`             | [https://colab.research.google.com](https://colab.research.google.com) → Prześlij |
| Wybór środowiska | menu Kernel → Change Kernel → „Python (gsn)”              | „Select Kernel” w prawym górnym rogu → „gsn” | brak - biblioteki są preinstalowane, brakujące:`!pip install nazwa`            |
| GPU                | tylko jeśli masz kartę NVIDIA                               | jw.                                                | Środowisko wykonawcze → Zmień typ → T4 GPU                                     |
| Pliki z danymi     | na dysku, ścieżki względne                                 | na dysku, ścieżki względne                      | trzeba wgrać lub podłączyć Dysk Google                                         |

### Jupyter Lab

```bash
conda activate gsn
cd ścieżka\do\L1-srodowisko-obrazy
jupyter lab
```

### VS Code

Zainstaluj rozszerzenia **Python** i **Jupyter**. Otwórz cały folder (File → Open Folder), nie pojedynczy plik - inaczej ścieżki względne do danych nie zadziałają.

### Google Colab

Sesja Colab jest tymczasowa: po rozłączeniu pliki wgrane do `/content` znikają. Dane trzymamy na Dysku Google:

```python
from google.colab import drive
drive.mount('/content/drive')
%cd /content/drive/MyDrive/GSN/L1-srodowisko-obrazy
```

## 5. Skróty klawiszowe (Jupyter / VS Code)

Tryb poleceń (`Esc`):

| Skrót        | Działanie                        |
| ------------- | --------------------------------- |
| `A` / `B` | nowa komórka powyżej / poniżej |
| `D D`       | usunięcie komórki               |
| `M` / `Y` | zamiana na Markdown / kod         |
| `Z`         | cofnięcie usunięcia komórki    |
| `0 0`       | restart kernela                   |

Tryb edycji (`Enter`):

| Skrót            | Działanie                                 |
| ----------------- | ------------------------------------------ |
| `Shift + Enter` | uruchom komórkę i przejdź do następnej |
| `Ctrl + Enter`  | uruchom komórkę i zostań w niej         |
| `Tab`           | podpowiedzi                                |
| `Shift + Tab`   | dokumentacja funkcji pod kursorem          |
| `Ctrl + /`      | zakomentowanie zaznaczonych wierszy        |

## 6. Trzy zasady pracy w notebooku

1. **Kolejność uruchamiania ma znaczenie.** Zmienne żyją w kernelu, nie w komórce. Liczba w `[ ]` obok komórki pokazuje kolejność wykonania.
2. **Przed oddaniem: Kernel → Restart Kernel and Run All Cells.** Jeśli notebook nie przechodzi od góry do dołu, to nie działa.
3. **Ścieżki względne.** `./dane/lot1.tif`, nigdy `C:/Users/Jan/Desktop/...` - taki notebook działa tylko u autora.

## 7. Sprawdzenie

Otwórz `L1-0-test-srodowiska.ipynb` kolejno w Jupyter Lab, VS Code i Colab, wybierz kernel „Python (gsn)” i uruchom wszystkie komórki. Wszystkie biblioteki powinny mieć status OK.
