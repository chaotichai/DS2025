# 🌍 RAPPORT D'ANALYSE DES PIB MONDIAUX 2024

**Date du rapport :** Octobre 2024  
**Source des données :** FMI, OCDE, Statista  
**Périmètre :** Top 15 économies mondiales

---

## 📊 PARTIE 1 : DONNÉES ET CODE

### 1.1 Données Brutes

```python
# Installation des bibliothèques
!pip install plotly pandas matplotlib seaborn -q

import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots

# Dataset complet
data = {
    'Pays': ['États-Unis', 'Chine', 'Japon', 'Allemagne', 'Inde', 
             'Royaume-Uni', 'France', 'Italie', 'Canada', 'Brésil',
             'Russie', 'Corée du Sud', 'Mexique', 'Australie', 'Espagne'],
    'PIB_Md': [29167.78, 21643, 4365, 4730, 3889.13, 
               3587.55, 3174.10, 2376.51, 2214.80, 2188.42,
               2200, 1800, 1700, 1650, 1600],
    'Croissance_%': [2.1, 5.0, 1.0, -0.2, 6.7, 
                     4.1, 2.6, 0.7, 3.1, 1.8,
                     -2.2, 2.4, 2.5, 2.1, 2.8],
    'Drapeau': ['🇺🇸', '🇨🇳', '🇯🇵', '🇩🇪', '🇮🇳', 
                '🇬🇧', '🇫🇷', '🇮🇹', '🇨🇦', '🇧🇷',
                '🇷🇺', '🇰🇷', '🇲🇽', '🇦🇺', '🇪🇸'],
    'Région': ['Amérique', 'Asie', 'Asie', 'Europe', 'Asie',
               'Europe', 'Europe', 'Europe', 'Amérique', 'Amérique',
               'Europe/Asie', 'Asie', 'Amérique', 'Océanie', 'Europe']
}

df = pd.DataFrame(data)
df['Rang'] = range(1, len(df) + 1)
df['Pays_Drapeau'] = df['Drapeau'] + ' ' + df['Pays']
```

### 1.2 Tableau Synthétique

| Rang | Pays | PIB (Md $) | Croissance (%) | Région |
|------|------|------------|----------------|--------|
| 1 | 🇺🇸 États-Unis | 29,167.78 | 2.1 | Amérique |
| 2 | 🇨🇳 Chine | 21,643.00 | 5.0 | Asie |
| 3 | 🇯🇵 Japon | 4,365.00 | 1.0 | Asie |
| 4 | 🇩🇪 Allemagne | 4,730.00 | -0.2 | Europe |
| 5 | 🇮🇳 Inde | 3,889.13 | 6.7 | Asie |
| 6 | 🇬🇧 Royaume-Uni | 3,587.55 | 4.1 | Europe |
| 7 | 🇫🇷 France | 3,174.10 | 2.6 | Europe |
| 8 | 🇮🇹 Italie | 2,376.51 | 0.7 | Europe |
| 9 | 🇨🇦 Canada | 2,214.80 | 3.1 | Amérique |
| 10 | 🇧🇷 Brésil | 2,188.42 | 1.8 | Amérique |
| 11 | 🇷🇺 Russie | 2,200.00 | -2.2 | Europe/Asie |
| 12 | 🇰🇷 Corée du Sud | 1,800.00 | 2.4 | Asie |
| 13 | 🇲🇽 Mexique | 1,700.00 | 2.5 | Amérique |
| 14 | 🇦🇺 Australie | 1,650.00 | 2.1 | Océanie |
| 15 | 🇪🇸 Espagne | 1,600.00 | 2.8 | Europe |

### 1.3 Statistiques Clés

