# Web Scraping & API Pipeline

## Objectif

L’objectif de cet exercice est de construire un **mini pipeline de collecte et transformation de données** en Python.

Les étudiants devront :

1. Scraper des données depuis un site web
2. Nettoyer certaines informations avec **regex**
3. Enrichir les données avec une **API de taux de change**
4. Exporter les données en **CSV**
5. Charger les données dans **PostgreSQL**

Ce type de workflow correspond à une **mini pipeline ETL** souvent rencontrée dans le travail d’un Data Analyst.

---

# Dataset utilisé

Site utilisé pour le scraping :

https://books.toscrape.com

Les données seront récupérées depuis les **2 premières pages du catalogue** :

```
https://books.toscrape.com/catalogue/page-1.html
https://books.toscrape.com/catalogue/page-2.html
```

Chaque page contient environ **20 livres**.

---

# Données à extraire

Pour chaque livre, il faut récupérer :

* **Nom du livre**
* **Prix en GBP**
* **Disponibilité (en stock ou non)**
* **Star rating** (bonus)

Exemple dans le HTML :

```html
<h3>
  <a title="Libertarianism for Beginners">...</a>
</h3>

<p class="price_color">£51.33</p>

<p class="instock availability">
    In stock
</p>

<p class="star-rating Two"></p>
```

---

# Partie Regex

Le prix récupéré dans le HTML est une chaîne de caractères :

```
£51.33
```

Vous devrez utiliser **regex** pour extraire uniquement la valeur numérique.

Exemple attendu :

```
£51.33 → 51.33
```

Bonus :

Extraire le **star rating** depuis la classe CSS :

```
star-rating Two → Two
```

---

# Conversion de devise

Les prix du site sont en **livres sterling (GBP)**.

Vous devrez convertir ces prix en **euros (EUR)** en utilisant l’API suivante :

```
https://hexarate.paikama.co/api/rates/GBP/EUR/latest
```

Cette API retourne le taux de change actuel.

---

# Pipeline de données

Le projet suit cette logique :

```
Scraping HTML
↓
data/raw/books_raw.csv
↓
Nettoyage + conversion EUR via API
↓
data/processed/books_processed.csv
↓
Export PostgreSQL
```

---

# Structure du projet

```
scraping_api_pipeline
│
├── data
│   ├── raw
│   └── processed
│
├── notebooks
│   └── scraping_demo.ipynb
│
├── scripts
│   ├── scrape_books.py
│   ├── convert_currency.py
│   └── export_postgres.py
│
├── sql
│   └── create_table.sql
│
├── requirements.txt
└── README.md
```

---

# Étapes de l'exercice

## 1 — Scraping

Créer le script :

```
scripts/scrape_books.py
```

Ce script doit :

* scraper les **2 pages du catalogue**
* extraire :

  * `name`
  * `price_pound`
  * `stock`
  * `star_rating`

Exporter le résultat dans :

```
data/raw/books_raw.csv
```

---

## 2 — Conversion des prix

Créer le script :

```
scripts/convert_currency.py
```

Ce script doit :

* lire `books_raw.csv`
* appeler l’API de taux de change
* créer une colonne :

```
price_euro
```

Sauvegarder le résultat dans :

```
data/processed/books_processed.csv
```

---

(Bonus)
## 3 — Export PostgreSQL

Créer le script :

```
scripts/export_postgres.py
```

Ce script doit :

* lire `books_processed.csv`
* insérer les données dans une table PostgreSQL

La table peut être créée avec :

```
sql/create_table.sql
```

---

# Format final attendu

Le fichier final doit contenir :

| name       | price_pound | price_euro | stock | star_rating |
| ---------- | ----------- | ---------- | ----- | ----------- |
| Book title | 51.33       | 60.21      | True  | Two         |

---

# Installation

Installer les dépendances :

```bash
pip install -r requirements.txt
```

---

# Lancer les scripts

Scraping :

```bash
python scripts/scrape_books.py
```

Conversion euro :

```bash
python scripts/convert_currency.py
```

(Bonus)
Export PostgreSQL : 

```bash
python scripts/export_postgres.py
```

---

# Compétences travaillées

Dans cet exercice vous utiliserez :

* **requests** → requêtes HTTP
* **BeautifulSoup** → parsing HTML
* **regex** → extraction de données
* **pandas** → manipulation de données
* **API REST**
* **CSV**
* **PostgreSQL**

---

# Bonus

Améliorations possibles :

* scraper plus de pages
* gérer les erreurs réseau
* transformer le star rating en valeur numérique
* automatiser complètement l’export PostgreSQL
