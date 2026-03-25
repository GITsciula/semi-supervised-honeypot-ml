# Detekcja i klasyfikacja ataków na podstawie danych z honeypot z wykorzystaniem sztucznej inteligencji

Celem niniejszego repozytorium jest przedstawienie implementacji badań prowadzonych w ramach pracy magisterskiej, dotyczących wykorzystania współczesnych narzędzi uczenia maszynowego i technik analizy danych do badania informacji pochodzących z systemów typu honeypot. Szczególny nacisk położono na wykrywanie oraz klasyfikację wzorców ataków w ruchu sieciowym.

---

## Informacje o pracy

* **Tytuł:** Detekcja i klasyfikacja ataków na podstawie danych z honeypot z wykorzystaniem sztucznej inteligencji.
* **Autor:** Szymon Ciuła
* **Rok:** 2026
* **Technologie:** Python, UMAP, HDBSCAN, Random Forest, XGBoost

---

## Instrukcja obsługi i uruchomienia

Projekt został zaimplementowany w formie interaktywnych notatników Jupyter Notebook (.ipynb). Pozwala to na śledzenie procesu analizy krok po kroku — od surowych danych po finalne raporty klasyfikacji i analizę zjawisk Zero-Day.

### 1. Środowisko uruchomieniowe
Notatniki można uruchomić na dwa sposoby:
* **Lokalnie:** Korzystając z dystrybucji Anaconda lub klasycznego środowiska wirtualnego (venv) i uruchamiając jupyter lab lub jupyter notebook.
* **W chmurze:** Poprzez platformę Google Colab. W przypadku Colab należy pamiętać o zamontowaniu dysku Google Drive w celu uzyskania dostępu do plików ze strukturą katalogów.

### 2. Przygotowanie danych (Dataset)
Projekt wykorzystuje dane z systemu Hornet:40. Aby zachować spójność ścieżek w kodzie, należy przygotować strukturę katalogów w następujący sposób:

```text
/dane
  ├── /treningowe     <-- Surowe pliki .csv (np. 14 dni z pierwszego tygodnia zbierania logów z jednej geolokalizacji)
  ├── /testowe_A       <-- Surowe pliki .csv z kolejnych okresów z tej samej geolokalizacji (np. 7 dni z kolejnego tygodnia)
  ├── /testowe_B       <-- Surowe pliki .csv z innnej geolokalizacji (np. geo-7 (Azja))
  └── /gotowe_ml      <-- Miejsce zapisu wygenerowanego Ground Truth dla klasyfikatora
```
### Uwaga: Ze względu na limity rozmiaru plików w serwisie GitHub, w repozytorium znajdują się jedynie reprezentatywne próbki danych. Pełne zestawy danych należy pobrać z oficjalnych źródeł projektu Hornet: 40 i umieścić w odpowiednich podfolderach katalogu /dane/.
### Update: Po zwiększeniu ilości danych treningowych i testowych katalog *'dane'* jest pusty pełna jego zawartość znajduję się na Google Drive pod linkiem: https://drive.google.com/drive/folders/1_SH2xF-vRWyT3P-55jVr8AbIZdpj-8Lv?usp=sharing
### 3. Kolejność uruchamiania (Pipeline Badawczy)

Badania są podzielone na logiczne etapy. Proszę uruchamiać notatniki w podanej kolejności:

1. **`01_KMeans_Baseline.ipynb`**
   * Wczytanie danych surowych, czyszczenie i inżynieria cech.
   * Zastosowanie modelu bazowego (Baseline) K-Means oraz liniowej redukcji wymiarowości PCA w celu przygotowania danych.


2. **`02_HDBSCAN.ipynb`**
   * Wykorzystanie algorytmu HDBSCAN oraz nieliniowej redukcji wymiarowości UMAP do wykrywania zorganizowanych kampanii ataków i izolacji szumu informacyjnego.
   * Implementacja autorskiej metody Hybrid Pseudo-labeling, łączącej topologię klastrów z analizą ekspercką (DPI) na poziomie pojedynczych wierszy.


3. **`03_Random_Forest_Classifier.ipynb`**
   * Trening klasyfikatora lasu losowego na wypracowanym "Złotym Standardzie".


4. **`04_XGBoost_Classifier.ipynb`**
   * Implementacja modelu gradient boostingu (XGBoost) i ocena jego skuteczności względem modelu Random Forest.

### Uwaga: Do uruchomienia notatnika **`05_Model_Comparison.ipynb`** potrzbne są modele znajdujące się w katalogu *'modele'* w związku z ograniczeniami przestrzeni modele te zostaną poprawnie wygenerowane po uruchomieniu notatnika 03 i 04.

5. **`05_Model_Comparison.ipynb`**
   * Ostateczne starcie modeli. Porównanie znaczenia cech sieciowych (Feature Importance) oraz analiza zjawiska overconfidence w detekcji nieznanych zagrożeń.
   * Porównanie skuteczności modeli w Teście A (Dryf czasowy).
   * Porównanie skuteczności modeli w Teście B (Dryf przestrzenny).

---

## Wymagania (Dependencies)

Wszystkie niezbędne biblioteki można zainstalować przy użyciu dołączonego pliku `requirements.txt`:

```bash
pip install -r requirements.txt
```

Kluczowe biblioteki użyte w projekcie:

- umap-learn oraz hdbscan (Analiza nienadzorowana)
- scikit-learn oraz xgboost (Uczenie maszynowe)
- pandas oraz numpy (Przetwarzanie i transformacja danych)
- matplotlib oraz seaborn (Wizualizacja danych)
