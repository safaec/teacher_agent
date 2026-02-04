# Chapitre 9 : Chargement vers le cloud

**Durée estimée : 4-5h**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Écrire** des données depuis Python vers AWS S3 en utilisant boto3 et pandas
2. **Organiser** un data lake avec une structure professionnelle (raw/processed/final)
3. **Documenter** un dataset avec un data dictionary complet

---

## 🎯 L'accroche : Le paradoxe du data scientist isolé

Imaginez ce scénario : vous avez passé trois jours à nettoyer un dataset de 2 millions de lignes. Vos transformations sont parfaites. Vos colonnes sont impeccables. Votre EDA révèle des insights précieux.

Puis votre ordinateur plante.

Tout est perdu.

Ou pire encore : votre collègue vous demande le dataset pour son modèle de machine learning. Vous lui envoyez le fichier par email — 500 Mo. Il échoue. Vous essayez WeTransfer. Ça prend 45 minutes. Le lendemain, il vous dit que le fichier est corrompu.

**C'est exactement pourquoi les entreprises utilisent le cloud.**

Selon AWS, les organisations qui centralisent leurs données sur S3 réduisent de 70% le temps passé à chercher et partager des fichiers. *(Source : AWS Well-Architected Framework, 2024)*
