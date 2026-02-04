# Chapitre 7 — EDA analytique

**Durée estimée : 10-12 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Analyser** la distribution d'une variable et interpréter ses caractéristiques statistiques
2. **Identifier** les relations entre variables (corrélation, associations)
3. **Comparer** des sous-populations et détecter des patterns significatifs
4. **Formuler** des hypothèses à partir des observations et éviter les pièges d'interprétation

---

## 🎯 Le Hook : Comment Target a su qu'une adolescente était enceinte avant son père

En 2012, un père furieux s'est rendu dans un magasin Target aux États-Unis pour se plaindre : sa fille de 16 ans recevait des coupons pour des produits de bébé. "Vous encouragez ma fille à tomber enceinte ?"

Quelques jours plus tard, il a rappelé pour s'excuser. Sa fille était effectivement enceinte. Il ne le savait pas encore.

Comment Target a-t-il deviné ? Par **l'analyse exploratoire des données** (EDA).

Les data scientists de Target avaient identifié un pattern : les femmes enceintes achètent des produits spécifiques (vitamines, coton, savon non parfumé) à des moments précis de leur grossesse. En analysant les achats, ils pouvaient prédire une grossesse avec une précision troublante. Le score de "grossesse" : En combinant environ 25 produits clés, Target pouvait non seulement deviner qu'une cliente était enceinte, mais aussi estimer sa date d'accouchement à quelques semaines près.

L'histoire a été révélée pour la première fois par le journaliste Charles Duhigg dans le New York Times Magazine en 2012 (puis dans son livre Le Pouvoir des habitudes).

C'est la puissance de l'EDA analytique : **découvrir des patterns cachés** dans les données.

> 💭 **Question Socratique #1** : Cette histoire soulève des questions éthiques. À partir de quand l'analyse de données devient-elle intrusive ? Où tracer la ligne entre "utile" et "dérangeant" ?

> 💭 **Réponse** :

🛡️ L'évolution légale : Le RGPD
Depuis cette affaire, des lois comme le RGPD en Europe ont été créées pour encadrer cela. Aujourd'hui, une entreprise ne peut pas (en théorie) traiter des données concernant la santé ou la vie intime sans un consentement explicite et une finalité claire.

L'astuce de Target consistant à "camoufler" ses publicités (mélanger les couches et les tondeuses) montre que l'entreprise avait conscience d'avoir franchi une limite psychologique, à défaut d'une limite légale à l'époque.

Suite à cette affaire, Target a réalisé que les clients trouvaient cela "effrayant" (creepy). Pour éviter de donner l'impression qu'ils espionnaient leurs clients, ils ont changé de stratégie :

Au lieu d'envoyer un catalogue 100% "bébé", ils ont commencé à mélanger les publicités. Ils mettaient un coupon pour des couches à côté d'une publicité pour une tondeuse à gazon ou du vin. Ainsi, la cliente pensait que les coupons étaient aléatoires, même si le coupon pour les couches était précisément ciblé pour elle.

---

## 7.1 Différence EDA diagnostique vs analytique

### Rappel : vous êtes passé de l'autre côté

| Aspect | EDA Diagnostique (Chapitre 4) | EDA Analytique (Ce chapitre) |
|--------|------------------------------|------------------------------|
| **Données** | Brutes, avec problèmes | Nettoyées, structurées |
| **Question** | "Qu'est-ce qui ne va pas ?" | "Que disent les données ?" |
| **Objectif** | Identifier les erreurs | Comprendre les phénomènes |
| **Mindset** | Auditeur / Détective | Scientifique / Explorateur |
| **Output** | Checklist de problèmes | Hypothèses, insights |

```
Données brutes → [EDA DIAGNOSTIQUE] → [NETTOYAGE] → [EDA ANALYTIQUE] → Insights
                                                     (Vous êtes ici)
```

---

### Le mindset de l'exploration

En EDA analytique, vous cherchez à répondre à des questions comme :

- Quelle est la distribution de mes clients par âge ?
- Y a-t-il une relation entre le montant d'achat et l'ancienneté ?
- Quels segments de clients sont les plus rentables ?
- Existe-t-il des patterns saisonniers dans les ventes ?

**L'EDA est un dialogue avec vos données.** Vous posez des questions, les données répondent, et leurs réponses génèrent de nouvelles questions.
