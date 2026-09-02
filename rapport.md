# Qualité des eaux de surface — Bassin Rhin-Meuse 2013
## Catégorisation des stations et lien avec l'occupation des sols

**UE Données complexes — Données environnementales — M1 SDSC**  
Source : NAIADES (Analyses.CSV, Stations.CSV) · Corine Land Cover 2012

---

## 1. Chargement et formatage des données

### 1.1 Source et structure

Les données proviennent de la base NAIADES (Agence de l'eau Rhin-Meuse) pour l'année 2013. Le fichier `Analyses.CSV` contient l'ensemble des mesures physico-chimiques réalisées sur les stations du bassin. On ne retient que les **15 paramètres** demandés (Tableau 1), en faisant correspondre exactement le nom du paramètre, la fraction analysée et l'unité.

**Tableau 1 — Paramètres physico-chimiques retenus**

| Paramètre | Fraction | Unité |
|---|---|---|
| Température de l'Eau | Eau brute | °C |
| Potentiel en Hydrogène (pH) | Eau brute | unité pH |
| Oxygène dissous | Eau brute | mg(O₂)/L |
| Conductivité à 25°C | Eau brute | µS/cm |
| Matières en suspension | Eau brute | mg/L |
| Turbidité Formazine Néphélométrique | Eau brute | NFU |
| Carbone Organique | Phase aqueuse filtrée | mg(C)/L |
| Nitrates | Phase aqueuse filtrée | mg(NO₃)/L |
| Phosphore total | Eau brute | mg(P)/L |
| Azote Kjeldahl | Eau brute | mg(N)/L |
| Orthophosphates (PO₄) | Phase aqueuse filtrée | mg(PO₄)/L |
| Ammonium | Phase aqueuse filtrée | mg(NH₄)/L |
| Nitrites | Phase aqueuse filtrée | mg(NO₂)/L |
| DBO₅ | Eau brute | mg(O₂)/L |
| DCO | Eau brute | mg(O₂)/L |

### 1.2 Tableau station × date

Après pivot (agrégation par moyenne lorsque plusieurs mesures existent pour un même triplet station–date–paramètre), on obtient un tableau de **3 223 lignes** (prélèvements) × 15 colonnes paramètres, couvrant **274 stations** sur la période du 7 janvier au 19 décembre 2013.

---

## 2. Statistiques descriptives

### 2.1 Fréquence des paramètres

![Fréquence des paramètres](outputs/figures/tp1_param_counts.png)

**Figure 1 — Nombre de mesures disponibles par paramètre.**

Les paramètres les plus fréquents (Tableau 2) sont ceux mesurés in situ : Température, Oxygène dissous et DBO₅ (~3 056 mesures chacun). Les moins fréquents sont les paramètres azotés et phosphorés en phase aqueuse filtrée (Ammonium, Nitrites, Orthophosphates : ~2 437 mesures), qui nécessitent une analyse en laboratoire et ne sont pas systématiquement prélevés dans toutes les campagnes.

**Tableau 2 — Mesures disponibles et statistiques clés par paramètre (2013)**

| Paramètre | n | Moyenne | Médiane | Max |
|---|---|---|---|---|
| Température (°C) | 3 056 | 11.0 | 11.5 | 26.6 |
| pH | 3 033 | 7.87 | 7.90 | 9.55 |
| O₂ dissous (mg/L) | 3 056 | 9.89 | 9.90 | 16.1 |
| Conductivité (µS/cm) | 3 041 | 535.6 | 440 | 4 500 |
| MES (mg/L) | 3 042 | 16.4 | 9.0 | 900 |
| Turbidité (NFU) | 2 491 | 8.05 | 4.20 | 203 |
| Carbone org. (mg/L) | 3 002 | 3.46 | 3.03 | 20.3 |
| Nitrates (mg/L) | 2 596 | 10.5 | 8.70 | 60.0 |
| Phosphore total (mg/L) | 3 040 | 0.11 | 0.08 | 2.10 |
| Azote Kjeldahl (mg/L) | 3 042 | 0.72 | 0.50 | 20.3 |
| Orthophosphates (mg/L) | 2 437 | 0.22 | 0.15 | 4.91 |
| Ammonium (mg/L) | 2 436 | 0.17 | 0.07 | 17.0 |
| Nitrites (mg/L) | 2 437 | 0.09 | 0.05 | 2.80 |
| DBO₅ (mg/L) | 3 056 | 1.75 | 1.60 | 18.2 |
| DCO (mg/L) | 3 041 | 11.7 | 9.00 | 240 |

### 2.2 Suivi par station

![Prélèvements par station](outputs/figures/tp1_prelevements_par_station.png)

**Figure 2 — Distribution du nombre de prélèvements par station.**

La médiane est de 12 prélèvements par station (environ mensuel). Quelques stations présentent un suivi renforcé (> 20 prélèvements) tandis qu'une minorité de stations très peu suivies (< 4 prélèvements) seront éliminées lors de l'étape de nettoyage.

### 2.3 Distribution de trois paramètres contrastés

![Distributions](outputs/figures/tp1_distributions_3params.png)

**Figure 3 — Distribution de trois paramètres contrastés.**

- **Température** : distribution quasi-gaussienne centrée sur 11.5 °C, traduisant la variabilité saisonnière sur l'année 2013.
- **Nitrates** : distribution asymétrique à droite (médiane 8.7 mg/L, max 60 mg/L), avec une queue de valeurs élevées trahissant des stations fortement exposées aux intrants agricoles.
- **Orthophosphates** : très concentrée en dessous de 0.5 mg/L pour la majorité des stations, mais avec quelques valeurs extrêmes (max 4.91 mg/L) caractéristiques de rejets ponctuels.

### 2.4 Corrélations

![Corrélations](outputs/figures/tp1_correlation_matrix.png)

**Figure 4 — Matrice de corrélation des 15 paramètres physico-chimiques.**

Les corrélations les plus fortes (Tableau 3) révèlent des liens physico-chimiques cohérents :

**Tableau 3 — Corrélations notables (|r| > 0.5)**

| Paramètre A | Paramètre B | r |
|---|---|---|
| Phosphore total | Orthophosphates | **0.944** |
| Azote Kjeldahl | Ammonium | **0.830** |
| Azote Kjeldahl | DCO | 0.661 |
| Température | O₂ dissous | −0.674 |
| Carbone org. | DCO | 0.624 |
| Turbidité | Carbone org. | 0.516 |
| MES | DCO | 0.505 |
| MES | Turbidité | 0.502 |

- **Phosphore total ↔ Orthophosphates (r = 0.94)** : les orthophosphates constituent la principale forme du phosphore total en solution, d'où cette quasi-colinéarité.
- **Azote Kjeldahl ↔ Ammonium (r = 0.83)** : l'ammonium est la forme dominante de l'azote organique dans les rejets domestiques.
- **Température ↔ O₂ (r = −0.67)** : relation physique classique, la solubilité de l'oxygène diminuant avec la température.
- **Azote Kjeldahl / Carbone org. / DCO** : ces trois paramètres covarient car ils reflètent tous la charge en matière organique.

### 2.5 Gestion des données manquantes

Le taux de complétion par station est calculé comme la proportion de cellules (station, paramètre) renseignées sur l'ensemble de l'année. **Les stations dont moins de 50 % des paramètres sont disponibles sont supprimées** (seuil choisi pour garantir un profil minimal représentatif).

![Complétude](outputs/figures/tp1_completude_stations.png)

**Figure 5 — Complétude par station et seuil de suppression à 50 %.**

Ce filtre conserve **274 stations** sur la totalité du bassin. Les 46 stations additionnellement éliminées lors de l'agrégation (paramètre toujours absent même en moyenne) aboutissent à **228 stations** disponibles pour le clustering.

---

## 3. Agrégation annuelle et clustering

### 3.1 Agrégation par la moyenne

Pour chaque station, on calcule la **moyenne annuelle** de chacun des 15 paramètres sur tous les prélèvements de 2013. Cette agrégation résume le profil moyen de qualité de l'eau à l'échelle de l'année, en lissant la variabilité saisonnière. Les données sont ensuite standardisées (z-score) avant le clustering, car les paramètres ont des unités et des ordres de grandeur très différents.

### 3.2 Justification du nombre de clusters

![Choix de k](outputs/figures/tp2_choix_k.png)

**Figure 6 — Méthode du coude (inertie) et score de silhouette pour k = 2 à 9.**

Le score de silhouette est maximum pour **k = 2** (≈ 0.67), mais cette bipartition sépare uniquement les stations de montagne (très faible conductivité) du reste du bassin — peu informatif d'un point de vue environnemental. À partir de k = 3, le score se stabilise autour de 0.22, et la méthode du coude montre un gain d'inertie significatif jusqu'à **k = 4** puis une décroissance nettement plus lente. On retient **k = 4 clusters**, qui permet d'identifier des profils de qualité écologiquement interprétables (eaux naturelles / agricoles / industrielles / très polluées).

![Silhouette](outputs/figures/tp2_silhouette.png)

**Figure 7 — Diagramme de silhouette pour k = 4 (score moyen = 0.22).**

Le diagramme confirme que les clusters 0 et 2 sont les plus homogènes, le cluster 1 (peu peuplé, 8 stations) présentant la silhouette la plus faible du fait de l'hétérogénéité des situations de forte pollution.

### 3.3 Caractérisation des clusters

![Heatmap clusters](outputs/figures/tp2_heatmap_clusters.png)

**Figure 8 — Profil moyen des clusters (z-score en couleur, valeur brute annotée).**

![Boxplots](outputs/figures/tp2_boxplots_clusters.png)

**Figure 9 — Distribution des paramètres discriminants par cluster.**

**Tableau 4 — Valeurs moyennes des paramètres clés par cluster**

| | C0 (n=57) | C1 (n=8) | C2 (n=97) | C3 (n=66) |
|---|---|---|---|---|
| Conductivité (µS/cm) | **119** | **1 079** | 509 | 929 |
| Température (°C) | 9.3 | 11.5 | 11.6 | 11.3 |
| O₂ dissous (mg/L) | **10.7** | **7.8** | 10.0 | 9.4 |
| Nitrates (mg/L) | **3.8** | **20.5** | 12.4 | 13.0 |
| Ammonium (mg/L) | 0.07 | **1.78** | 0.10 | 0.16 |
| DBO₅ (mg/L) | **1.3** | **3.8** | 1.4 | 1.8 |
| DCO (mg/L) | **9.8** | **24.1** | 9.5 | 16.0 |
| Phosphore total (mg/L) | 0.05 | 0.47 | 0.08 | 0.14 |
| pH | **7.48** | 7.94 | 8.02 | 7.99 |

- **Cluster 0** — très faible conductivité, nitrates quasi absents, O₂ élevé, pH légèrement acide : eaux peu minéralisées de type montagnard.
- **Cluster 1** — conductivité extrême (×9 vs C0), ammonium élevé, DBO₅ et DCO très hautes, O₂ faible : stations fortement impactées.
- **Cluster 2** — profil intermédiaire équilibré, nitrates modérés, bonne oxygénation : stations de plaine à pression diffuse.
- **Cluster 3** — conductivité élevée, DCO et MES significatives, nitrates similaires à C2 : pression cumulée azotée et organique.

---

## 4. Visualisation géographique

### 4.1 Données mobilisées

Le fichier `Stations.CSV` fournit les coordonnées Lambert 93 (EPSG:2154) des 228 stations. Le fond cartographique est le **Corine Land Cover 2012** (CLC12_RACAL_RGF.shp), millésime le plus proche de l'année d'étude 2013, couvrant le bassin Rhin-Meuse / Grand Est.

### 4.2 Carte des clusters

![Carte CLC niveau 2](outputs/figures/tp3_carte_clusters_clc.png)

**Figure 10 — Clusters de qualité d'eau 2013 sur fond CLC 2012 (niveau 2).**

![Carte CLC niveau 1](outputs/figures/tp3_carte_clusters_clc_niv1.png)

**Figure 11 — Clusters de qualité d'eau 2013 sur fond CLC 2012 (niveau 1, grandes catégories).**

La carte révèle une organisation spatiale nette : les stations du Cluster 0 (bleu) se concentrent sur les reliefs vosgiens à l'ouest et les têtes de bassins, tandis que les clusters 2 et 3 occupent la plaine rhénane et mosellane. Les 8 stations du Cluster 1 (très pollué) se localisent ponctuellement dans les vallées industrielles.

---

## 5. Interprétation

### 5.1 Tableau croisé cluster × occupation du sol

![Heatmap CLC](outputs/figures/tp3_heatmap_cluster_clc.png)

**Figure 12 — Répartition de l'occupation du sol (CLC niveau 2) par cluster (% de stations).**

**Tableau 5 — Occupation du sol dominante par cluster (% stations, CLC 2012 niveau 2)**

| Code CLC | Type | C0 (n=57) | C1 (n=8) | C2 (n=97) | C3 (n=66) |
|---|---|---|---|---|---|
| 11 | Zones urbanisées | 28.1 % | 0 % | 24.7 % | 24.2 % |
| 12 | Zones industrielles | 0 % | **12.5 %** | 3.1 % | 3.0 % |
| 13 | Mines / décharges | 0 % | **12.5 %** | 0 % | 0 % |
| 21 | Terres arables | 1.8 % | 12.5 % | **11.3 %** | **13.6 %** |
| 23 | Prairies | 26.3 % | 25.0 % | **27.8 %** | **30.3 %** |
| 24 | Zones agr. hétérogènes | 10.5 % | 12.5 % | 9.3 % | 15.2 % |
| 31 | Forêts | **28.1 %** | 25.0 % | 7.2 % | 4.5 % |
| 51 | Eaux continentales | 0 % | 0 % | 13.4 % | 7.6 % |

### 5.2 Interprétation par cluster

**Cluster 0 — Eaux de montagne, qualité naturelle (57 stations)**  
Ce cluster présente le profil le plus pristine du bassin : conductivité très faible (119 µS/cm, reflétant un substrat géologique pauvre en ions — grès vosgien et granite), nitrates quasi absents (3.8 mg/L), fort taux d'O₂ et pH légèrement acide (7.48). La forte proportion de forêts (28.1 %) et de prairies (26.3 %) dans l'environnement de ces stations, ainsi que leur localisation sur les reliefs des Vosges (altitude > 300 m), confirment l'absence de pression agricole intensive ou industrielle. Les 28.1 % de zones urbanisées (code 11) correspondent à de petites communes du massif vosgien sans impact significatif sur la qualité de l'eau.

**Cluster 1 — Eaux fortement dégradées, pression industrielle (8 stations)**  
Ce petit groupe concentre les stations les plus impactées du bassin : conductivité extrêmement élevée (1 079 µS/cm), trahissant des apports minéraux massifs (exploitation potassique en Alsace, rejets industriels), ammonium élevé (1.78 mg/L — signe de dégradation de matière organique), DBO₅ et DCO les plus hautes, O₂ le plus faible. La présence unique de codes CLC 12 (zones industrielles, 12.5 %) et 13 (mines/décharges, 12.5 %) dans ce cluster confirme un impact anthropique direct. Ces stations sont localisées dans des vallées fortement industrialisées (secteur potassique haut-rhinois, confluences industrielles de la Moselle).

**Cluster 2 — Eaux des plaines à pression diffuse (97 stations, cluster majoritaire)**  
Avec 97 stations, ce cluster représente le profil le plus répandu du bassin. La conductivité modérée (509 µS/cm), les nitrates à 12.4 mg/L (pression agricole diffuse) et la bonne oxygénation suggèrent des stations de plaine soumises à un lessivage agricole sans pollution ponctuelle majeure. La forte proportion de prairies (27.8 %) et d'eaux continentales (13.4 % — stations sur le Rhin et grands cours d'eau) caractérise l'environnement. L'ammonium reste bas (0.10 mg/L), ce qui exclut un impact domestique significatif.

