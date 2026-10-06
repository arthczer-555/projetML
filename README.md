# Qui utilise mon appli ?

**Catégorie** : à confirmer (projet d'école, MOD 7.2 Introduction à la science des données)

## But

Compétition Kaggle de classe : prédire quel utilisateur (trigramme) a réalisé une session du logiciel Copilote
(Infologic, agroalimentaire) à partir de la trace de ses actions. Classification multi-classe supervisée,
247 utilisateurs, 3 279 sessions d'entraînement, 324 sessions de test.

Description détaillée du dataset : [doc Dataset : Qui utilise mon appli ?](https://claude.ai/code/artifact/914223c7-23d0-4c57-829a-92c595cff82a)

## Contenu

| Fichier | Rôle |
| --- | --- |
| `notebook_consigne.ipynb` | Notebook fourni par les enseignants (étapes et squelette de code) |
| `indications_supplementaires.ipynb` | Copie identique de la consigne pour l'instant |
| `exploration_dataset.ipynb` | Exploration du dataset : format, parsing des actions, chiffres clés, glossaire |
| `qui-utilise-mon-appli-v-2026-2027-groupe-2/` | Données Kaggle : `train.csv.GZ`, `test.csv.GZ`, `sample_submission.csv` |

## Stack

Python, pandas, matplotlib, scikit-learn (modélisation à venir).

## Lancer

```bash
pip install pandas matplotlib jupyter
jupyter lab  # ou ouvrir les notebooks dans VS Code, depuis la racine du projet
```

Les notebooks lisent les données depuis `qui-utilise-mon-appli-v-2026-2027-groupe-2/` (fichiers `.GZ` laissés compressés).
