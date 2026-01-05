
# Compte rendu — Analyse de la consommation électrique d’un ménage

# Chaali Ossama 

*Numéro d'étudiant* : 24010364

*Classe* : CAC1


<img src="Chaali Ossama.jpg" style="height:464px;margin-right:432px"/>

<br clear="left"/>

---
## 🎯 Objectif de l’analyse
Comprendre le comportement de la consommation électrique d’un ménage sur une longue période, à partir de mesures effectuées chaque minute pendant près de 4 ans.  
L’analyse porte donc sur une série temporelle énergétique de grande taille, permettant d’observer les tendances, usages et variables explicatives de la consommation.

---

## 1️⃣ Chargement et préparation des données
Les données sont importées depuis le fichier `household_power_consumption.txt` en spécifiant :

- séparateur : `;`
- `na_values='?'` pour remplacer les valeurs manquantes
- fusion `Date + Time` en un index temporel `Datetime`

📌 Cela permet une manipulation efficace des séries temporelles.

**Code utilisé : lecture + parsing des dates**  
```python
# Exemple
import pandas as pd

df = pd.read_csv(
    'household_power_consumption.txt',
    sep=';',
    na_values='?',
    parse_dates={'Datetime': ['Date', 'Time']},
    infer_datetime_format=True
)
```

---

## 2️⃣ Nettoyage des données
- Suppression des valeurs manquantes : `df.dropna()`  
- Définition de l’index : `df.set_index('Datetime')`  
- Conversion des colonnes en float

➡️ Objectif : améliorer la qualité statistique et la précision des analyses.

**Code nettoyage + typage des colonnes**
```python
df = df.dropna()
df = df.astype(float)
df.set_index('Datetime', inplace=True)
```

---

## 3️⃣ Agrégation quotidienne (Resampling)
Les valeurs minute par minute sont regroupées en moyenne quotidienne :  
```python
df_daily = df.resample('D').mean()
```

🎯 But : réduire la volumétrie tout en conservant les tendances journalières, hebdomadaires et saisonnières.

---

## 4️⃣ Visualisation : Puissance active globale
La variable `Global_active_power` (kW) est tracée sur toute la période.

📌 Apports de cette visualisation :  
- Détection de périodes de forte consommation (chauffage, climatisation…)  
- Observation de cycles annuels récurrents  

*(Graphique généré dans le script)*

---

## 5️⃣ Analyse des sous-compteurs (Sub-metering)
Trace des 3 postes de consommation :

| Variable           | Usage probable                 |
|-------------------|-------------------------------|
| Sub_metering_1     | Cuisine                        |
| Sub_metering_2     | Buanderie / Réfrigération      |
| Sub_metering_3     | Chauffe-eau / Climatisation    |

📌 Cette analyse permet d’identifier quels appareils consomment le plus, et quand.

*(Code graphique sous-compteurs)*

---

## 6️⃣ Corrélations entre variables
Calcul et heatmap des corrélations : `df_daily.corr()`

Objectifs :  
- Repérer les relations linéaires entre paramètres électriques  
- Identifier les meilleures variables explicatives pour la consommation globale  

➡️ Exemple : forte corrélation entre `Global_active_power` et `Global_intensity`

*(Code heatmap corrélations)*

---

## 7️⃣ Régression Linéaire (prédiction simple)
- Variables explicatives : toutes sauf `Global_active_power`  
- Variable cible : `Global_active_power`

**Étapes :**

| Étape           | Technique utilisée          |
|-----------------|----------------------------|
| Split données   | 80% train / 20% test       |
| Modèle          | `LinearRegression()`       |
| Évaluation      | MSE + R²                   |
| Visualisation   | valeurs réelles vs prédites|

*(Graphique + métriques du modèle affichés dans le script)*

---

## 💡 Principaux enseignements
- La consommation suit des cycles saisonniers marqués  
- Certains sous-compteurs ont un rôle majeur dans la dépense énergétique  
- Une simple régression linéaire capture déjà une partie du comportement global  
- Les corrélations aident à cibler les équipements les plus énergivores

---

## 🔎 Perspectives d’amélioration
- Exploiter la granularité minute pour détecter les pics rapides (appareils cycliques)  
- Tester des modèles plus puissants : Random Forest, XGBoost  
- Modèles de séries temporelles : SARIMA, Prophet, LSTM  
- Intégrer des variables externes : météo, occupation du logement, jours fériés…

---

## 📌 Conclusion
Cette analyse exploratoire met en évidence les tendances de consommation électrique d’un ménage sur 4 ans, ainsi que les facteurs qui influencent le plus la puissance demandée.  
Elle constitue une base solide pour un futur modèle prédictif plus précis.
