# Bascule de ophtalmo-retine-cannes.fr vers le nouveau site

Préparation commencée le 19 novembre 2026, mise à jour le 21 novembre 2026 (passage à Cloudflare Pages le soir). Le domaine pointe toujours vers l'ancien WordPress : rien n'est actif tant que les DNS ne sont pas modifiés.

## Décisions prises

- **Hébergement : Cloudflare Pages** (décidé le 21/11/2026 au soir, à la place de GitHub Pages retenu le matin). Le code reste dans le dépôt GitHub `sosretine/ophtalmo-retine-cannes` ; Cloudflare Pages le publie automatiquement à chaque modification. Raison du choix : les vraies redirections 301 du fichier `_redirects` (anciennes adresses du WordPress), meilleures pour le référencement que les pages de redirection. Offre gratuite (500 publications par mois, bande passante illimitée, HTTPS inclus).
- **Les pages de redirection et `404.html` sont conservées** : elles servent de filet de sécurité pour toute adresse absente de `_redirects`.
- **DNS : restent chez OVH** (pas de transfert vers Cloudflare, pour ne pas toucher aux mails). Conséquence : Cloudflare Pages ne peut servir que `www.ophtalmo-retine-cannes.fr` (un domaine nu exige des DNS chez Cloudflare). Le domaine nu (sans www) reste pointé sur l'hébergement OVH existant, où un simple fichier `.htaccess` le redirige en 301 vers `https://www.ophtalmo-retine-cannes.fr`.
- **Mesure d'audience : Cloudflare Web Analytics**, sans cookie (pas de bandeau de consentement). À activer dans le projet Cloudflare Pages.
- **Date de bascule visée** : vacances de Noël 2026, un jour où le Dr Portmann reste joignable dans les 48 h qui suivent.

## Ce qui est prêt

- **Adresse de référence : `https://www.ophtalmo-retine-cannes.fr`** (avec www, comme l'ancien site). Toutes les balises canonical, og:url, données structurées, sitemap.xml, robots.txt et llms.txt l'utilisent.
- **Pages de redirection** (dossiers `pathologie/`, `jean-paul-amar/`, etc.) : redirection immédiate et canonical vers la nouvelle page. C'est la solution retenue sur GitHub Pages.
- **`404.html`** : redirige les familles d'anciennes adresses (activités, conférences, slider…) et propose l'accueil, les pathologies et le contact.
- **`_redirects`** : 301 pour 43 adresses exactes et 6 familles. Utilisé par Cloudflare Pages.
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
6. Créer le compte Cloudflare (gratuit), puis Workers & Pages > Créer > Pages > connecter le dépôt GitHub `sosretine/ophtalmo-retine-cannes` (branche `main`, aucune commande de build, dossier de sortie `/`). Le site est alors visible sur une adresse de test en `.pages.dev` : vérifier les pages et une dizaine d'anciennes adresses (les 301 doivent fonctionner).
7. Dans le projet Cloudflare Pages : activer Web Analytics.
8. Préparer sur l'hébergement OVH un `.htaccess` qui redirige tout en 301 vers `https://www.ophtalmo-retine-cannes.fr` (à mettre en place le jour J à la place du WordPress, sauvegarde faite).
9. Une fois le site validé sur Cloudflare Pages : désactiver GitHub Pages sur le dépôt (Settings > Pages), pour éviter un doublon de contenu en `sosretine.github.io`.
10. 48 h avant : baisser le TTL de la ligne www à 300 secondes dans la zone OVH.

## Le jour J

11. Dans Cloudflare Pages, projet du site : Domaines personnalisés > ajouter `www.ophtalmo-retine-cannes.fr`. Cloudflare indique la cible du CNAME (l'adresse `.pages.dev` du projet).
12. Dans la zone DNS OVH, **ne toucher qu'à la ligne www** : supprimer les lignes A, AAAA, MX et TXT de www, puis créer une ligne CNAME `www` vers l'adresse `.pages.dev` donnée par Cloudflare (un CNAME ne peut pas coexister avec d'autres lignes sur www). Avant de supprimer les MX de www : vérifier que personne ne reçoit de mails sur une adresse en @www.ophtalmo-retine-cannes.fr (très peu probable).
13. **Ne pas toucher** au domaine nu (lignes A `213.186.33.16` et AAAA, qui restent sur l'hébergement OVH), ni aux lignes MX, SPF, mail, smtp, imap, pop3, autoconfig, _autodiscover, _imaps, _submission (mails en @ophtalmo-retine-cannes.fr hébergés chez OVH), ni à la ligne TXT google-site-verification.
14. Sur l'hébergement OVH (dossier `www/amar`) : remplacer le WordPress par le `.htaccess` de redirection préparé à l'étape 8 (la sauvegarde complète doit être faite et vérifiée avant). Vérifier que le certificat HTTPS du domaine nu reste actif chez OVH.
15. Attendre que Cloudflare affiche le domaine www comme actif, certificat émis (quelques minutes à quelques heures).
16. Tester : l'accueil avec et sans www, le HTTPS, 10 anciennes adresses du tableau (dont /pathologie/decollement-de-retine/ et /jean-paul-amar/, qui doivent répondre en 301), et l'envoi / la réception d'un mail en @ophtalmo-retine-cannes.fr.
17. Search Console et Bing : soumettre `https://www.ophtalmo-retine-cannes.fr/sitemap.xml` en gardant l'ancien sitemap quelques semaines ; dans Search Console, demander l'indexation de l'accueil, décollement de rétine, vitrectomie, trou maculaire, membrane épirétinienne, équipe.

En cas de problème : remettre les lignes de la copie de la zone DNS du 21/11/2026 et le contenu sauvegardé de `www/amar`, l'ancien site revient tel quel.

## Les 2 mois suivants

18. Mettre à jour les liens externes : fiche Google Business Profile, Doctolib, scrv.eu, site de la clinique Oxford, annuaires.
19. Chaque semaine : Search Console > Pages (erreurs 404, pages exclues) et Performances (clics, positions). Une baisse temporaire de 2 à 6 semaines est normale.
20. Après quelques semaines sans problème : supprimer définitivement les fichiers et la base de l’ancien WordPress de l’hébergement OVH (faille de sécurité PHP/WordPress), une fois la sauvegarde vérifiée, et changer le mot de passe FTP. **Garder l’hébergement OVH** (payé jusqu’en oct. 2029) : il porte la redirection du domaine nu vers www.
21. Garder les pages de redirection au moins un an (idéalement toujours).

## Contenu sans équivalent direct

- **Vidéos** (11 pages /video/) : redirigées vers les pages pathologies ou vitrectomie correspondantes. Une page « Vidéos chirurgicales » dédiée pourrait être ajoutée plus tard.
- **Activités et conférences** (196 pages) : regroupées vers l'équipe et les publications.
- **Rééducation basse vision** : pas de page équivalente sur le nouveau site. À décider : créer une courte page ou laisser la redirection actuelle.
