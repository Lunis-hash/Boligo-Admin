# BOLIGO — Back-office Admin (dépôt archivé)

> **Ce dépôt n'est plus utilisé.** Le tableau de bord d'administration a été
> intégré au dépôt `Boligo-back`, dossier `admin/`, et il est servi par le site
> statique Render `boligo-admin` (https://boligo-admin.onrender.com/login).
> Toute modification se fait désormais là-bas ; le projet Vercel `boligo-admin`
> peut être supprimé.

Tableau de bord Next.js de l'équipe BOLIGO : membres, rencontres, parcours,
signalements, messages bloqués et finances. Il ne contient aucune donnée : il
appelle les routes `/api/admin/*` de l'API BOLIGO (dépôt `Boligo-back`).

## En ligne

- Adresse : https://boligo-admin.vercel.app/login (projet Vercel `boligo-admin`,
  redéployé à chaque fusion sur `main`).
- API appelée : `NEXT_PUBLIC_API_URL` (variable Vercel). Sans valeur, la version
  en ligne appelle `https://boligo-back.onrender.com/api`.

## Compte administrateur

Seul un compte dont le rôle est `ADMIN` peut se connecter. Aucun mot de passe
n'est fourni par défaut, et aucun ne doit être écrit dans ce dépôt.

Deux façons de créer le compte :

1. **Depuis l'application** (le mot de passe ne passe par personne d'autre) :
   - créer un compte BOLIGO normal avec l'adresse de l'administrateur ;
   - passer son rôle à `ADMIN` dans la base BOLIGO (Supabase, projet
     « Lunis-hash's Project ») :
     `update "User" set role = 'ADMIN' where email = '<adresse>';`
   - un compte `ADMIN` n'apparaît jamais dans la Découverte des membres.
2. **Par le script du dépôt `Boligo-back`** :
   `ADMIN_EMAIL=<adresse> ADMIN_PASSWORD=<12 caractères minimum> npx ts-node prisma/seed-admin.ts`,
   avec `DATABASE_URL` pointant vers la base BOLIGO.

La session dure 8 heures.

## En local

```bash
# 1. API BOLIGO (dépôt Boligo-back, port 3000)
npm run start:dev

# 2. Ce tableau de bord (port 3001)
npm install
npm run dev
```

Ouvrir http://localhost:3001/login. En local, l'API par défaut est
`http://localhost:3000/api`. Pour en viser une autre, créer `.env.local` avec
`NEXT_PUBLIC_API_URL=<adresse>/api`.

## Erreur `Cannot find module './627.js'` ou `./638.js`

Ce n'est **pas** un bug de code : le dossier **`.next`** (cache de compilation)
est corrompu ou désynchronisé. Cela arrive souvent quand le serveur de
développement tourne pendant une compilation interrompue, ou après un `EPERM`
sous Windows.

1. Arrêter le serveur de développement (`Ctrl+C`).
2. Nettoyer le cache : `npm run clean`.
3. Relancer : `npm run dev`.

Si `npm run clean` échoue (fichier verrouillé), fermer tous les terminaux Next.js
puis supprimer à la main le dossier `.next`.
