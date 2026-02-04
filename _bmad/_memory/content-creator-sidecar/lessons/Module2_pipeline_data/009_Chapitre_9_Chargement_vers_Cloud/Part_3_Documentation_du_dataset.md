# Chapitre 9 : Chargement vers le cloud

**Durée estimée : 4-5h**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Écrire** des données depuis Python vers AWS S3 en utilisant boto3 et pandas
2. **Organiser** un data lake avec une structure professionnelle (raw/processed/final)
3. **Documenter** un dataset avec un data dictionary complet

---

## 9.3. Documentation du dataset

### 3.1 Pourquoi documenter est crucial

Un dataset non documenté est un dataset inutilisable dans 6 mois — même par vous-même.

**Le syndrome du "je me souviendrai"** :

- Que signifie la colonne `flag_x` ?
- Pourquoi y a-t-il des valeurs négatives dans `montant` ?
- Les dates sont-elles en UTC ou en heure locale ?

### 3.2 Le Data Dictionary

Un data dictionary décrit chaque colonne de votre dataset :

```markdown
# Data Dictionary : clients_clean.parquet

## Métadonnées générales
- **Dernière mise à jour** : 2024-01-20
- **Source** : CRM Salesforce (extraction quotidienne)
- **Responsable** : equipe-data@entreprise.com
- **Lignes** : 45,230
- **Colonnes** : 12

## Description des colonnes

| Colonne | Type | Description | Valeurs possibles | Notes |
|---------|------|-------------|-------------------|-------|
| client_id | int64 | Identifiant unique client | 1-999999 | Clé primaire |
| nom | string | Nom complet | Texte libre | Nettoyé (majuscules) |
| email | string | Email principal | format@email.com | Validé par regex |
| date_inscription | datetime | Date création compte | 2015-01-01 à aujourd'hui | Timezone UTC |
| segment | category | Segment marketing | 'premium', 'standard', 'nouveau' | Dérivé du CA annuel |
| ca_annuel | float64 | Chiffre d'affaires 12 mois | 0.0 - 999999.99 | En euros, peut être 0 |
| nb_commandes | int64 | Nombre de commandes | 0 - 9999 | Période 12 mois |
| actif | bool | Client actif ? | True/False | True si commande < 6 mois |

## Transformations appliquées
1. Emails invalides → remplacés par NULL (2.3% des lignes)
2. Doublons sur email → conservé le plus récent (154 lignes supprimées)
3. CA négatifs → corrigés via jointure avec table remboursements
```

---

### 3.3 Générer automatiquement avec l'IA

Vous pouvez utiliser un LLM pour générer un premier draft :

```python
# Prompt pour générer un data dictionary
prompt = f"""
Génère un data dictionary en markdown pour ce DataFrame :

Colonnes : {list(df_clean.columns)}
Types : {df_clean.dtypes.to_dict()}
Statistiques : {df_clean.describe().to_dict()}
Valeurs uniques (catégories) : {df_clean.select_dtypes('object').nunique().to_dict()}

Format demandé : tableau markdown avec Colonne, Type, Description, Valeurs possibles, Notes
"""

print(prompt)
```

> 🤖 **Astuce IA** : Copiez ce prompt dans Claude ou ChatGPT avec vos vraies données pour obtenir un draft en 30 secondes. Vous n'aurez plus qu'à vérifier et compléter.

---

### ✍️ Exercice pratique 4 : Documentez un dataset

Prenez un des DataFrames que vous avez nettoyé dans les chapitres précédents et créez son data dictionary.

Minimum requis :

- [ ] Métadonnées générales (date, source, responsable)
- [ ] Tableau des colonnes avec type et description
- [ ] Au moins 3 transformations documentées
