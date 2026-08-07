# Barographe — mise en ligne

Safari sur iPhone refuse d'ouvrir un fichier local (`file://`). Il faut servir ces
fichiers derrière une URL **HTTPS** — c'est aussi la condition pour que le service
worker fonctionne et que l'app tourne hors réseau au terrain.

Les six fichiers vont **à plat**, dans le même dossier :

    index.html
    sw.js
    manifest.webmanifest
    apple-touch-icon.png
    icon-192.png
    icon-512.png

## Option A — GitHub Pages

1. Nouveau dépôt public, par exemple `barographe`.
2. « Add file → Upload files », déposer les six fichiers, commit.
3. Settings → Pages → Source : `Deploy from a branch`, branche `main`, dossier `/ (root)`.
4. L'URL arrive en une minute : `https://<compte>.github.io/barographe/`

## Option B — nginx sur un serveur existant

    scp -r barographe/ user@serveur:/srv/barographe/
    docker run -d --name barographe --restart unless-stopped \
      -p 8088:80 -v /srv/barographe:/usr/share/nginx/html:ro nginx:alpine

Puis exposer en HTTPS derrière le reverse proxy ou un tunnel Cloudflare.
En HTTP simple l'app s'affiche, mais le service worker est refusé : pas de mode hors ligne.

## Installation sur l'iPhone

Ouvrir l'URL dans **Safari** (pas Chrome), puis Partager → **Sur l'écran d'accueil**.
Lancer l'app une fois avec du réseau : elle met tout en cache et fonctionne
ensuite sans connexion.

## Mise à jour

Modifier `index.html`, puis incrémenter `VERSION` dans `sw.js`
(`barographe-v1` → `barographe-v2`) pour forcer le renouvellement du cache.
