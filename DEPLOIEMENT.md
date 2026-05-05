# Déploiement de lesclefsdelavenir.ch

Site statique monofichier (`index.html`) prêt à être mis en ligne. Aucun serveur ni base de données n'est nécessaire.

## Contenu du dépôt

- `index.html` — site complet (HTML + CSS + JS + SVG inline)
- `DEPLOIEMENT.md` — ce fichier

Tout est embarqué : pas de build step, pas de dépendances locales (GSAP est chargé depuis le CDN cdnjs).

---

## Option A — OVH Hébergement Web (la plus simple si vous avez déjà un hébergement)

Si votre nom de domaine `lesclefsdelavenir.ch` est chez OVH (ou Infomaniak, Hostpoint, etc.) avec un hébergement mutualisé :

1. Connectez-vous au FTP/SFTP de votre hébergeur (FileZilla, Cyberduck…)
2. Allez dans le dossier racine public (souvent `www/`, `public_html/` ou `htdocs/`)
3. Uploadez `index.html` à la racine
4. Le site est en ligne sur `https://lesclefsdelavenir.ch` après quelques minutes

**Coût** : compris dans votre forfait OVH/Infomaniak existant (CHF 5–10/mois).

---

## Option B — Cloudflare Pages (gratuit, recommandé)

Hébergement gratuit, rapide, certificat SSL automatique.

1. Créez un compte gratuit sur https://pages.cloudflare.com
2. **Create a project** → **Connect to Git** → choisissez `utilisateur456/lesclefsdelavenir`
3. Branch de production : `claude/fortune-telling-site-design-D6YjY` (ou `main` si vous le renommez)
4. Build command : *(laisser vide)*
5. Build output directory : `/`
6. Cliquez **Save and Deploy**

Le site est déployé sur `https://lesclefsdelavenir.pages.dev`.

### Brancher le domaine `lesclefsdelavenir.ch`

7. Onglet **Custom domains** → **Set up a custom domain** → entrez `lesclefsdelavenir.ch` puis `www.lesclefsdelavenir.ch`
8. Cloudflare vous donne 2 enregistrements DNS à créer chez votre registrar (ex: GoDaddy, Hostpoint, Switch) :
   - `CNAME @ → <votre-projet>.pages.dev`
   - `CNAME www → <votre-projet>.pages.dev`
9. Connectez-vous chez votre registrar du domaine `.ch`, modifiez les DNS, ajoutez ces deux entrées
10. Patientez 1 à 24h pour la propagation DNS — le SSL s'active automatiquement

**Coût** : gratuit. Trafic illimité.

---

## Option C — Netlify (alternative à Cloudflare, aussi gratuit)

1. Compte sur https://netlify.com → **Add new site** → **Import from Git** → GitHub
2. Sélectionnez le repo, branche `claude/fortune-telling-site-design-D6YjY`
3. Build command : *(vide)*  /  Publish directory : `/`
4. **Deploy site**
5. Onglet **Domain settings** → **Add custom domain** → `lesclefsdelavenir.ch`
6. Ajoutez les enregistrements DNS donnés par Netlify chez votre registrar

---

## Option D — GitHub Pages (déjà actif sur ce repo)

La branche `gh-pages` de ce dépôt est déjà servie sur :
`https://utilisateur456.github.io/lesclefsdelavenir/`

Pour utiliser le domaine `lesclefsdelavenir.ch` :

1. Créez un fichier `CNAME` à la racine de la branche `gh-pages` contenant simplement :
   ```
   lesclefsdelavenir.ch
   ```
2. Chez votre registrar du `.ch`, créez les enregistrements :
   - `A @ → 185.199.108.153`
   - `A @ → 185.199.109.153`
   - `A @ → 185.199.110.153`
   - `A @ → 185.199.111.153`
   - `CNAME www → utilisateur456.github.io`
3. Dans GitHub → repo Settings → Pages → vérifiez que le domaine est bien pris en compte → cochez **Enforce HTTPS**

---

## Vérifications avant mise en ligne

- [ ] Tester le site sur mobile (responsive 600px)
- [ ] Vérifier les 3 liens de paiement (Twint, PayPal, cartes)
- [ ] Tester le numéro `tel:0901212212` sur mobile
- [ ] Tester le lien WhatsApp `wa.me/41782535452`
- [ ] Configurer un service de réception de formulaire (le `<form>` actuel fait `onsubmit="return false"` — il faut le brancher à Formspree, FormKeep, ou un endpoint personnalisé pour recevoir les demandes)

## Brancher le formulaire de contact

Solution la plus rapide : **Formspree** (gratuit jusqu'à 50 messages/mois)

1. Créez un compte sur https://formspree.io
2. Créez un nouveau formulaire, copiez l'endpoint (ex : `https://formspree.io/f/abcdwxyz`)
3. Dans `index.html`, remplacez :
   ```html
   <form class="cf-form" data-r="up" onsubmit="return false;">
   ```
   par :
   ```html
   <form class="cf-form" data-r="up" action="https://formspree.io/f/abcdwxyz" method="POST">
   ```

Les demandes arriveront dans votre boîte mail.

---

## Mises à jour ultérieures

Toute modification du site se fait directement dans `index.html`.
- Pour Cloudflare Pages / Netlify / GitHub Pages : pousser un commit sur la branche connectée déclenche un redéploiement automatique
- Pour OVH/FTP : ré-uploader le fichier `index.html`