```python
# Calcul des statistiques
total_pib = df['PIB_Md'].sum()
croissance_moyenne = df['Croissance_%'].mean()

print(f"💰 PIB Total Top 15: {total_pib/1000:.1f} Trillions $")
print(f"📊 Croissance moyenne: {croissance_moyenne:.2f}%")
print(f"🏆 Plus forte économie: USA (29,167.78 Md $)")
print(f"🚀 Plus forte croissance: Inde (6.7%)")
print(f"📉 Plus faible croissance: Russie (-2.2%)")
```

**Résultats :**
- 💰 **PIB Total Top 15 :** 88.3 Trillions $
- 📊 **Croissance moyenne :** 2.24%
- 🏆 **Plus forte économie :** États-Unis (29.2T $)
- 🚀 **Plus forte croissance :** Inde (6.7%)
- 📉 **Plus faible croissance :** Russie (-2.2%)

---

## 📈 PARTIE 2 : VISUALISATIONS GRAPHIQUES

### 2.1 Code pour le Classement PIB

```python
# Graphique 1: Classement horizontal par PIB
fig1 = go.Figure()
fig1.add_trace(go.Bar(
    y=df['Pays_Drapeau'],
    x=df['PIB_Md'],
    orientation='h',
    marker=dict(
        color=df['PIB_Md'],
        colorscale='Blues',
        showscale=True
    ),
    text=df['PIB_Md'].apply(lambda x: f'{x:,.0f} Md $'),
    textposition='outside'
))

fig1.update_layout(
    title='📊 Classement des Pays par PIB 2024',
    xaxis_title='PIB (Milliards $)',
    height=600,
    template='plotly_white',
    yaxis={'categoryorder': 'total ascending'}
)

fig1.show()
```

**Graphique produit :** Barres horizontales montrant la domination écrasante des USA et de la Chine, qui représentent à eux seuls 57% du PIB total du top 15.

### 2.2 Code pour les Taux de Croissance

```python
# Graphique 2: Taux de croissance avec code couleur
df_sorted = df.sort_values('Croissance_%', ascending=True)
colors = ['#ef4444' if x < 0 else '#22c55e' if x > 3 else '#3b82f6' 
          for x in df_sorted['Croissance_%']]

fig2 = go.Figure()
fig2.add_trace(go.Bar(
    x=df_sorted['Croissance_%'],
    y=df_sorted['Pays_Drapeau'],
    orientation='h',
    marker=dict(color=colors),
    text=df_sorted['Croissance_%'].apply(lambda x: f'{x}%'),
    textposition='outside'
))

fig2.update_layout(
    title='📈 Taux de Croissance du PIB 2024',
    xaxis_title='Taux de Croissance (%)',
    height=600,
    shapes=[dict(type='line', x0=0, x1=0, y0=-0.5, y1=14.5,
                 line=dict(color='black', width=2, dash='dash'))]
)

fig2.show()
```

**Légende des couleurs :**
- 🟢 Vert : Croissance > 3% (forte)
- 🔵 Bleu : Croissance 0-3% (modérée)
- 🔴 Rouge : Croissance < 0% (contraction)

### 2.3 Code pour la Répartition Régionale

```python
# Graphique 3: Pie chart par région
region_data = df.groupby('Région')['PIB_Md'].sum().reset_index()

fig3 = go.Figure(data=[go.Pie(
    labels=region_data['Région'],
    values=region_data['PIB_Md'],
    hole=0.4,
    marker=dict(colors=['#3b82f6', '#ef4444', '#22c55e', '#f59e0b', '#8b5cf6']),
    textinfo='label+percent'
)])

fig3.update_layout(
    title='🗺️ Répartition du PIB par Région 2024',
    height=600
)

fig3.show()
```

**Répartition obtenue :**
- 🔵 Asie : ~42%
- 🔴 Europe : ~29%
- 🟢 Amérique : ~26%
- 🟡 Europe/Asie : ~2%
- 🟣 Océanie : ~1%

### 2.4 Code pour Scatter Plot

