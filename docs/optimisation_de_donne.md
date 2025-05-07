## 🔍 Étude exploratoire pour la sélection des variables

### 1. 🎯 **Analyse de la variance et du taux de valeurs manquantes**

* **But** : Éliminer les variables inutiles dès le départ.
* **Étapes** :

  * Supprimer les variables avec > 90% de valeurs manquantes.
  * Supprimer les variables constantes ou quasi-constantes (variance très faible).
* **Code** :

```python
from sklearn.feature_selection import VarianceThreshold

selector = VarianceThreshold(threshold=0.01)
X_reduced = selector.fit_transform(X)
```

---

### 2. 📉 **Analyse de corrélation**

* **But** : Éliminer les variables très corrélées entre elles (colinéarité).
* Appliquer un filtre basé sur un **seuil de corrélation** > 0.9.
* **Bonus** : Garder la variable la plus corrélée avec la cible.
* **Code** :

```python
import seaborn as sns
import matplotlib.pyplot as plt

corr_matrix = X.corr().abs()
sns.heatmap(corr_matrix)
```

---

### 3. 🧪 **PCA (Analyse en Composantes Principales)**

* **But** : Réduire la dimensionnalité tout en gardant 95% de la variance.
* Attention : la PCA **ne conserve pas l'interprétabilité** directe.
* **Utilisation conseillée** :

  * Pour visualiser la structure des données (PCA 2D).
  * Pour aider à du clustering.
* **Code** :

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=0.95)
X_pca = pca.fit_transform(X_scaled)
```

---

### 4. 👥 **Clustering non supervisé (KMeans ou DBSCAN)**

* **But** : Identifier des regroupements naturels de clients.
* Peut révéler des variables discriminantes (ex : revenu, nombre de crédits...).
* Sert aussi à créer des **features supplémentaires** :

  * Ex. : `cluster_label` = 0 à 4.
* **Code** :

```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=5)
clusters = kmeans.fit_predict(X_scaled)
X['cluster'] = clusters
```

---

### 5. 🧠 **Feature importance avec modèles simples**

* **But** : Évaluer l’importance réelle des variables dans un **modèle brut** (pas encore optimisé).
* Méthodes :

  * Random Forest
  * XGBoost
  * Régression logistique L1 (lasso)
* **Code (Random Forest)** :

```python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(n_estimators=100)
rf.fit(X_train, y_train)
importances = rf.feature_importances_
```

---

### 6. 💡 **Méthodes avancées : Mutual Information**

* Capte des relations **non linéaires** entre les variables et la cible.
* Très utile dans le scoring de crédit.
* **Code** :

```python
from sklearn.feature_selection import mutual_info_classif

mi_scores = mutual_info_classif(X, y)
```

---

## ✅ Recommandation d’approche combinée

| Étape | Méthode                                  | Rôle                             |
| ----- | ---------------------------------------- | -------------------------------- |
| 1     | Nettoyage (valeurs manquantes, variance) | Filtrage initial                 |
| 2     | Corrélation + importance via RF/XGBoost  | Réduction + compréhension        |
| 3     | PCA + Clustering                         | Analyse structurelle des données |
| 4     | SHAP (après modèle)                      | Interprétation locale + globale  |