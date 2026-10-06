# Qui utilise mon appli ?

**Catégorie** : à confirmer (projet d'école, MOD 7.2 Introduction à la science des données)

## But

Compétition Kaggle de classe : prédire quel utilisateur (trigramme) a réalisé une session du logiciel Copilote
(Infologic, agroalimentaire) à partir de la trace de ses actions. Classification multi-classe supervisée,
247 utilisateurs, 3 279 sessions d'entraînement, 324 sessions de test.

Les utilisateurs sont des testeurs internes d'Infologic qui reportent les problèmes sous leur vrai profil ; l'enjeu
est de reconnaître quelqu'un à sa façon d'utiliser le logiciel (application possible : détection d'intrusion).

**Métrique officielle** : score F1 moyen. En validation on suit le F1 macro et l'accuracy (validation croisée
stratifiée, K = 4).

Description détaillée du dataset : [doc Dataset : Qui utilise mon appli ?](https://claude.ai/code/artifact/914223c7-23d0-4c57-829a-92c595cff82a)

## Contenu

| Fichier | Rôle |
| --- | --- |
| `notebook_consigne.ipynb` | Notebook fourni par les enseignants (étapes et squelette de code) |
| `exploration_dataset.ipynb` | Exploration du dataset : format, parsing des actions, chiffres clés, glossaire |
| `modelisation.ipynb` | Features (TF-IDF des actions, écrans, confs, chaînes + rythme + navigateur), comparaison de modèles, génération de `submission.csv` |
| `qui-utilise-mon-appli-v-2026-2027-groupe-2/` | Données Kaggle : `train.csv.GZ`, `test.csv.GZ`, `sample_submission.csv` |

## Stack

Python, pandas, matplotlib, scikit-learn.

## Lancer

```bash
pip install pandas matplotlib scikit-learn jupyter
jupyter lab  # ou ouvrir les notebooks dans VS Code, depuis la racine du projet
```

Les notebooks lisent les données depuis `qui-utilise-mon-appli-v-2026-2027-groupe-2/` (fichiers `.GZ` laissés compressés).

## Résultats

Validation croisée stratifiée à 4 plis sur le train :

| Modèle | Accuracy | F1 macro |
| --- | --- | --- |
| SVM linéaire (C = 3) | 0,964 | 0,960 |
| Régression logistique | 0,942 | 0,934 |
| Random Forest (500 arbres) | 0,937 | 0,914 |

`modelisation.ipynb` écrit `submission.csv` à la racine, à soumettre sur Kaggle.
