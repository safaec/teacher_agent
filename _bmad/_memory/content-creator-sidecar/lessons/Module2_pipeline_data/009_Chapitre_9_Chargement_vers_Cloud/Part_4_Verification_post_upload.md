# Chapitre 9 : Chargement vers le cloud

**Durée estimée : 4-5h**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Écrire** des données depuis Python vers AWS S3 en utilisant boto3 et pandas
2. **Organiser** un data lake avec une structure professionnelle (raw/processed/final)
3. **Documenter** un dataset avec un data dictionary complet

---

## 9.4. Vérification post-upload

### 4.1 Ne jamais faire confiance à l'upload

Règle cardinale : **toujours vérifier ce qu'on a écrit**.

Les problèmes courants :

- Fichier corrompu pendant le transfert
- Mauvais encodage
- Colonnes manquantes
- Types modifiés

### 4.2 Checklist de vérification

```python
import pandas as pd

# 1. Relire le fichier uploadé
chemin_s3 = "s3://mon-bucket/processed/clients_clean.parquet"
df_verification = pd.read_parquet(chemin_s3)

# 2. Vérifier les dimensions
assert df_verification.shape == df_clean.shape, "Erreur : dimensions différentes !"

# 3. Vérifier les colonnes
assert list(df_verification.columns) == list(df_clean.columns), "Erreur : colonnes différentes !"

# 4. Vérifier les types
for col in df_clean.columns:
    assert df_verification[col].dtype == df_clean[col].dtype, f"Erreur type : {col}"

# 5. Vérifier un échantillon de valeurs
assert df_verification['client_id'].sum() == df_clean['client_id'].sum(), "Erreur : somme ID différente"

print("✅ Vérification réussie : le fichier S3 est identique au DataFrame source")
```

---

### 4.3 Script de validation réutilisable

```python
def valider_upload_s3(df_source, chemin_s3):
    """
    Valide qu'un fichier uploadé sur S3 correspond au DataFrame source.

    Args:
        df_source: DataFrame original
        chemin_s3: Chemin S3 du fichier uploadé

    Returns:
        bool: True si validation réussie

    Raises:
        AssertionError: Si une vérification échoue
    """
    # Détecter le format
    if chemin_s3.endswith('.parquet'):
        df_uploaded = pd.read_parquet(chemin_s3)
    elif chemin_s3.endswith('.csv'):
        df_uploaded = pd.read_csv(chemin_s3)
    else:
        raise ValueError(f"Format non supporté : {chemin_s3}")

    # Vérifications
    checks = {
        "Nombre de lignes": df_uploaded.shape[0] == df_source.shape[0],
        "Nombre de colonnes": df_uploaded.shape[1] == df_source.shape[1],
        "Noms des colonnes": list(df_uploaded.columns) == list(df_source.columns),
    }

    # Rapport
    for check, result in checks.items():
        status = "✅" if result else "❌"
        print(f"{status} {check}")

    if all(checks.values()):
        print("\n✅ Validation complète réussie")
        return True
    else:
        raise AssertionError("Échec de la validation")

# Utilisation
# valider_upload_s3(df_clean, "s3://mon-bucket/processed/clients_clean.parquet")
```
