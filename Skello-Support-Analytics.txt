# Skello-Support-Analytics - Optimisation du Service Client 
*Optimisation du service client via la data – Cas pratique Data Analyst*

---

## Contexte

Cette étude de cas a été réalisée dans le cadre du processus de recrutement pour le stage de Data Analyst chez Skello.
Au sein de **Skello**, l’équipe Support utilise l’outil **Intercom** pour gérer les conversations clients.  
Lorette, responsable du support, souhaite mieux piloter la qualité de service à travers :

- la **satisfaction client (CSAT)**,  
- la **rapidité de réponse (< 5 minutes)**,  
- les **volumes de conversations par agent**,  
- et la **répartition des charges** dans la semaine.  

---

## Objectifs du projet

- Construire un **modèle de données modulaire** (RAW → SILVER → GOLD).  
- Calculer les KPI clés : **CSAT, % < 5 min, volume par agent, réactivité.**  
- Mettre en place un **reporting clair et actionnable** dans Power BI.  
- Documenter et publier le projet sur GitHub dans une logique professionnelle.

---
## WORKFLOW

**Transformations clés réalisées en SQL :**
- Conversion et typage des dates (`TIMESTAMP_NTZ`)
- Parsing des champs JSON pour extraire les identifiants et ratings
- Exclusion des **messages bots**
- Calculs des KPI :  
  - *First Response Time (FRT)*  
  - *CSAT moyen*  
  - *% de réponses < 5 min*  
  - *Heatmap horaire Europe/Paris*
 
    
## Modèle de données 'Snowflake'

| Niveau | Description | Tables principales |
|---------|--------------|--------------------|
| **RAW** | Données brutes importées depuis Intercom | `CONVERSATIONS_RAW`, `CONVERSATION_PARTS_RAW` |
| **SILVER** | Données nettoyées et harmonisées | `FACT_CONVERSATIONS`, `FACT_CONVERSATION_PARTS`, `DIM_AGENTS` |
| **GOLD** | Couches analytiques prêtes à l’usage | `VW_SUPPORT_WEEKLY_NAMED`, `VW_FIRST_RESPONSE_NAMED`, `VW_ACTIVITY_HEATMAP_PARIS` |

➡️ Architecture : **RAW → SILVER → GOLD → Power BI**

---

## Dashboard Power BI

Le dashboard se compose de **deux pages** principales :

### Page 1 : Vue hebdomadaire
- Indicateurs clés : CSAT moyen, % < 5 min, volume total, temps moyen de réponse  
- Comparatif agents (tableau synthétique)  
- Graphique d’évolution CSAT / réactivité sur 8 semaines  

### Page 2 : Analyse et alertes
- Heatmap horaire d’activité (pics de charge)  
- Identification des conversations à risque (CSAT bas, lenteur)  
- Volume par agent et filtres interactifs  

---

## Dictionnaire de KPI

| KPI | Description | Formule simplifiée | Source |
|------|--------------|--------------------|---------|
| **CSAT moyen** | Note moyenne de satisfaction client | `AVG(csat_rating)` | `VW_SUPPORT_WEEKLY_NAMED` |
| **% < 5 min** | Conversations répondue < 300 s | `(count < 300 s / total)*100` | `VW_FIRST_RESPONSE_NAMED` |
| **Volume total** | Conversations ouvertes par agent | `COUNT(conversation_id)` | `FACT_CONVERSATIONS` |
| **Temps moyen réponse** | Moyenne du First Response Time | `AVG(first_response_seconds)` | `VW_FIRST_RESPONSE_NAMED` |

---

## Résultats et Insights clés

- 70 % des conversations reçoivent une réponse en moins de 5 minutes.  
- Le **CSAT moyen** s’élève à **4,2/5**, avec une amélioration sur les dernières semaines.  
- **Héloïse** est l’agente la plus réactive.  
- Les pics d’activité sont observés entre **10h et 12h**, surtout les **lundis et mardis**.  
- Volume d’alertes : **747**, Réponses lentes : **696**

---

##  Stack technique

| Outil | Rôle |
|--------|------|
| **Sn owflake** | Stockage et transformation de données |
| **SQL** | Nettoyage, jointures, création de vues |
| **Power BI** | Visualisation et analyse |
| **GitHub** | Documentation et versioning |
| **Excel** | Dictionnaire de données et KPI |
| **Python**| Data préparation (Etude statistique, prise d'information necessaire, verification des data bases |

---

## Recommandations

1. **Renforcer les effectifs** sur les créneaux critiques (10h–12h et 14h–16h).
2. **Justine apparaît dans les données mais n’a aucune conversation assignée** (inactivité détectée et signalée via une carte d’alerte personnalisée). 
3. **Réduire le FRT moyen** pour atteindre 50 % de réponses en moins de 5 minutes.  
4. **Analyser les causes des CSAT bas** pour améliorer les processus de support.  
5. **Présenter ce dashboard** chaque lundi pour un pilotage data-driven et prévoir une automatisation des reportings.
6. **Enrichissement** : Joindre avec données CRM (plan tarifaire, industrie du client)

---

## Limites et remarques techniques
1. le CSAT ne couvre que les conversations notées → risque de biais dans la satisfaction globale.
2. certains agents (ex. Justine) apparaissent comme actifs sans être officiellement assignés → attribution parfois ambiguë.
3. Temps de réponse : basé sur le premier message admin hors bot, ce qui peut sous-estimer la réactivité dans certains cas.
4. DirectQuery non supporté : certaines transformations SQL (JSON, dates) ont nécessité le mode Import, limitant l’actualisation en temps réel.

---

## Technologies & Compétences Démontrées

### Techniques

-  Architecture de données (Medallion)
-  Modélisation (Star Schema : faits + dimensions)
-  SQL avancé (CTEs, Window Functions, JSON parsing)
-  Visualisation de données (Power BI, DAX)
-  Data Quality (contrôles, validations)

---

### Métier

-  Compréhension enjeux business (Support Client)
-  Définition de KPIs pertinents
-  Storytelling avec les données
-  Recommandations actionnables

---

### Soft Skills

-  Documentation (README, dictionnaire, guides)
-  Communication (présentation, explications)
-  Rigueur (structure, nommage, versioning)

---

##  Contact

**[Laetitia NGONO]**  
📧 Email : laetitia.ngono@inseec-france.com  
💼 LinkedIn : [https://www.linkedin.com/in/laetitia-n/]  
🐙 GitHub : [Laetitia-Ngono]

---

## 🙏 Remerciements

Merci à **Skello** pour ce cas pratique stimulant qui m'a permis de mettre en pratique mes compétences en Data Analytics dans un contexte réel de Support Client.


