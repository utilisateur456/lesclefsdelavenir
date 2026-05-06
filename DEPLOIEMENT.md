# Déploiement de lesclefsdelavenir.ch

Site statique monofichier (`index.html`) prêt à être mis en ligne. Aucun serveur ni base de données n'est nécessaire.

## Contenu du dépôt

- `index.html` — site complet (HTML + CSS + JS + SVG inline)
- `CNAME` — domaine personnalisé GitHub Pages (`www.lesclefsdelavenir.ch`)
- `DEPLOIEMENT.md` — ce fichier

Tout est embarqué : pas de build step, pas de dépendances locales (GSAP est chargé depuis le CDN cdnjs).

---

## Configuration actuelle — GitHub Pages + Hostpoint DNS

### État DNS Hostpoint (configuré le 06.05.2026)

| Sous-domaine | Type | Valeur |
|---|---|---|
| `www.lesclefsdelavenir.ch` | CNAME | `utilisateur456.github.io` |
| `lesclefsdelavenir.ch` | — | *(géré par Hostpoint — voir note)* |

### Ce qui fonctionne

- **`https://www.lesclefsdelavenir.ch`** → votre site GitHub Pages ✓
- Le fichier `CNAME` dans ce dépôt indique à GitHub Pages de servir `www.lesclefsdelavenir.ch`

### Note sur l'apex (sans www)

L'adresse `lesclefsdelavenir.ch` (sans `www`) ne pointera **pas** vers GitHub Pages tant qu'il n'y a pas d'enregistrements A. Pour activer l'apex :

**Option 1 — Ajouter les enregistrements A** (si Hostpoint n'a pas de conflit) :
```
A @ → 185.199.108.153
A @ → 185.199.109.153
A @ → 185.199.110.153
A @ → 185.199.111.153
```
Puis dans GitHub → Settings → Pages → vérifier le domaine → cocher **Enforce HTTPS**.

**Option 2 — Redirection apex→www via Hostpoint** :
Dans votre panneau Hostpoint, activez une redirection permanente (301) de `lesclefsdelavenir.ch` vers `https://www.lesclefsdelavenir.ch`.

---

## Option A — Hostpoint FTP (alternative simple)

Si vous préférez utiliser l'hébergement Hostpoint inclus dans votre forfait :

1. Connectez-vous au FTP/SFTP (FileZilla, Cyberduck…)
2. Allez dans le dossier racine public (`www/` ou `htdocs/`)
3. Uploadez `index.html` à la racine
4. Dans Hostpoint DNS, supprimez le CNAME `www → utilisateur456.github.io` et laissez Hostpoint gérer les A records

**Coût** : compris dans votre forfait Hostpoint existant.

---

## Étapes GitHub Pages (rappel)

1. GitHub → repo → **Settings** → **Pages**
2. Source : **Deploy from a branch** → branche `claude/github-pages-dns-config-IHZMN` (ou `main`)
3. Custom domain : `www.lesclefsdelavenir.ch`
4. Cocher **Enforce HTTPS** (disponible après vérification DNS)

---

## Vérifications avant mise en ligne

- [ ] Tester le site sur mobile (responsive 600px)
- [ ] Vérifier les 3 liens de paiement (Twint, PayPal, cartes)
- [ ] Tester le numéro `tel:0901212212` sur mobile
- [ ] Tester le lien WhatsApp `wa.me/41782535452`
- [ ] Configurer un service de réception de formulaire (Formspree, FormKeep…)

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

---

## Mises à jour ultérieures

Toute modification du site se fait directement dans `index.html`.
Pousser un commit sur la branche connectée déclenche un redéploiement automatique.
