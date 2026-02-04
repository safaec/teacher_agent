
### Table de Référence : Sélecteurs CSS pour BeautifulSoup

| Syntaxe HTML | Syntaxe Python (Sélecteur) | Signification | Exemple d'usage |
| --- | --- | --- | --- |
| `<balise>` | `select("balise")` | Cible toutes les balises de ce nom. | `select("tr")` (Toutes les lignes) |
| `class="nom"` | `select(".nom")` | Cible tous les éléments ayant cette **classe**. | `select(".prix")` |
| `id="nom"` | `select("#nom")` | Cible l'élément **unique** ayant cet **ID**. | `select("#footer")` |
| `<balise class="nom">` | `select("balise.nom")` | **Filtre :** La balise précise avec la classe précise. | `select("table.wikitable")` |
| `[attribut]` | `select("[attribut]")` | Cible les éléments qui possèdent cet **attribut**. | `select("[href]")` (Tous les liens) |
| `[attr="valeur"]` | `select("[attr='valeur']")` | Cible l'élément avec la **valeur exacte** de l'attribut. | `select("[width='20']")` |
| `[attr*="valeur"]` | `select("[attr*='valeur']")` | **Contient :** L'attribut contient ce morceau de texte. | `select("[title*='Drapeau']")` |
| `class="c1 c2"` | `select(".c1.c2")` | **ET :** L'élément possède les deux classes. | `select(".btn.active")` |
| `<baliseP> <baliseE>` (espace) | `select("parent enfant")` | **Descendant :** Cherche l'enfant partout dans le parent. | `select("table a")` (Liens dans table) |
| `<P> > <E>` | `select("parent > enfant")` | **Enfant direct :** Ne cherche pas plus loin que le 1er niveau. | `select("ul > li")` |
| `balise:nth-of-type(n)` | `select("balise:nth-of-type(2)")` | **Position :** Cible le n-ième élément de ce type. | `select("td:nth-of-type(2)")` (2e col) |
| `balise.c[attr='v']` | `select("balise.c[attr='v']")` | **Le Sniper :** Combine balise, classe et attribut. | `select("a.link[href='/home']")` |

---

### Rappel de l'extraction (Une fois l'élément trouvé)

Une fois que le sélecteur a trouvé l'élément, il faut dire à Python **quelle partie** de la balise on veut récupérer :

| Ce que je veux extraire | Code Python | Résultat attendu |
| --- | --- | --- |
| **Le texte** (entre les balises) | `element.text.strip()` | `"Nestlé"` |
| **Un attribut** (ex: le lien) | `element["href"]` | `"/wiki/Nestle"` |
| **Une source d'image** | `element["src"]` | `"image.png"` |

---

### Les 3 "Règles d'Or"

1. **L'espace est un voyage :** Si tu mets un espace, tu "entres" à l'intérieur de l'élément précédent. Si tu colles les symboles (`balise.classe`), tu restes sur le même élément.
2. **La liste vs L'objet :** * `select()` renvoie toujours une **liste** (donc boucle `for` obligatoire).

* `select_one()` renvoie le **premier objet** trouvé (donc `.text` possible direct).

1. **Le nettoyage est obligatoire :** Le web est sale. Utilise toujours `.strip()` sur le texte pour éviter les espaces invisibles (`\n`, `\t`) qui cassent les analyses de données.