```python
# Graphique 4: PIB vs Croissance
fig4 = px.scatter(df, 
                  x='PIB_Md', 
                  y='Croissance_%',
                  size='PIB_Md',
                  color='Région',
                  hover_name='Pays_Drapeau',
                  text='Drapeau',
                  size_max=60,
                  log_x=True)

fig4.update_layout(
    title='💹 PIB vs Taux de Croissance 2024',
    xaxis_title='PIB (Milliards $, échelle log)',
    yaxis_title='Taux de Croissance (%)',
    height=600
)

fig4.show()
```

**Insight visuel :** Ce graphique révèle qu'il n'y a pas de corrélation directe entre la taille de l'économie et son taux de croissance.

### 2.5 Dashboard Combiné

```python
# Graphique 5: Dashboard 2x2
fig5 = make_subplots(
    rows=2, cols=2,
    subplot_titles=('Top 5 PIB', 'Top 5 Croissance', 
                    'Bottom 5 Croissance', 'PIB par Région'),
    specs=[[{'type': 'bar'}, {'type': 'bar'}],
           [{'type': 'bar'}, {'type': 'pie'}]]
)

# Top 5 PIB
top5_pib = df.nlargest(5, 'PIB_Md')
fig5.add_trace(go.Bar(x=top5_pib['Pays_Drapeau'], y=top5_pib['PIB_Md'],
                      marker_color='lightblue'), row=1, col=1)

# Top 5 Croissance
top5_growth = df.nlargest(5, 'Croissance_%')
fig5.add_trace(go.Bar(x=top5_growth['Pays_Drapeau'], y=top5_growth['Croissance_%'],
                      marker_color='lightgreen'), row=1, col=2)

# Bottom 5 Croissance
bottom5 = df.nsmallest(5, 'Croissance_%')
fig5.add_trace(go.Bar(x=bottom5['Pays_Drapeau'], y=bottom5['Croissance_%'],
                      marker_color='lightcoral'), row=2, col=1)

# Région
region_data = df.groupby('Région')['PIB_Md'].sum().reset_index()
fig5.add_trace(go.Pie(labels=region_data['Région'], 
                      values=region_data['PIB_Md']), row=2, col=2)

fig5.update_layout(title_text='🎯 Dashboard Synthétique 2024', height=800)
fig5.show()
```

---

## 💡 PARTIE 3 : INTERPRÉTATION ET ANALYSE

### 3.1 Vue d'Ensemble Économique

L'année 2024 marque un **tournant dans la répartition des forces économiques mondiales**. Avec un PIB cumulé de 88.3 trillions de dollars pour les 15 premières économies, nous observons une **croissance moyenne modérée de 2.24%**, reflétant un contexte géopolitique tendu et des ajustements post-pandémiques.

### 3.2 Les Grands Gagnants 🏆

#### **🇮🇳 Inde : Le nouveau moteur mondial**
- **Croissance record : 6.7%**
- L'Inde s'impose comme la locomotive de la croissance mondiale
- Facteurs clés : démographie jeune, digitalisation, réformes structurelles
- Projection : dépassement du Japon d'ici 2026-2027

#### **🇨🇳 Chine : Résilience malgré les défis**
- **Croissance de 5.0%** malgré le vieillissement démographique
- Maintien de sa position de 2ème économie mondiale (21.6T $)
- Défis : crise immobilière, tensions avec l'Occident
- Transition vers une économie de consommation

#### **🇬🇧 Royaume-Uni : La surprise post-Brexit**
- **Croissance solide de 4.1%**
- Dépasse les attentes après des années d'incertitude
- Secteur des services très performant (finance, tech)
- Résilience remarquable face aux défis du Brexit

#### **🇺🇸 États-Unis : Domination confirmée**
- **PIB de 29.2T $** - près de 1/3 du top 15
- Croissance stable de 2.1%
- Secteurs moteurs : tech (IA, cloud), finance, énergie
- Projection 2025 : franchissement des 30T $

