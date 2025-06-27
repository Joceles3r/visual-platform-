# Guide d'Intégration Stripe pour VISUAL

## 📋 Vue d'ensemble

Ce guide vous accompagne dans la configuration complète de Stripe pour la plateforme VISUAL, incluant les paiements, webhooks, et la redistribution automatique des fonds.

## 🔧 Configuration Initiale

### 1. Création du Compte Stripe

1. Rendez-vous sur [dashboard.stripe.com](https://dashboard.stripe.com)
2. Créez un compte Stripe Business
3. Validez votre identité (requis pour les paiements réels)
4. Activez votre compte pour accepter les paiements

### 2. Récupération des Clés API

1. Dans le Dashboard Stripe, allez dans **Développeurs > Clés API**
2. Récupérez vos clés :
   - **Clé publiable** : `pk_test_...` (pour le frontend)
   - **Clé secrète** : `sk_test_...` (pour le backend)

### 3. Configuration des Variables d'Environnement

Ajoutez dans votre fichier `.env` :

```env
# Stripe Configuration
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_votre_cle_publiable
STRIPE_SECRET_KEY=sk_test_votre_cle_secrete
STRIPE_WEBHOOK_SECRET=whsec_votre_secret_webhook
```

## 🔗 Configuration des Webhooks

### 1. Création du Webhook

1. Dans le Dashboard Stripe : **Développeurs > Webhooks**
2. Cliquez sur **Ajouter un endpoint**
3. URL : `https://votre-domaine.com/api/stripe/webhook`
4. Sélectionnez les événements :
   - `payment_intent.succeeded`
   - `payment_intent.payment_failed`
   - `payment_method.attached`

### 2. Configuration du Secret

1. Après création, récupérez le **Secret de signature**
2. Ajoutez-le dans votre `.env` : `STRIPE_WEBHOOK_SECRET=whsec_...`

## 💳 Types de Paiements VISUAL

### 1. Investissements dans les Projets

```javascript
// Montants minimum et maximum
const MIN_INVESTMENT = 200; // 2€ en centimes
const MAX_INVESTMENT = 100000; // 1000€ en centimes

// Montants prédéfinis
const PRESET_AMOUNTS = [200, 500, 1000, 2000, 5000, 10000];
```

### 2. Frais de Plateforme

- **Commission VISUAL** : 2.5% sur chaque investissement
- **Frais Stripe** : ~2.9% + 0.25€ par transaction
- **Total frais** : ~5.4% + 0.25€

### 3. Redistribution des Gains

```javascript
// Logique de redistribution
const platformFee = totalAmount * 0.025; // 2.5% pour VISUAL
const redistributionPool = totalAmount - platformFee;
const top10Share = redistributionPool * 0.90; // 90% pour le top 10
const communityShare = redistributionPool * 0.10; // 10% pour la communauté
```

## 🔄 Flux de Paiement

### 1. Processus Standard

1. **Utilisateur** choisit un montant d'investissement
2. **Frontend** crée un PaymentIntent via `/api/stripe/create-payment-intent`
3. **Stripe Elements** collecte les informations de carte
4. **Confirmation** du paiement côté client
5. **Webhook** traite le paiement réussi
6. **Base de données** mise à jour automatiquement

### 2. Gestion des Erreurs

```javascript
// Codes d'erreur courants
const ERROR_CODES = {
  'card_declined': 'Carte refusée par votre banque',
  'insufficient_funds': 'Fonds insuffisants',
  'expired_card': 'Carte expirée',
  'incorrect_cvc': 'Code CVC incorrect',
  'processing_error': 'Erreur de traitement'
};
```

## 🏦 Intégration STARBANK

### 1. Comptes Utilisateurs

Chaque utilisateur possède :
- **Compte Courant** : pour les transactions courantes
- **Compte Épargne** : pour sécuriser les gains

### 2. Mouvements Automatiques

```sql
-- Exemple de mouvement après investissement réussi
UPDATE starbank_accounts 
SET balance = balance + investment_amount 
WHERE user_id = investor_id;

-- Redistribution des gains (top 10)
UPDATE starbank_accounts 
SET balance = balance + calculated_reward 
WHERE user_id IN (top_10_investors);
```

## 🧪 Tests avec Stripe

### 1. Cartes de Test

```javascript
// Cartes de test Stripe
const TEST_CARDS = {
  success: '4242424242424242',
  declined: '4000000000000002',
  insufficient_funds: '4000000000009995',
  expired: '4000000000000069'
};
```

### 2. Scénarios de Test

1. **Paiement réussi** : Utiliser `4242424242424242`
2. **Paiement refusé** : Utiliser `4000000000000002`
3. **Fonds insuffisants** : Utiliser `4000000000009995`
4. **Authentification 3D Secure** : Utiliser `4000002500003155`

## 🚀 Passage en Production

### 1. Activation du Compte Live

1. Complétez les informations business dans Stripe
2. Validez votre identité et vos informations bancaires
3. Activez votre compte pour les paiements live

### 2. Migration des Clés

```env
# Production Environment
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_live_...
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_live_...
```

### 3. Configuration des Webhooks Production

1. Créez un nouveau webhook pour votre domaine de production
2. Utilisez la même configuration d'événements
3. Mettez à jour le `STRIPE_WEBHOOK_SECRET`

## 📊 Monitoring et Analytics

### 1. Dashboard Stripe

Surveillez :
- Volume des transactions
- Taux de réussite des paiements
- Chargebacks et disputes
- Revenus nets

### 2. Métriques VISUAL

```javascript
// KPIs à surveiller
const METRICS = {
  total_investments: 'Montant total investi',
  average_investment: 'Investissement moyen',
  success_rate: 'Taux de réussite des paiements',
  platform_revenue: 'Revenus de la plateforme'
};
```

## 🔒 Sécurité et Conformité

### 1. PCI Compliance

- Stripe gère la conformité PCI DSS
- Ne stockez jamais de données de carte
- Utilisez uniquement les tokens Stripe

### 2. Prévention de la Fraude

```javascript
// Configuration Radar (anti-fraude Stripe)
const FRAUD_RULES = {
  block_if_cvc_fails: true,
  block_if_address_mismatch: true,
  limit_per_card: 5000 // 50€ max par carte/jour
};
```

## 🆘 Résolution de Problèmes

### 1. Erreurs Courantes

**Webhook non reçu :**
- Vérifiez l'URL du webhook
- Confirmez que le endpoint répond en 200
- Vérifiez les logs Stripe

**Paiement en attente :**
- Authentification 3D Secure requise
- Carte nécessitant une confirmation bancaire

### 2. Support

- **Documentation Stripe** : [stripe.com/docs](https://stripe.com/docs)
- **Support Stripe** : Via le dashboard
- **Support VISUAL** : support@visual.com

## 📞 Contact

Pour toute question spécifique à l'intégration VISUAL :
- **Email** : tech@visual.com
- **Documentation** : [docs.visual.com/stripe](https://docs.visual.com/stripe)

---

**🔐 Sécurité Garantie** | **💳 Paiements Instantanés** | **🌍 International**