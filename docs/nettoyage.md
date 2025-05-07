## 🧼 RECOMMANDATIONS POUR LE NETTOYAGE DES DONNÉES

### 1. ✅ **Comprendre la donnée en amont**

* **Comprendre la signification métier** de chaque variable si possible (ex. : `EXT_SOURCE_1`, `DAYS_BIRTH`, etc.).
* Identifier :

  * Variables numériques / catégorielles / dates.
  * Présence de doublons ou d'ID clients.

---

### 2. 🔍 **Valeurs manquantes**

#### 👉 Étapes :

* Calculer le **% de valeurs manquantes** par colonne.
* **Supprimer** les colonnes avec trop de données manquantes (> 90%).
* **Imputation** :

  * Numérique → moyenne, médiane, ou `KNNImputer`.
  * Catégorique → valeur fréquente ou "Unknown".

#### ✅ Recommandations :

* Pour les variables du type `EXT_SOURCE`, imputer avec la **moyenne** ou la **médiane**, car elles sont souvent corrélées avec la cible.
* Pour les variables du type `DAYS_EMPLOYED = 365243`, remplacer cette valeur magique (valeur spéciale pour "non employé").

#### 🔧 Code :

```python
import numpy as np
X['DAYS_EMPLOYED'].replace(365243, np.nan, inplace=True)
X['DAYS_EMPLOYED'].fillna(X['DAYS_EMPLOYED'].median(), inplace=True)
```

---

### 3. 🔢 **Types de variables**

#### ✅ Recommandations :

* Convertir correctement les **booléens** (0/1).
* Identifier les **catégorielles déguisées** en numériques (ex : `CODE_GENDER`, `FLAG_OWN_CAR`).
* Convertir les dates exprimées en jours (ex : `DAYS_BIRTH`) en âge :

```python
X['AGE'] = -X['DAYS_BIRTH'] // 365
```

---

### 4. 🧼 **Outliers (valeurs aberrantes)**

#### 👉 Étapes :

* Repérer les valeurs extrêmes avec :

  * Boxplot
  * IQR (interquartile range)
* Remplacer ou **tronquer** (winsorizing) si pertinent.

#### ✅ Recommandations :

* `AMT_INCOME_TOTAL` > 1M → probablement à tronquer.
* `CNT_CHILDREN` > 10 → suspect.

#### 🔧 Code :

```python
Q1 = X['AMT_INCOME_TOTAL'].quantile(0.25)
Q3 = X['AMT_INCOME_TOTAL'].quantile(0.75)
IQR = Q3 - Q1
X = X[(X['AMT_INCOME_TOTAL'] >= Q1 - 1.5 * IQR) & (X['AMT_INCOME_TOTAL'] <= Q3 + 1.5 * IQR)]
```

---

### 5. 🧹 **Colonnes inutiles ou redondantes**

* Supprimer les identifiants ou variables dupliquées.
* Supprimer les colonnes **non informatives** :

  * Trop corrélées entre elles.
  * Codées de façon identique (ex : `FLAG_DOCUMENT_2` à `FLAG_DOCUMENT_21`).

---

### 6. ♻️ **Normalisation / Standardisation**

* Obligatoire pour :

  * PCA, KMeans, SVM, régression logistique.
* À faire **après** le nettoyage complet.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

---

## ✅ Résumé Pipeline Nettoyage

| Étape                 | But                | Méthodes                      |
| --------------------- | ------------------ | ----------------------------- |
| 1. Traitement des NaN | Éviter les erreurs | Suppression, imputation       |
| 2. Correction types   | Bon encodage       | Mapping, cast, `LabelEncoder` |
| 3. Valeurs extrêmes   | Réduire le bruit   | IQR, winsorizing              |
| 4. Colonnes inutiles  | Alléger le dataset | Variance faible, corrélation  |
| 5. Normalisation      | Préparer le modèle | `StandardScaler`, `MinMax`    |