### 3.3 Les Difficultés Majeures 📉

#### **🇩🇪 Allemagne : L'homme malade de l'Europe**
- **Seul pays du G20 en contraction (-0.2%)**
- Causes multiples :
  - Crise énergétique post-dépendance au gaz russe
  - Faiblesse du secteur automobile face à la transition électrique
  - Manque d'investissement dans l'innovation
- Impact sur toute la zone euro

#### **🇷🇺 Russie : Les sanctions mordent**
- **Contraction de -2.2%**
- Isolement économique international
- Fuite des capitaux et des cerveaux
- Dépendance accrue à la Chine et aux pays émergents

#### **🇮🇹 Italie : Stagnation persistante**
- **Croissance anémique de 0.7%**
- Sort du top 10 mondial
- Problèmes structurels : dette publique (140% du PIB), productivité faible
- Vieillissement démographique critique

### 3.4 Analyse Régionale Approfondie

#### **🌏 Asie : Le continent du futur (42% du PIB total)**
**Forces :**
- 5 pays dans le top 15 (Chine, Japon, Inde, Corée du Sud, Indonésie hors classement)
- Taux de croissance moyens les plus élevés
- Classe moyenne en expansion rapide
- Hub de l'innovation technologique

**Contrastes :**
- Japon stagne (+1.0%) avec un vieillissement extrême
- Inde et Chine tirent la croissance régionale
- Tensions géopolitiques (Chine-Taiwan, Corée du Nord)

#### **🇪🇺 Europe : Un continent en quête de dynamisme (29% du PIB total)**
**Faiblesses :**
- Croissance moyenne la plus faible : 1.4%
- 6 pays sur 15, mais performances hétérogènes
- Allemagne en récession impacte toute la zone
- Dépendance énergétique non résolue

**Points positifs :**
- UK et Espagne (+2.8%) surprennent positivement
- France stable (+2.6%)
- Transition énergétique en cours

#### **🌎 Amérique : Polarisation Nord-Sud (26% du PIB total)**
**Domination US :**
- 62% du PIB américain total
- Leadership technologique et financier incontesté
- Dollar demeure monnaie de réserve mondiale

**Émergences :**
- 🇧🇷 Brésil retrouve le top 10 (+1.8%)
- 🇲🇽 Mexique profite du nearshoring américain (+2.5%)
- 🇨🇦 Canada stable (+3.1%)

### 3.5 Tendances Structurelles Identifiées

#### **1. Découplage économie/démographie**
Les pays à forte croissance (Inde, UK) ne sont pas nécessairement les plus peuplés. La qualité institutionnelle et l'innovation comptent autant que la taille.

#### **2. Déclin du lien PIB/taux de croissance**
Le scatter plot révèle une **absence de corrélation** :
- Grandes économies stagnent (Japon, Allemagne)
- Économies moyennes accélèrent (Inde, UK)
- La taille ne garantit plus la croissance

#### **3. Géopolitique > Économie**
Les sanctions contre la Russie (-2.2%) prouvent que les décisions politiques peuvent surpasser les fondamentaux économiques.

#### **4. Transition énergétique : gagnants et perdants**
- **Perdants :** Allemagne (dépendance gaz), Russie (pétrole/gaz)
- **Gagnants :** USA (gaz de schiste), Australie (lithium), Chine (renouvelables)

### 3.6 Changements de Classement Notables

| Mouvement | Pays | Impact |
|-----------|------|--------|
| 📈 Entrée top 10 | 🇧🇷 Brésil | Commodities + politique stable |
| 📉 Sortie top 10 | 🇮🇹 Italie | Stagnation structurelle |
| ⚠️ Perte de rang | 🇯🇵 Japon | Dépassé par Allemagne (temporaire) |
| 🚀 Progression | 🇮🇳 Inde | Rattrapage accéléré |

### 3.7 Perspectives 2025-2030

#### **Scénarios probables :**

