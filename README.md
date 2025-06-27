# VISUAL - Plateforme de Streaming Participatif

**Slogan :** _Regarde, investit, gagne !_

VISUAL est une plateforme révolutionnaire combinant streaming et investissement participatif. Elle permet aux porteurs de projets de présenter leurs créations, aux investisseurs de soutenir et rentabiliser leurs investissements, et aux visiteurs de découvrir du contenu créatif unique.

## 🚀 Fonctionnalités Principales

### Multi-Utilisateurs
- **Visiteurs** : Accès gratuit aux extraits et découverte des projets
- **Investisseurs** : Investissement dans les projets, accès au contenu complet, participation aux gains
- **Porteurs** : Dépôt de projets créatifs, réception de financements participatifs
- **Administrateurs** : Gestion complète de la plateforme

### Système de Paiement
- Intégration Stripe sécurisée pour tous les paiements
- Système bancaire interne STARBANK (compte courant + épargne)
- Redistributions automatiques basées sur les performances
- Retraits sécurisés vers comptes bancaires

### Contenu & Streaming
- 6 catégories : Films, Vidéos/Clips, Théâtre, Documentaires, Émissions TV, Livres/Audio
- Lecteur vidéo intégré avec système d'extraits/contenu complet
- Chat en temps réel sur chaque projet
- Système de notation et commentaires

### Intelligence Artificielle
- Modération automatique des contenus
- Support client multilingue
- Traductions automatiques
- Suggestions personnalisées

## 🛠 Installation

### Prérequis
- Node.js 18+
- PostgreSQL
- Compte Stripe (clés API)

### Installation Rapide

```bash
# Cloner le projet
git clone <repository-url>
cd visual

# Copier et configurer les variables d'environnement
cp .env.example .env
# Remplir vos clés Stripe et autres configurations dans .env

# Installer les dépendances
npm install

# Configurer la base de données
npx prisma migrate dev
npx prisma generate

# Lancer l'application
npm run dev
```

L'application sera accessible sur [http://localhost:3000](http://localhost:3000)

### Configuration Stripe

1. Créez un compte sur [dashboard.stripe.com](https://dashboard.stripe.com)
2. Récupérez vos clés API (publishable et secret)
3. Ajoutez-les dans votre fichier `.env` :

```env
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
```

## 🏗 Architecture Technique

### Frontend
- **Next.js 14** avec TypeScript
- **React 18** pour l'interface utilisateur
- **CSS personnalisé** avec design responsive
- **Stripe Elements** pour les paiements

### Backend
- **Next.js API Routes** pour l'API REST
- **NextAuth.js** pour l'authentification
- **Prisma ORM** avec PostgreSQL
- **Stripe API** pour les paiements

### Base de Données
- **PostgreSQL** avec Prisma ORM
- Schéma complet : utilisateurs, projets, investissements, paiements, social
- Relations optimisées et indexées

### Sécurité
- Authentification JWT sécurisée
- Protection CSRF et XSS
- Validation des données côté serveur
- Chiffrement des mots de passe (bcrypt)

## 🎨 Design & UX

### Palette de Couleurs
- **Primaire** : #b3c6e6 (Bleu pastel)
- **Secondaire** : #f6e7d7 (Crème)
- **Accent** : #ffe3e3 (Corail)
- **Succès** : #81b29a (Vert)
- **Danger** : #ef476f (Rouge)

### Responsive Design
- Mobile-first approche
- Breakpoints optimisés (mobile, tablette, desktop)
- Interface intuitive accessible dès 7 ans

## 🚀 Déploiement

### Plateformes Recommandées

**Frontend + Backend :**
- **Vercel** (recommandé pour Next.js)
- **Netlify**
- **Railway**

**Base de Données :**
- **Supabase** (PostgreSQL managé)
- **Railway** (avec PostgreSQL)
- **PlanetScale** (MySQL compatible)

### Déploiement sur Vercel

1. Connectez votre repository à Vercel
2. Configurez les variables d'environnement
3. Déployez automatiquement via Git

### Variables d'Environnement de Production

```env
DATABASE_URL="postgresql://..."
NEXTAUTH_URL="https://votre-domaine.com"
NEXTAUTH_SECRET="votre-secret-production"
STRIPE_SECRET_KEY="sk_live_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_live_..."
STRIPE_WEBHOOK_SECRET="whsec_..."
OPENAI_API_KEY="sk-..."
```

## 🔧 Scripts Disponibles

```bash
# Développement
npm run dev

# Build de production
npm run build

# Démarrage production
npm start

# Base de données
npm run db:migrate    # Appliquer les migrations
npm run db:generate   # Générer le client Prisma
```

## 📁 Structure du Projet

```
visual/
├── src/
│   ├── components/     # Composants React réutilisables
│   ├── pages/         # Pages Next.js et API routes
│   ├── lib/           # Utilitaires et configurations
│   └── styles/        # Styles CSS globaux
├── prisma/
│   └── schema.prisma  # Schéma de base de données
├── public/            # Assets statiques
└── README.md
```

## 🤝 Contribution

1. Forkez le projet
2. Créez une branche feature (`git checkout -b feature/AmazingFeature`)
3. Committez vos changements (`git commit -m 'Add some AmazingFeature'`)
4. Poussez vers la branche (`git push origin feature/AmazingFeature`)
5. Ouvrez une Pull Request

## 📞 Support

- **Email** : support@visual.com
- **Documentation** : [docs.visual.com](https://docs.visual.com)
- **Chat IA** : Disponible directement dans l'application

## 📜 Licence

Ce projet est sous licence MIT. Voir le fichier `LICENSE` pour plus de détails.

---

**VISUAL** - _Regarde, investit, gagne !_ 🎬💰✨