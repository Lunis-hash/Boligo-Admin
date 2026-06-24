# BOLIGO — Back-office Admin

Panel Next.js (port **3001** par défaut) pour gérer utilisateurs, matchs, parcours et modération.

## Démarrage

```bash
# 1. Backend API (port 3000)
cd ../backend
npm run start:dev

# 2. Admin
cd admin
cp .env.local.example .env.local   # si besoin
npm install
npm run dev
```

Ouvrir **http://localhost:3001/login**

Compte admin (après seed) :
- Email : `admin@boligo.app`
- Mot de passe : `AdminBoligo2026!` (ou `ADMIN_PASSWORD` du seed)

```bash
cd ../backend
npx ts-node prisma/seed-admin.ts
```

## Erreur `Cannot find module './627.js'` ou `./638.js`

Ce n’est **pas** un bug de code : le dossier **`.next`** (cache de compilation) est **corrompu** ou **désynchronisé** — souvent quand le serveur dev tourne pendant un build interrompu ou un `EPERM` Windows.

**Correction :**

1. **Arrêter** le serveur dev (`Ctrl+C` dans le terminal `npm run dev`)
2. Nettoyer le cache :
   ```bash
   cd admin
   npm run clean
   ```
3. Relancer :
   ```bash
   npm run dev
   ```

Si `npm run clean` échoue (fichier verrouillé), fermer tous les terminaux Next.js puis supprimer manuellement le dossier `admin\.next`.