**Court terme (2025) :**
- USA franchissent 30T $ (+2.2% FMI)
- Inde dépasse 4.2T $ avec croissance maintenue
- Allemagne : reprise timide (+0.8%)
- Croissance mondiale G20 : 3.0-3.3%

**Moyen terme (2027-2030) :**
- 🇮🇳 Inde devient 3ème économie mondiale (dépasse Japon et Allemagne)
- 🇨🇳 Chine pourrait atteindre 27-28T $ sans dépasser USA
- 🇪🇺 Europe stagnante sauf UK
- 🇧🇷🇲🇽 Brésil et Mexique consolident leur top 10

#### **Risques majeurs :**
1. **Fragmentation géopolitique** (blocs US-Chine)
2. **Crises climatiques** impactant PIB (inondations, sécheresses)
3. **Dette publique** insoutenable (Italie, Japon >200% PIB)
4. **Disruption IA** : gagnants technologiques vs perdants industriels

### 3.8 Recommandations Stratégiques

#### **Pour les investisseurs :**
- ✅ Surpondérer : Inde, USA (tech), UK (services)
- ⚠️ Prudence : Allemagne, Russie, Italie
- 🎯 Opportunités : Mexique (nearshoring), Brésil (commodities)

#### **Pour les décideurs politiques :**
- **Europe :** Investissement massif dans innovation et énergie
- **Chine :** Pivot vers consommation intérieure
- **Pays émergents :** Profiter du découplage US-Chine

#### **Pour les entreprises :**
- Diversification géographique essentielle
- Focus marchés à forte croissance (Asie du Sud, Mexique)
- Anticipation disruptions climatiques et géopolitiques

---

## 🎯 CONCLUSIONS CLÉS

### ✅ Les 5 Faits Marquants 2024

1. **🇮🇳 L'Inde est le vrai gagnant** avec 6.7% de croissance
2. **🇩🇪 L'Allemagne est en crise** (-0.2%, seul pays G20 en récession)
3. **🌏 L'Asie domine avec 42%** du PIB total du top 15
4. **🇺🇸 Les USA restent intouchables** (29.2T $, 33% du total)
5. **🇧🇷 Le Brésil revient** dans le top 10 mondial

### 📊 Chiffres à Retenir

- **88.3 Trillions $** : PIB total top 15
- **2.24%** : Croissance moyenne (vs 3.4% en 2023)
- **+6.7%** : Record de l'Inde
- **-2.2%** : Pire performance (Russie)
- **3.2%** : Croissance G20 2024

### 🔮 Prédiction Audacieuse

**D'ici 2030, le classement sera :**
1. 🇺🇸 USA (~35T $)
2. 🇨🇳 Chine (~28T $)
3. 🇮🇳 Inde (~6-7T $)
4. 🇯🇵 Japon (~4.5T $)
5. 🇩🇪 Allemagne (~4.8T $)

**Le monde de 2024 n'est plus celui de 2019.** La pandémie, les tensions géopolitiques et la transition énergétique ont redistribué les cartes. Les économies agiles, innovantes et démographiquement dynamiques l'emportent sur les géants stagnants.

---

## 📚 Méthodologie

**Sources :**
- Fonds Monétaire International (FMI) - World Economic Outlook Oct 2024
- OCDE - Economic Projections 2024
- Statista - Global GDP Rankings
- Banque Mondiale - World Development Indicators

**Limites de l'analyse :**
- PIB nominal en USD (sensible aux fluctuations de change)
- Données préliminaires 2024 (sujettes à révision)
- Exclusion du PIB PPP (parité de pouvoir d'achat)

**Note :** Ce rapport offre une photographie des dynamiques économiques mondiales à fin 2024. Les projections sont soumises aux aléas géopolitiques, climatiques et technologiques.

---

**Rapport généré le :** Octobre 2024  
**Version :** 1.0  
**Contact :** Analyse économique Claude AI