# Chapitre 5 — Nettoyage des données

**Durée estimée : 10-12 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Appliquer** différentes stratégies de traitement des valeurs manquantes (suppression, imputation)
2. **Identifier et supprimer** les doublons en préservant les informations pertinentes
3. **Traiter** les valeurs aberrantes selon le contexte métier
4. **Nettoyer** les types de données (dates, numériques, texte) pour les rendre exploitables

---

## 🎯 Le Hook : La startup qui a perdu 2 millions à cause de doublons

Aux États-Unis, les hôpitaux s'appuient sur le numérique pour sauver des vies. Mais une étude de Black Book Research a révélé une faille critique. Le problème ? Leurs bases de données contiennent en moyenne 18 % de doublons.

Résultat : Les médecins accèdent à des dossiers incomplets, manquant parfois des allergies vitales. Selon l'Institut ECRI, 9 % de ces erreurs d'identité entraînent directement des préjudices physiques ou des décès. Perte totale : 1,5 million de dollars par an pour un hôpital moyen.

Le nettoyage des données n'est pas un luxe académique. C'est une **nécessité business**.

> 💭 **Question Socratique #1** : Cette erreur aurait-elle pu être évitée par une simple requête SQL de dé-duplication ? Ou le problème était-il plus profond (processus, culture, outils) ?

> 💭 **Réponse** :

Une simple requête SQL (`SELECT DISTINCT` ou un `GROUP BY`) aurait été totalement **inutile** et potentiellement **dangereuse** dans ce contexte.

Le problème est beaucoup plus profond qu'une ligne de code. Voici pourquoi le SQL échoue et pourquoi c'est un problème de **processus et d'outils complexes** :

**1. L'échec technique du SQL (Le piège de la correspondance exacte)**

Le SQL est un langage binaire et rigide. Il fonctionne sur la **correspondance exacte** (*Exact Matching*).
Pour qu'une requête SQL supprime un doublon, les deux lignes doivent être strictement identiques.

* **Ce que voit le SQL :**
* Ligne A : `Jonathan Smith | 12/05/1980 | 123 Main St`
* Ligne B : `Jon Smith | 12/05/1980 | 123 Main Street`

* **La conclusion du SQL :** "Ces deux lignes sont différentes. Ce sont deux personnes distinctes."

Si vous forcez le dédoublonnage SQL uniquement sur le nom de famille et la date de naissance, vous risquez de fusionner deux homonymes (deux "Smith" nés le même jour) qui n'ont rien à voir. Dans un hôpital, cela signifie mélanger les groupes sanguins de deux inconnus. C'est mortel.

**La solution technique nécessaire :** Il ne faut pas du SQL, mais des algorithmes de **Logique Floue (Fuzzy Logic)** et de **Matching Probabiliste**. L'algorithme doit calculer un score de ressemblance : *"Il y a 95% de chances que Jon et Jonathan habitant à la même adresse soient la même personne".* Le SQL standard ne sait pas faire ça.

**2. Le problème de Processus (Le "Point d'Entrée")**

Les doublons ne naissent pas dans la base de données, ils naissent **au bureau d'accueil**.

* **Scénario typique :** Un patient arrive aux urgences, stressé. L'agent d'accueil tape vite. Il cherche "Bill Smith". Le système ne trouve rien (car le patient est enregistré sous "William Smith").
* **L'erreur de processus :** Au lieu de chercher des variantes, l'agent, pressé par la file d'attente, clique sur "Créer nouveau patient". Le doublon est né.
* **La solution Process :** Il faut imposer une étape de validation obligatoire ou utiliser des lecteurs biométriques (paume de la main, reconnaissance faciale) à l'entrée, ce que certains hôpitaux américains commencent à faire pour contourner l'erreur humaine.

**4. Le problème Culturel (Silos de données)**

Souvent, chaque département (Laboratoire, Radiologie, Urgences, Pharmacie) utilise son propre logiciel.

* Le logiciel de la pharmacie appelle le patient "J. Smith".
* Le logiciel de radiologie l'appelle "Jonathan Smith".
* Ces systèmes ne se parlent pas en temps réel.

**En résumé**

Ce n'était pas une erreur de codeur débutant (oublier un `DISTINCT`). C'était un problème de **Master Data Management (MDM)**.

Pour éviter ces pertes de 1,5 million de dollars, les hôpitaux doivent investir dans des **EMPI (Enterprise Master Patient Index)**. Ce sont des logiciels coûteux et complexes qui utilisent l'intelligence artificielle pour dire : *"Attention, William et Bill semblent être la même personne, voulez-vous fusionner ?"*.

Une simple requête SQL n'aurait fait qu'aggraver le problème en supprimant potentiellement des données valides.
