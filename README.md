# 📊 Projet Power BI – Tableau de bord Fiabilité & Opérations Moteurs

## 🎯 Objectif

Ce projet vise à fournir aux équipes **opérations**, **maintenance**, **logistique** et **finance** un **dashboard interactif** permettant de suivre en temps réel :
- La **fiabilité des moteurs**
- Les **interventions MRO**
- Les **consommations de pièces et niveaux de stock**
- L’**impact financier** des activités
- Une **simulation what-if** sur les intervalles d’inspection

## 🧩 Sources de données

- **SQL Server**: `Flights`, `Engines`, `Events`, `WorkOrders`, `Parts`, `Inventory`, `Vendors`, `Costs`, `Calendar`
- **SAP** (exports CSV ou SharePoint): `OT`, `Pièces consommées`, `Fournisseurs`, `Centres de coûts`
- **SharePoint**: Listes de référence (`codes défauts`, `SLA`, `mappings programmes`)
- **Python (préprocessing)**: détection d’anomalies capteurs (z-score, IQR) + consolidation journalière

## 🧱 Modèle de données

Architecture en **étoile** :
- **Faits**: `Fact_Events`, `Fact_WorkOrders`, `Fact_Inventory`, `Fact_Costs`
- **Dimensions**: `Dim_Engine`, `Dim_Date`, `Dim_Part`, `Dim_Location`, `Dim_Program`, `Dim_Vendor`

## ⚙️ Fonctionnalités

- 📈 **KPIs dynamiques** : MTBF, AOG, TAT, % OT à l’heure, NFF %, backorders, coût/heure de vol
- 🔁 **Rafraîchissement** : incrémental sur 12 mois glissants via **Gateway Entreprise**
- 🔐 **Sécurité** : RLS par programme/site, journalisation, sensibilité des données
- 💡 **Simulation what-if** : ajustement des intervalles d’inspection avec impact prévisionnel (TAT, AOG, coûts)

## 📄 Pages Power BI

1. **Executive Dashboard** : Vue synthétique pour direction
2. **Fiabilité moteurs** : Pannes, tendances, drill-down moteur
3. **Maintenance** : Suivi OT, SLA, ateliers
4. **Pièces & Stock** : Conso, ruptures, délais fournisseurs
5. **Finances** : Coûts, écarts vs budget
6. **Simulation** : What-if sur intervalles d’inspection

## 🧠 DAX – Exemples

```DAX
MTBF = DIVIDE([Total Flight Hours], [Nb Pannes])

OT OnTime % = DIVIDE([OT respectant SLA], [OT totales])

Rolling 12m MTBF = CALCULATE([MTBF], DATESINPERIOD('Dim_Date'[Date], MAX('Dim_Date'[Date]), -12, MONTH))
