# Qui utilise mon appli ?

Projet du cours MOD 7.2 (Introduction à la science des données) : compétition Kaggle
« Qui utilise mon appli ? », groupe 2.

## Le problème

Prédire quel utilisateur (trigramme) a réalisé une session du logiciel Copilote (Infologic, agroalimentaire) à partir
de la seule trace de ses actions. C'est une classification supervisée multi-classe : 247 utilisateurs, 3 279 sessions
d'entraînement, 324 sessions de test. Les utilisateurs sont des testeurs internes d'Infologic ; l'enjeu est de
reconnaître quelqu'un à sa façon d'utiliser le logiciel (application possible : détection d'intrusion).

Métrique officielle : score F1 moyen. En validation, on suit le F1 macro et l'accuracy (validation croisée stratifiée
à 4 plis).

## Contenu du dépôt

| Fichier | Rôle |
| --- | --- |
| `exploration_dataset.ipynb` | Exploration des données : format, découpage des actions, chiffres clés, valeurs aberrantes, glossaire |
| `modelisation.ipynb` | Solution complète : construction des features, comparaison de modèles, réglage, ablation, modèle final, variables les plus influentes, génération de `submission.csv` |
| `submission.csv` | Prédictions sur le jeu de test, au format Kaggle (`RowId,prediction`) |
| `requirements.txt` | Bibliothèques nécessaires (versions testées avec Python 3.14) |
| `notebook_consigne.ipynb` | Notebook fourni par les enseignants (étapes proposées) |
| `qui-utilise-mon-appli-v-2026-2027-groupe-2/` | Données Kaggle : `train.csv.GZ`, `test.csv.GZ`, `sample_submission.csv` |

## Installation et exécution

Testé avec Python 3.14.10. Depuis la racine du dépôt :

```bash
python -m venv .venv
source .venv/bin/activate          # Windows : .venv\Scripts\activate
pip install -r requirements.txt

# Exécuter les notebooks (ou les ouvrir dans Jupyter / VS Code et faire « Run All »)
jupyter nbconvert --to notebook --execute --inplace exploration_dataset.ipynb
jupyter nbconvert --to notebook --execute --inplace modelisation.ipynb
```

Les notebooks lisent les données dans `qui-utilise-mon-appli-v-2026-2027-groupe-2/` (fichiers `.GZ` laissés
compressés). `modelisation.ipynb` écrit `submission.csv` à la racine ; son exécution complète prend une dizaine de
minutes (validations croisées).

## Démarche

1. **Lecture** ligne par ligne (les sessions ont un nombre de champs variable) et nettoyage des tokens.
2. **Features** : chaque session devient un document de mots préfixés (type d'action `T=`, écran `E=`, conf écran
   `C=`, chaîne `H=`, écran courant + action `ET=`, bigramme d'actions `B=`), vectorisé en TF-IDF ; plus le navigateur
   (one-hot) et 6 features de rythme (nombre d'actions, durée, vitesse, inactivité, régularité, part de modifications).
3. **Modèles** comparés en validation croisée stratifiée à 4 plis, dans un `Pipeline` scikit-learn (pas de fuite de
   données) : régression logistique, SVM linéaire, Random Forest.
4. **Modèle final** : SVM linéaire (C = 3, `class_weight="balanced"`) réentraîné sur tout le train.

## Résultats (validation croisée stratifiée à 4 plis)

| Modèle | Accuracy | F1 macro |
| --- | --- | --- |
| SVM linéaire (C = 3, retenu) | 0,964 | 0,960 |
| SVM linéaire (C = 1) | 0,962 | 0,957 |
| Random Forest (500 arbres) | 0,946 | 0,941 |
| Régression logistique (C = 20) | 0,943 | 0,934 |

Ablation sur le SVM : sans le TF-IDF, le F1 macro tombe à 0,038 ; sans le navigateur, à 0,935 ; sans le rythme, il
reste à 0,960.