**Cluster 3 — Eaux des plaines industrialisées et périurbaines (66 stations)**  
Ce cluster se distingue du C2 par une conductivité bien plus élevée (929 µS/cm), une DCO significative (16 mg/L) et des MES importantes (26.7 mg/L), indiquant des apports organiques et minéraux plus importants. L'occupation du sol est dominée par les prairies (30.3 %) et les zones agricoles hétérogènes (15.2 %), mais la forte présence urbaine (24.2 % — code 11) reflète l'influence des agglomérations (Metz, Nancy, Mulhouse, Strasbourg périphérique). La DCO élevée par rapport au C2 suggère des rejets organiques diffus liés aux zones d'activité.

### 5.3 Lien avec le relief

| | C0 | C1 | C2 | C3 |
|---|---|---|---|---|
| **Relief** | Vosges (> 300 m) | Plaine / vallées ind. | Plaine rhénane | Plaine mosellane/périurb. |
| **Pression dominante** | Naturelle | Industrielle/minière | Agricole diffuse | Urbaine + organique |
| **Qualité globale** | Excellente | Médiocre à mauvaise | Passable | Médiocre |

Le gradient altitudinal est le premier facteur structurant : l'opposition Vosges (C0) / Plaine (C2, C3) est clairement lisible sur la carte. La surreprésentation industrielle dans C1 correspond aux corridors de la Moselle industrielle et du secteur potassique alsacien, deux zones de pression ponctuelle intense. Les clusters C2 et C3, bien que situés tous deux en plaine, se différencient principalement par la densité d'infrastructure urbaine et industrielle autour des stations, reflétée dans la conductivité et la DCO.

---

*Données : NAIADES / Agence de l'eau Rhin-Meuse — CLC 2012 / SDES. Traitement : Python (pandas, scikit-learn, geopandas). Clustering KMeans, k = 4, standardisation z-score.*
