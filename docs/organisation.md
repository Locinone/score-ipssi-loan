
🧠 CONTEXTE

Vous êtes consultant data chez Home Credit, une société de crédit pour clients sans historique bancaire. Votre rôle est de :

Modéliser un système de scoring de crédit (Machine Learning).
Déployer le modèle via une API Web.
Créer un dashboard interactif et interprétable pour les conseillers.

📌 VOTRE MISSION (hors base de données et Scrum)

✅ MISSION 2 : Modèle de scoring + mise en production

- Pipeline ML complet : Feature engineering, entraînement, évaluation, interprétation.
- Techniques attendues :
- Encodage : LabelEncoding, OneHotEncoding.
- Création de features avancées : groupby, ratios, combinaisons de variables.
- SMOTE : rééquilibrage des classes (important car dataset déséquilibré).
- Hyperopt : optimisation automatique des hyperparamètres.
- Fonction coût personnalisée : intégrer les enjeux métier :
- Faux négatif = gros risque (mauvais client accepté).
- Faux positif = manque à gagner (bon client refusé).
- Déploiement API :

En Flask (ou Django).
Appel via un identifiant client → réponse : prédiction + score.
Déploiement conseillé : Heroku ou Netlify.

✅ MISSION 3 : Dashboard interactif

Afficher :

- Données descriptives client.
- Score et interprétation accessible à un non-expert.
- Comparaison avec population moyenne ou clients similaires.
- Outils possibles : Dash, Bokeh, ou Streamlit (non listé mais pertinent).
- Dashboard en ligne (WebApp).
- Graphiques interactifs obligatoires (≥ 2).


🧪 ÉVALUATION / SCORE & MÉTRIQUES

Voici où intervient ton besoin d’expertise avancée sur les scores :

⚙️ Techniques à inclure potentielement :

- Courbe ROC / AUC pour la qualité de discrimination.
F1-score : bon équilibre précision / rappel.

- Fonction de coût personnalisée métier (coût asymétrique FP/FN).

- SHAP / LIME : interprétation des modèles (important pour le dashboard).
- Feature importance : RandomForest, XGBoost, coefficients logistiques.
- Validation croisée / Stratégies robustes pour évaluer le modèle.
🚀 ORGANISATION TECHNIQUE

Environnement Python Poetry OK.

Scripts à organiser :

    1 - notebook_exploration.ipynb → nettoyage
    
    1.5 - notebook_exploration.ipynb → etude des profiles (outliers) ... 

    2 - train_model.py → pipeline + entraînement + serialisation .pkl.

    3 - app.py (Flask API/Fast API).

    4 - dashboard.py (Dash/Bokeh).

    5 - utils/ (fonctions de preprocessing, métriques, coût métier, etc).