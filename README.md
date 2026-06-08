# Hackathon — Sujet 1 : Système de Recommandation Utilisateur

> Pipeline complet de data science : génération de données, nettoyage, analyse statistique et moteur de recommandation hybride.

---

## Aperçu du projet

Ce projet simule un système de recommandation de contenu personnalisé, construit de A à Z :

1. Génération d'un dataset réaliste avec anomalies intégrées
2. Nettoyage et prétraitement structuré
3. Validation statistique par test du Chi-carré
4. Moteur de recommandation hybride (statique + dynamique + collaboratif)
5. Dashboard de visualisation

---

## Structure du pipeline

```
ÉTAPE 1  →  Génération du dataset sale        (600 utilisateurs + anomalies natives)
ÉTAPE 2  →  Prétraitement (PP1 à PP6)         (doublons, NaN, âges aberrants)
ÉTAPE 3  →  Analyse statistique (Chi²)        (corrélation intérêts × actions)
ÉTAPE 3.1→  Enrichissement (MAIN_LOG_CAT)     (catégorie comportementale réelle)
ÉTAPE 4  →  Moteur de recommandation          (OOP + Cosine Similarity)
ÉTAPE 5  →  Dashboard (8 visualisations)      (matplotlib + seaborn)
```

---

## Stack technique

| Bibliothèque | Usage |
|---|---|
| `pandas` | Manipulation des DataFrames |
| `numpy` | Calculs numériques et vectorisation |
| `scipy` | Test Chi-carré + distance cosinus |
| `matplotlib` / `seaborn` | Visualisations et dashboard |
| `random` | Génération aléatoire reproductible |

---

## Données générées

Le dataset est généré synthétiquement avec des anomalies intentionnelles simulant un vrai formulaire de saisie :

| Anomalie | Probabilité | Traitement |
|---|---|---|
| Âge manquant (NaN) | 6% | Imputation par la médiane |
| Âge négatif | 1% | Remplacement par la médiane |
| Âge aberrant (> 80) | 1% | Remplacement par la médiane |
| Centre d'intérêt manquant | 5% | Imputation par le mode |
| Nom vide | 3% | Suppression de la ligne |
| Doublon (double soumission) | ~5 lignes | Suppression |

**Dataset brut : 605 lignes → Dataset nettoyé : 575 lignes**

---

## Résultat du test statistique

```
Test du Chi-carré (Centre_interet × actions)
─────────────────────────────────────────────
Chi²    = 9174.10
p-value = 0.00e+00
Degrés  = 145
─────────────────────────────────────────────
Résultat : Corrélation significative ✓  (p < 0.05)
```

H0 rejetée : il existe une relation statistiquement significative entre le centre d'intérêt d'un utilisateur et les actions qu'il effectue.

---

## Moteur de recommandation

Le moteur est hybride : il combine trois approches complémentaires.

```
┌─────────────────────────────────────────────────────┐
│            RecommendationEngine (OOP)               │
├─────────────────────┬───────────────────────────────┤
│  Statique           │  Basée sur le profil           │
│                     │  d'inscription (Centre_interet)│
├─────────────────────┼───────────────────────────────┤
│  Dynamique          │  Basée sur l'activité réelle   │
│                     │  (analyse des logs récents)    │
├─────────────────────┼───────────────────────────────┤
│  Collaborative      │  Top 3 utilisateurs similaires │
│                     │  via similarité cosinus (SciPy)│
└─────────────────────┴───────────────────────────────┘
```

### Exemple de sortie

```
→ Utilisateur : Yann Adingra (44 ans)
• Intérêt déclaré : CULTURE

[STATIQUE]    → Expo Louvre virtuelle, Podcast Historia, Livre Prix Goncourt
[DYNAMIQUE]   → Expo Louvre virtuelle, Podcast Historia, Livre Prix Goncourt
[SIMILAIRES]  → Alice Akpa, Grace Tape, Sébastien Akpa
```

---

## Catégories et actions

| Catégorie | Actions disponibles |
|---|---|
| Tech | vue_tuto_python, telechargement_app, clic_actu_ia, achat_gadget, vue_conf_tech |
| Fitness | achat_produit_sport, vue_tuto_exercice, inscription_salle, achat_supplement, vue_challenge_fit |
| Musique | ecoute_chanson, achat_album, recherche_artiste, partage_playlist, vue_concert_live |
| Danse | vue_video_danse, inscription_cours, partage_performance, achat_tenue_danse, like_choreographie |
| Culture | lecture_article_histoire, clic_expo_musee, vue_docu_art, achat_livre, visite_virtuelle |
| Tourisme | clic_article_voyage, recherche_hotel, vue_video_plage, reservation_vol, achat_guide |

---

## Lancer le projet

```bash
# Cloner le dépôt
git clone https://github.com/amosagekouassi-source/HACKATHON1-TTA-W3

# Ouvrir dans Google Colab ou Jupyter
# Exécuter les cellules dans l'ordre (1 → 14)
```

> Les graines aléatoires sont fixées (`random.seed(42)`, `np.random.seed(42)`) : les résultats sont entièrement reproductibles.

---

## Auteurs

Projet réalisé dans le cadre du **Hackathon TTA — W3**
