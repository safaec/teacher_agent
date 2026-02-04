# Partie 1 – Fondamentaux : Le protocole HTTP

*Comprendre comment fonctionne la communication web*

## 1.1 Qu'est-ce que HTTP ?

**HTTP** signifie **HyperText Transfer Protocol**. C'est le protocole qui permet au navigateur (ou tout autre client) de communiquer avec un serveur web.
Tout échange HTTP repose sur deux éléments fondamentaux :

- Une **requête** envoyée par le client
- Une **réponse** renvoyée par le serveur

## 1.2 Le modèle Client-Serveur

Le web fonctionne selon une architecture client-serveur :

- **Client** : le navigateur web, une application mobile, ou un script Python qui envoie des requêtes.
- **Serveur** : la machine qui héberge les ressources (pages web, API, fichiers) et qui traite les requêtes.
*Quand tu écris une URL, le client construit une requête HTTP, l'envoie au serveur, le serveur traite la demande et renvoie une réponse HTTP.*

demo : <https://reqbin.com/post-online>

## 1.3 Structure d'une requête HTTP

Une requête HTTP suit toujours la même structure :

| &lt;METHOD&gt; &lt;RESOURCE&gt; &lt;VERSION&gt;&lt;HEADER-NAME-1&gt;: &lt;HEADER-VALUE-1&gt;&lt;HEADER-NAME-2&gt;: &lt;HEADER-VALUE-2&gt;...&lt;BODY&gt; |
| --- |
**Exemple concret** : requête vers <https://jsonplaceholder.typicode.com/posts>
| GET /posts HTTP/1.1Host: jsonplaceholder.typicode.comAccept: application/jsonUser-Agent: Mozilla/5.0 |
| --- |

### Explication des éléments

**1\. Méthode (METHOD)**
Indique l'action à effectuer sur la ressource : GET (lire), POST (créer), PUT (remplacer), PATCH (modifier), DELETE (supprimer).
**2\. Ressource (RESOURCE)**
Chemin vers ce qu'on veut sur le serveur (URL relative). Exemple : /posts
**3\. Version du protocole (VERSION)**
HTTP/1.1, HTTP/2, ou HTTP/3. Généralement choisi automatiquement par l'outil.
**4\. En-têtes (HEADERS)**
Informations supplémentaires sous forme clé:valeur (Content-Type, Accept, Authorization, User-Agent...).
**5\. Corps (BODY)**
Utilisé avec POST, PUT, PATCH. Contient les données envoyées au serveur (JSON, formulaire...).

## 1.4 Structure d'une réponse HTTP

La réponse du serveur suit également une structure standardisée :

| &lt;VERSION&gt; &lt;STATUS-CODE&gt; &lt;STATUS-MESSAGE&gt;&lt;HEADER-NAME-1&gt;: &lt;HEADER-VALUE-1&gt;...&lt;BODY&gt; |
| --- |
**Exemple de réponse** :
| HTTP/1.1 200 OKContent-Type: application/json; charset=utf-8Date: Fri, 05 Sep 2025 15:00:29 GMT{"userId": 1, "id": 1, "title": "Mon post"} |
| --- |

## 1.5 HTTPS et sécurité des communications

**HTTPS** (HTTP Secure) est la version sécurisée de HTTP. Elle utilise le chiffrement TLS/SSL pour protéger les données en transit.
**Pourquoi HTTPS est obligatoire :**

- **Chiffrement** : les données sont illisibles pour un attaquant qui intercepterait le trafic.
- **Intégrité** : garantit que les données n'ont pas été modifiées en transit.
- **Authentification** : certifie l'identité du serveur (certificat SSL).
- **Protection contre les attaques man-in-the-middle**.
**⚠️ Important :** L'authentification Basic Auth n'est sécurisée QUE si la connexion utilise HTTPS. En HTTP simple, le mot de passe peut être intercepté !

## 1.6 Les versions du protocole HTTP

Le protocole HTTP a évolué au fil du temps pour améliorer les performances :

| Version | Année | Transport | Caractéristiques | Limites |
| --- | --- | --- | --- | --- |
| HTTP/1.0 | 1996 | TCP | Première version largement adoptée, headers introduits | Une requête = une connexion TCP |
| HTTP/1.1 | 1997 | TCP | Connexions persistantes (Keep-Alive), cache, compression gzip | Bloquage de tête de ligne, pas de multiplexage |
| HTTP/2 | 2015 | TCP | Multiplexage, compression headers (HPACK), format binaire | Toujours basé sur TCP, sensible à la perte de paquets |
| HTTP/3 | 2019 | QUIC (UDP) | Connexions rapides (0-RTT), résilient à la perte de paquets | Déploiement encore en cours |

## 1.7 Exemple concret : charger une page web

Quand tu tapes **<https://www.youtube.com/@LiveCampus\>_** dans ton navigateur :
**1\. Première requête (HTML)**
Le navigateur envoie une requête HTTP vers le serveur de YouTube. Le serveur répond avec un fichier HTML (le "squelette" de la page).
**2\. Analyse du HTML**
Le navigateur lit ce HTML et y trouve des références vers d'autres ressources : images, vidéos, fichiers CSS, fichiers JavaScript.
**3\. Nouvelles requêtes HTTP**
Pour chaque ressource listée dans le HTML, le navigateur fait une requête HTTP distincte. Si la page contient 50 images, 10 fichiers CSS et JS, cela fait potentiellement 60+ requêtes au total !
**4\. Affichage progressif**
Le navigateur affiche la page au fur et à mesure que les ressources arrivent. On peut observer toutes ces requêtes dans l'onglet "Réseau" des outils développeur.
**💡 Résumé :** Charger une page web, ce n'est pas une seule requête, mais des dizaines. Le HTML est la porte d'entrée, puis le navigateur va chercher toutes les "briques" une par une.

