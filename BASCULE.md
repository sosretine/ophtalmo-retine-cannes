# Bascule de ophtalmo-retine-cannes.fr vers le nouveau site

Préparation faite le 19 novembre 2026. Le domaine pointe toujours vers l'ancien WordPress : rien n'est actif tant que les DNS ne sont pas modifiés.

## Ce qui est prêt

- **Adresse de référence : `https://www.ophtalmo-retine-cannes.fr`** (avec www, comme l'ancien site). Toutes les balises canonical, og:url, données structurées, sitemap.xml, robots.txt et llms.txt l'utilisent.
- **`_redirects`** : redirections 301 de toutes les anciennes adresses (43 adresses exactes + 6 familles : /activite/*, /conference/*, /slider/*, /category/*, /feed/*, /author/*). Lu par Cloudflare Pages ou Netlify.
- **Pages de redirection** (dossiers `pathologie/`, `jean-paul-amar/`, etc.) : solution de repli si le site reste sur GitHub Pages, qui ne sait pas faire de 301. Redirection immédiate + canonical vers la nouvelle page.
- **`404.html`** : redirige les familles d'anciennes adresses (activités, conférences…) et propose l'accueil, les pathologies et le contact.
- **Table de correspondance** : `correspondance_ancien_nouveau_site.csv` (fournie à part).

## Choix de l'hébergement (à décider)

| | GitHub Pages (actuel) | Cloudflare Pages ou Netlify |
|---|---|---|
| Vraies redirections 301 | Non (pages de redirection + 404) | Oui (`_redirects`) |
| Coût | Gratuit | Gratuit |
| Recommandé pour la bascule | Acceptable | **Oui** |

Si Cloudflare Pages ou Netlify : **supprimer les dossiers de pages de redirection** avant la mise en ligne, pour que le fichier `_redirects` s'applique sans conflit (Netlify sert un fichier existant avant une redirection non forcée).

## Avant le jour J

1. Récupérer l'accès à Google Search Console (propriété « domaine » ophtalmo-retine-cannes.fr) et aux statistiques auprès d'Inglobo. Exporter les pages et requêtes des 12 derniers mois.
2. Faire une copie complète de l'ancien site WordPress (sauvegarde Inglobo) et la garder au moins 3 mois.
3. Vérifier qu'aucune page importante de l'ancien site n'a été oubliée (Search Console > Pages).
4. Choisir une date calme (pas juste avant une communication ou le congrès SCRV).

## Le jour J

5. GitHub Pages : ajouter un fichier `CNAME` contenant `www.ophtalmo-retine-cannes.fr`, puis déclarer le domaine dans Settings > Pages et activer « Enforce HTTPS ».
   Cloudflare/Netlify : connecter le dépôt, déclarer le domaine `www.ophtalmo-retine-cannes.fr` et la redirection du domaine nu vers www.
6. Demander à Nettic : `www` en CNAME vers l'hébergeur choisi ; le domaine nu (sans www) redirigé vers www.
7. Tester : l'accueil en www et sans www, 10 anciennes adresses du tableau (dont /pathologie/decollement-de-retine/ et /jean-paul-amar/), le HTTPS.
8. Search Console : soumettre `https://www.ophtalmo-retine-cannes.fr/sitemap.xml` ; demander l'indexation de l'accueil, décollement de rétine, vitrectomie, trou maculaire, membrane épirétinienne, équipe.

## Les 2 mois suivants

9. Mettre à jour les liens externes : fiche Google Business Profile, Doctolib, scrv.eu, site de la clinique Oxford, annuaires.
10. Chaque semaine : Search Console > Pages (erreurs 404, pages exclues) et Performances (clics, positions). Une baisse temporaire de 2 à 6 semaines est normale.
11. Garder les redirections au moins un an (idéalement toujours).

## Contenu sans équivalent direct

- **Vidéos** (11 pages /video/) : redirigées vers les pages pathologies ou vitrectomie correspondantes. Une page « Vidéos chirurgicales » dédiée pourrait être ajoutée plus tard.
- **Activités et conférences** (196 pages) : regroupées vers l'équipe et les publications.
