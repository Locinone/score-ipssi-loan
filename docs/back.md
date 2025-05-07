## 🏗️ STRUCTURE PROPOSÉE DU BACKEND

```
credit_scoring_api/
│
├── app/
│   ├── main.py               ← Point d'entrée FastAPI
│   ├── models/               ← Modèles de données Pydantic (entrée/sortie)
│   │   └── schemas.py
│   ├── api/                  ← Routes de l'API
│   │   └── endpoints.py
│   ├── core/                 ← Logique métier
│   │   ├── predictor.py      ← Chargement modèle + prédiction
│   │   ├── utils.py          ← Encodage, normalisation, etc.
│   └── config.py             ← Chemins, constantes, logging
│
├── model/
│   └── model.pkl             ← Modèle ML sérialisé
│
├── requirements.txt          ← Dépendances
├── run.sh                    ← Script pour lancer FastAPI
├── .env                      ← Variables d’environnement
└── README.md
```

---

## ⚙️ ÉTAPES TECHNIQUES POUR FAIRE TOURNER TON BACKEND

### 1. 📦 Installation des dépendances

```bash
pip install fastapi uvicorn scikit-learn joblib pydantic ngrok
```

---

### 2. 📁 `models/schemas.py` – Définir l'entrée/sortie

```python
from pydantic import BaseModel

class ClientInput(BaseModel):
    EXT_SOURCE_1: float
    DAYS_BIRTH: int
    AMT_INCOME_TOTAL: float
    # ajoute les features nécessaires…

class PredictionOutput(BaseModel):
    prediction: int
    score: float
```

---

### 3. 🧠 `core/predictor.py` – Charger le modèle + prédire

```python
import joblib

model = joblib.load("model/model.pkl")

def predict(client_data: dict):
    # Transformer le dict en DataFrame → appliquer le pipeline → prédire
    import pandas as pd
    df = pd.DataFrame([client_data])
    score = model.predict_proba(df)[0][1]
    pred = int(score > 0.5)
    return pred, score
```

---

### 4. 🚀 `api/endpoints.py` – Créer la route `/predict`

```python
from fastapi import APIRouter
from app.models.schemas import ClientInput, PredictionOutput
from app.core.predictor import predict

router = APIRouter()

@router.post("/predict", response_model=PredictionOutput)
def make_prediction(client: ClientInput):
    pred, score = predict(client.dict())
    return {"prediction": pred, "score": score}
```

---

### 5. 🏁 `main.py` – Lancer l'API

```python
from fastapi import FastAPI
from app.api import endpoints

app = FastAPI(title="Credit Scoring API")
app.include_router(endpoints.router)
```

---

### 6. ✅ Lancer FastAPI

```bash
uvicorn app.main:app --reload
```

---

### 7. 🌍 Tester avec ngrok

```bash
ngrok http 8000
```

Tu verras une URL publique comme :

```
https://abc123.ngrok.io
```

Tu peux tester `/docs` pour la doc Swagger auto-générée :

```
https://abc123.ngrok.io/docs
```

---

## 🔒 BONUS (optionnel mais pro)

| Tâche                | Tech/Libs                         |
| -------------------- | --------------------------------- |
| Logger pro           | `loguru`, `logging`               |
| Environnement `.env` | `python-dotenv`                   |
| Tests unitaires      | `pytest`, `httpx`, `unittest`     |
| Sécurité auth        | Token JWT avec `fastapi.security` |
| Déploiement cloud    | Heroku, Render, Azure App Service |