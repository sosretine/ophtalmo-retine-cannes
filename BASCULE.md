# Bascule de ophtalmo-retine-cannes.fr vers le nouveau site

Préparation commencée le 19 novembre 2026, mise à jour le 21 novembre 2026. Le domaine pointe toujours vers l'ancien WordPress : rien n'est actif tant que les DNS ne sont pas modifiés.

## Décisions prises

- **Hébergement : GitHub Pages** (décidé le 21/11/2026). Pas de vraies 301 : ce sont les pages de redirection et la page 404 qui prennent le relais. Le fichier `_redirects` est conservé au cas où l'on passerait un jour sur Cloudflare Pages ou Netlify (il est sans effet sur GitHub Pages).
- **Date de bascule visée** : vacances de Noël 2026, un jour où le Dr Portmann reste joignable dans les 48 h qui suivent.

## Ce qui est prêt

- **Adresse de référence : `https://www.ophtalmo-retine-cannes.fr`** (avec www, comme l'ancien site). Toutes les balises canonical, og:url, données structurées, sitemap.xml, robots.txt et llms.txt l'utilisent.
- **Pages de redirection** (dossiers `pathologie/`, `jean-paul-amar/`, etc.) : redirection immédiate et canonical vers la nouvelle page. C'est la solution retenue sur GitHub Pages.
- **`404.html`** : redirige les familles d'anciennes adresses (activités, conférences, slider…) et propose l'accueil, les pathologies et le contact.
- **`_redirects`** : 301 pour 43 adresses exactes et 6 familles. Non utilisé sur GitHub Pages.
- **Table de correspondance** : `correspondance_ancien_nouveau_site.csv` (fournie à part).

## Accès et comptes (état au 21/11/2026)

- **Nom de domaine** : chez OVH, compte du cabinet aj168197-ovh (SOS RETINE SUD). Titulaire transféré d'Inglobo vers SOS RETINE SUD, validé par Patrick Porta le 20/11/2026. Renouvelé jusqu'au 23/10/2027. Contact technique : David Benguigui (Smartip 06).
- **DNS** : gérés dans l'espace OVH (et non chez Nettic). Copie de la zone faite le 21/11/2026 : `zone-dns-ophtalmo-retine-cannes-2026-11-21.txt`.
- **Ancien WordPress** : hébergement OVH « sosretinecannes.com » (hosting-perso, payé jusqu'en oct. 2029), dossier `www/amar`. La bascule ne le supprime pas.
- **Google Search Console** : propriété « domaine » ophtalmo-retine-cannes.fr créée le 21/11/2026 (validation par TXT dans la zone OVH, à ne jamais supprimer). Propriétaires : alex7portmann@gmail.com et ran.portmann21@gmail.com. Sitemap de l'ancien site soumis.
- **Bing Webmaster Tools** : ophtalmo-retine-cannes.fr et scrv.eu importés depuis la Search Console le 21/11/2026. Sitemap de l'ancien site soumis.
- **Fiche Google Business** : gestionnaire inconnu (Inglobo n'y a jamais eu accès). À retrouver au cabinet.

## Avant le jour J

1. ~~Récupérer le nom de domaine auprès d'Inglobo.~~ Fait le 20/11/2026.
2. ~~Créer la Search Console.~~ Fait le 21/11/2026. Laisser quelques semaines de données sur l'ancien site, puis exporter pages et requêtes avant la bascule.
3. ~~Sauvegarder l'ancien WordPress.~~ Fichiers copiés par SFTP le 21/11/2026 (27 962 fichiers). Sauvegarde de la base demandée à OVH le même jour : récupérer le fichier, l'archiver avec les fichiers, et garder le tout au moins 3 mois, hors dossier partagé (il contient `wp-config.php`).
4. Vérifier qu'aucune page importante de l'ancien site n'a été oubliée (Search Console > Pages).
5. Retrouver qui gère la fiche Google Business et s'y faire ajouter comme propriétaire.
6. Dans GitHub, compte sosretine : Settings > Pages > « Add a domain » pour vérifier ophtalmo-retine-cannes.fr. GitHub donne une ligne TXT à ajouter dans la zone OVH. Cela empêche quelqu'un d'autre d'utiliser le domaine sur GitHub.
7. 48 h avant : baisser le TTL des lignes A, AAAA et www à 300 secondes dans la zone OVH.

## Le jour J

8. Dans le dépôt : ajouter un fichier `CNAME` contenant `www.ophtalmo-retine-cannes.fr`. Puis Settings > Pages : déclarer `www.ophtalmo-retine-cannes.fr` comme domaine personnalisé.
9. Dans la zone DNS OVH, **ne toucher qu'aux lignes du site** :
- domaine nu (sans www) : remplacer la ligne A `213.186.33.16` par les 4 lignes A de GitHub : `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153` ;
- domaine nu : remplacer la ligne AAAA `2001:41d0:1:1b00:213:186:33:16` par `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153` ;
- www : supprimer les lignes A, AAAA, MX et TXT de www, puis créer une ligne CNAME `www` vers `sosretine.github.io.` (un CNAME ne peut pas coexister avec d'autres lignes sur www) ;
- **ne pas toucher** aux lignes MX, SPF, mail, smtp, imap, pop3, autoconfig, _autodiscover, _imaps, _submission (mails en @ophtalmo-retine-cannes.fr hébergés chez OVH), ni à la ligne TXT google-site-verification.
10. Avant de supprimer les lignes MX de www : vérifier que personne ne reçoit de mails sur une adresse en @www.ophtalmo-retine-cannes.fr (très peu probable).
11. Une fois le certificat émis par GitHub (quelques minutes à quelques heures) : activer « Enforce HTTPS ».
12. Tester : l'accueil avec et sans www, le HTTPS, 10 anciennes adresses du tableau (dont /pathologie/decollement-de-retine/ et /jean-paul-amar/), et l'envoi / la réception d'un mail en @ophtalmo-retine-cannes.fr.
13. Search Console et Bing : soumettre `https://www.ophtalmo-retine-cannes.fr/sitemap.xml` en gardant l'ancien sitemap quelques semaines ; dans Search Console, demander l'indexation de l'accueil, décollement de rétine, vitrectomie, trou maculaire, membrane épirétinienne, équipe.

En cas de problème : remettre les lignes de la copie de la zone DNS du 21/11/2026, l'ancien site revient tel quel.

## Les 2 mois suivants

14. Mettre à jour les liens externes : fiche Google Business Profile, Doctolib, scrv.eu, site de la clinique Oxford, annuaires.
15. Chaque semaine : Search Console > Pages (erreurs 404, pages exclues) et Performances (clics, positions). Une baisse temporaire de 2 à 6 semaines est normale.
16. Après quelques semaines sans problème : fermer l'ancien WordPress (faille de sécurité PHP/WordPress), une fois la sauvegarde vérifiée. Changer le mot de passe FTP de l'hébergement.
17. Garder les pages de redirection au moins un an (idéalement toujours).

## Contenu sans équivalent direct

- **Vidéos** (11 pages /video/) : redirigées vers les pages pathologies ou vitrectomie correspondantes. Une page « Vidéos chirurgicales » dédiée pourrait être ajoutée plus tard.
- **Activités et conférences** (196 pages) : regroupées vers l'équipe et les publications.
- **Rééducation basse vision** : pas de page équivalente sur le nouveau site. À décider : créer une courte page ou laisser la redirection actuelle.