## 🎯 Exercice : Analyser les requêtes d'un site web

**Objectif** : comprendre combien de requêtes sont nécessaires pour charger une page web.
**Sites à tester :**

- <https://example.com> (ultra minimaliste)
- <https://wikipedia.org> (modéré)
- <https://openai.com> (plus complexe)
**Questions à analyser :**

1. Combien de requêtes au total ont été faites ?
2. Quelle est la première requête envoyée ?
3. Classez les requêtes par type (HTML, CSS, JS, Images, Autres)
4. Quelle est la taille totale téléchargée ?
5. Quelle requête est la plus lourde ?

# Partie 2 – Les méthodes HTTP et codes de statut

*Le vocabulaire essentiel des APIs*

## 2.1 Les méthodes HTTP détaillées

Les méthodes HTTP définissent l'action à effectuer sur une ressource. Voici les principales :

### GET – Récupérer des données

- **Utilisation** : Lire une ressource
- **Idempotent** : OUI (même résultat à chaque appel)
- **Sécurisé** : OUI (pas d'effet de bord)
| GET /users # Tous les utilisateursGET /users/123 # Utilisateur avec ID 123GET /users/123/posts # Posts de l'utilisateur 123GET /search?q=python # Recherche avec paramètres |
| --- |

### POST – Créer des ressources

- **Utilisation** : Créer une nouvelle ressource
- **Idempotent** : NON (peut créer plusieurs ressources)
- **Sécurisé** : NON (modifie l'état)
| POST /usersContent-Type: application/json{"name": "John Doe", "email": "<john@example.com>"} |
| --- |

### PUT – Mise à jour complète

- **Utilisation** : Remplacer complètement une ressource
- **Idempotent** : OUI
On envoie TOUTES les propriétés de la ressource, même celles qui ne changent pas.

### PATCH – Mise à jour partielle

- **Utilisation** : Modifier partiellement une ressource
- **Idempotent** : NON
On envoie uniquement les champs à modifier.
| PATCH /users/123{"email": "<john.new@example.com>"} |
| --- |

### DELETE – Supprimer des ressources

- **Utilisation** : Supprimer une ressource
- **Idempotent** : OUI (supprimer deux fois = même résultat)
| DELETE /users/123 |
| --- |

## 2.2 Notion d'idempotence et de sécurité

**Idempotent** : une opération est idempotente si l'exécuter plusieurs fois produit le même résultat qu'une seule exécution.
**Sécurisé (Safe)** : une opération est sécurisée si elle ne modifie pas l'état du serveur.

| Méthode | Action | Idempotent | Sécurisé |
| --- | --- | --- | --- |
| GET | Lire | ✅ OUI | ✅ OUI |
| POST | Créer | ❌ NON | ❌ NON |
| PUT | Remplacer | ✅ OUI | ❌ NON |
| PATCH | Modifier | ❌ NON | ❌ NON |
| DELETE | Supprimer | ✅ OUI | ❌ NON |

## 2.3 Les codes de statut HTTP

Chaque réponse HTTP contient un code de statut qui indique le résultat de la requête.

### Codes 2xx – Succès

| Code | Message | Utilisation |
| --- | --- | --- |
| 200 | OK | Requête réussie (GET, PUT, PATCH) |
| 201 | Created | Ressource créée avec succès (POST) |
| 204 | No Content | Succès sans contenu à retourner (DELETE) |

### Codes 3xx – Redirection

| Code | Message | Utilisation |
| --- | --- | --- |
| 301 | Moved Permanently | Ressource déplacée définitivement |
| 304 | Not Modified | Ressource non modifiée (cache) |

### Codes 4xx – Erreur client

| Code | Message | Utilisation |
| --- | --- | --- |
| 400 | Bad Request | Requête malformée |
| 401 | Unauthorized | Authentification requise |
| 403 | Forbidden | Accès refusé (droits insuffisants) |
| 404 | Not Found | Ressource non trouvée |
| 409 | Conflict | Conflit (ex: email déjà utilisé) |
| 429 | Too Many Requests | Trop de requêtes (rate limiting) |

### Codes 5xx – Erreur serveur

| Code | Message | Utilisation |
| --- | --- | --- |
| 500 | Internal Server Error | Erreur interne du serveur |
| 502 | Bad Gateway | Serveur intermédiaire indisponible |
| 503 | Service Unavailable | Service temporairement indisponible |

## 2.4 Les headers HTTP essentiels

Les headers (en-têtes) fournissent des métadonnées sur la requête ou la réponse.

| Header | Description |
| --- | --- |
| Content-Type | Type du contenu envoyé (application/json, text/html, multipart/form-data...) |
| Accept | Formats que le client accepte en réponse |
| Authorization | Informations d'authentification (Basic, Bearer token...) |
| User-Agent | Identité du client (navigateur, application...) |
| Cache-Control | Directives de mise en cache |
| X-RateLimit-* | Informations sur les limites de requêtes (rate limiting) |

## 2.5 Structure d'erreur cohérente (JSON)

Une bonne API renvoie des erreurs structurées et informatives. Voici un format recommandé :

| {"error": {"code": "USER_NOT_FOUND","message": "User with ID 123 not found","details": "The requested user does not exist"}} |
| --- |
**Éléments clés d'une bonne erreur :**

- **code** : identifiant technique de l'erreur (pour le débogage)
- **message** : description lisible par un humain
- **details** : informations supplémentaires (optionnel)
