boutikpilot/README.md
# BoutikPilot
# BoutikPilot

BoutikPilot est une application de gestion destinée aux petites et moyennes boutiques de vêtements.

Elle permet notamment de gérer :

- les produits ;
- les tailles et les couleurs ;
- le stock ;
- les ventes ;
- les clients ;
- les fournisseurs ;
- les dépenses ;
- les reçus ;
- les annulations ;
- la clôture de caisse ;
- les utilisateurs et leurs rôles ;
- les rapports de base.

L’application est conçue pour fonctionner sur :

- Android ;
- iOS ;
- ordinateur via navigateur web.

## Technologies

- Expo ;
- React Native ;
- TypeScript ;
- Expo Router ;
- Supabase ;
- PostgreSQL ;
- Supabase Auth ;
- Supabase Row Level Security ;
- Expo Camera ;
- Expo Print ;
- Expo Sharing ;
- EAS Build.

## Prérequis

Installez les éléments suivants :

- Node.js LTS ;
- npm ;
- Git ;
- un compte Supabase ;
- un compte Expo pour les compilations EAS.

Vérifiez les installations :

```bash
node --version
npm --version
Installation du projet
Placez-vous dans le dossier du projet :
cd boutikpilot
Installez les dépendances :
npm install
Installez les modules Expo supplémentaires :
npx expo install expo-camera expo-print expo-sharing
Configuration Supabase
Créez un projet Supabase de test.
Dans Supabase, ouvrez :
Project Settings → API
Copiez :
l’URL du projet ;
la clé anon.
Ne copiez jamais la clé service_role dans l’application.
Créez le fichier .env à partir du modèle :
cp .env.example .env
Puis renseignez :
EXPO_PUBLIC_SUPABASE_URL=[https://votre-projet.supabase.co](https://votre-projet.supabase.co)
EXPO_PUBLIC_SUPABASE_ANON_KEY=votre-cle-anon
Le fichier .env ne doit jamais être publié sur GitHub.
Configuration de la base de données
Exécutez les scripts SQL dans cet ordre dans Supabase SQL Editor :
1. supabase/schema.sql
2. supabase/adjust_stock.sql
3. supabase/migration_02.sql
Utilisez de préférence un projet Supabase vierge pour le premier test.
Après l’exécution, vérifiez :
que toutes les tables ont été créées ;
que RLS est activé ;
que les politiques sont présentes ;
que les fonctions SQL ne sont pas exécutables par anon ou public.
Création du premier utilisateur
Dans Supabase :
Authentication → Users → Add user
Copiez l’UUID de l’utilisateur.
Créez ensuite une boutique :
insert into public.stores (
  name,
  currency
)
values (
  'Ma boutique',
  'CDF'
)
returning id;
Copiez l’identifiant de la boutique.
Créez le profil du gérant :
insert into public.profiles (
  id,
  store_id,
  full_name,
  role,
  active
)
values (
  'UUID_DE_UTILISATEUR',
  'UUID_DE_LA_BOUTIQUE',
  'Gérant principal',
  'owner',
  true
);
Lancement en développement
Lancez le serveur Expo :
npx expo start
Pour lancer la version web :
npx expo start --web
Pour ouvrir directement Android :
npx expo start --android
Pour ouvrir directement iOS :
npx expo start --ios
Pour utiliser Expo Go, installez l’application Expo Go sur le téléphone et scannez le QR code affiché dans le terminal ou le navigateur.
Si le téléphone ne parvient pas à joindre l’ordinateur :
npx expo start --tunnel
Comptes et rôles
Owner
Le propriétaire peut :
gérer les produits ;
modifier les prix ;
ajuster le stock ;
annuler une vente ;
gérer l’équipe ;
consulter les rapports ;
clôturer la caisse ;
gérer les fournisseurs et les dépenses.
Manager
Le manager peut généralement :
gérer les produits ;
ajuster le stock ;
annuler une vente ;
gérer les fournisseurs ;
consulter les rapports ;
clôturer la caisse.
Seller
Le vendeur peut généralement :
consulter les produits ;
enregistrer une vente ;
rechercher un article ;
scanner un code-barres ;
consulter les opérations qui lui sont autorisées.
Le vendeur ne doit pas pouvoir :
modifier un prix ;
ajouter un produit ;
modifier directement le stock ;
annuler une vente ;
gérer les utilisateurs ;
clôturer la caisse.
Les permissions sont protégées côté interface et côté base Supabase.
Fonctionnalités principales
Produits
Chaque variante de vêtement peut être enregistrée comme une ligne indépendante :
ROB-FLE-M
ROB-FLE-L
ROB-FLE-XL
Une variante peut avoir :
un SKU ;
une taille ;
une couleur ;
un prix d’achat ;
un prix de vente ;
une quantité ;
un seuil minimum ;
un code-barres.
Ventes
Le module de vente permet :
de rechercher un produit ;
de scanner un code-barres ;
d’ajouter un article au panier ;
d’enregistrer le paiement ;
d’associer un client ;
d’appliquer une remise ;
de diminuer automatiquement le stock ;
de générer un reçu.
Annulation
Une annulation doit :
exiger un motif ;
être réservée aux rôles autorisés ;
modifier le statut de la vente ;
remettre le stock ;
conserver l’historique ;
créer un mouvement de stock inverse.
Dépenses
Le module permet d’enregistrer :
une dépense en espèces ;
une dépense bancaire ;
une dépense mobile money ;
un motif ;
un montant ;
un justificatif si cette fonction est activée.
Clôture de caisse
La clôture compare :
Espèces attendues
avec
Espèces réellement comptées
Formule :
Écart = Montant compté - Montant attendu
Une deuxième clôture pour la même période doit être refusée.
Reçus
Le reçu peut être :
affiché après la vente ;
partagé par WhatsApp ;
partagé par une autre application ;
imprimé si le périphérique le permet.
Tests recommandés
Testez d’abord avec une boutique fictive et des montants de test.
Test de base
connexion owner ;
création d’un produit ;
consultation du stock ;
vente d’un article ;
vérification de la diminution du stock ;
affichage du reçu.
Test de sécurité
un utilisateur de la boutique A ne voit pas les produits de B ;
un vendeur ne peut pas modifier un produit ;
un vendeur ne peut pas ajuster le stock ;
un vendeur ne peut pas annuler une vente ;
un vendeur ne peut pas clôturer la caisse ;
une fonction SQL sensible est refusée à un utilisateur non autorisé.
Test de stock
vente avec stock disponible ;
vente avec stock insuffisant ;
deux ventes simultanées du dernier article ;
annulation d’une vente ;
vérification du stock après annulation.
Test de caisse
clôture sans écart ;
écart positif ;
écart négatif ;
deuxième clôture refusée ;
clôture tentée par un vendeur.
Compilation Android
Installez EAS CLI :
npm install --global eas-cli
Connectez votre compte Expo :
eas login
Vérifiez le compte :
eas whoami
Configurez EAS :
eas build:configure
Créez un APK de test :
eas build --platform android --profile preview
Le profil preview est destiné aux tests internes et à l’installation directe sur des téléphones. EAS Build génère des fichiers installables à partir du projet Expo. �
Compilation iOS
Pour créer une version iOS :
eas build --platform ios --profile preview
Pour les stores :
eas build --platform all --profile production
La publication iOS nécessite un compte Apple Developer.
Variables d’environnement EAS
Pour une compilation cloud, configurez les variables dans EAS :
eas env:create \
  --name EXPO_PUBLIC_SUPABASE_URL \
  --value [https://votre-projet.supabase.co](https://votre-projet.supabase.co) \
  --environment production
Puis :
eas env:create \
  --name EXPO_PUBLIC_SUPABASE_ANON_KEY \
  --value votre-cle-anon \
  --environment production
Ne configurez jamais la clé service_role dans l’application.
Mode hors connexion
Le mode hors connexion n’est pas encore disponible.
En cas de coupure :
notez la vente dans un cahier ;
notez le SKU ;
notez la taille et la couleur ;
notez la quantité ;
notez le montant ;
notez le mode de paiement ;
saisissez la vente dans l’application après reconnexion ;
marquez la vente comme saisie.
Identifiants manuels recommandés :
OFF-20261006-001
OFF-20261006-002
Limitations actuelles
Les fonctions suivantes peuvent encore nécessiter un développement :
mode hors connexion ;
planning du personnel ;
pointage ;
promotions programmées ;
fidélité client ;
analyses avancées ;
retours partiels ;
échanges ;
paiements mixtes ;
commandes fournisseurs avancées ;
intégration complète avec les caisses physiques.
Sécurité
Respectez les règles suivantes :
ne jamais utiliser service_role dans l’application ;
ne jamais publier .env ;
activer RLS sur les tables ;
vérifier les politiques par boutique ;
limiter les fonctions SQL sensibles ;
utiliser des mots de passe forts ;
désactiver les comptes inactifs ;
effectuer des sauvegardes ;
tester la restauration ;
conserver les journaux d’audit.
Dépannage
L’application ne trouve pas Supabase
Vérifiez :
cat .env
Les variables doivent commencer par :
EXPO_PUBLIC_
Redémarrez ensuite Expo :
npx expo start --clear
Le produit n’apparaît pas
Vérifiez :
le store_id ;
le profil de l’utilisateur ;
la colonne active ;
les politiques RLS ;
la session Supabase.
La vente est refusée
Vérifiez :
le stock disponible ;
le rôle de l’utilisateur ;
l’appartenance à la boutique ;
la fonction create_sale() ;
les paramètres envoyés par le panier.
La caméra ne fonctionne pas
Vérifiez :
npx expo install expo-camera
Puis testez sur un appareil réel et autorisez la caméra dans les paramètres du téléphone.
Passage en production
Avant d’utiliser l’application avec de vraies boutiques :
testez avec une boutique pilote ;
conservez le cahier ou le tableur en parallèle ;
comparez les ventes chaque soir ;
vérifiez le stock ;
vérifiez les annulations ;
vérifiez les clôtures ;
activez les sauvegardes ;
utilisez un projet Supabase de production séparé ;
créez une version de production EAS ;
documentez les procédures de secours.
Licence
Projet interne de gestion commerciale.
À adapter selon les besoins juridiques, commerciaux et fiscaux de l’entreprise.

## Fichiers associés

Le `README.md` explique le projet, mais il ne remplace pas les fichiers de configuration suivants :

```text
.env.example
.gitignore
app.json
eas.json
package.json
supabase/schema.sql
supabase/adjust_stock.sql
supabase/migration_02.sql
Le fichier eas.json est utilisé par EAS CLI pour les profils de compilation et doit se trouver à côté de package.json.�
Création rapide du fichier
Depuis la racine du projet :
touch README.md
Ouvrez-le avec VS Code :
code README.md
Collez ensuite le contenu ci-dessus et enregistrez.
Pour vérifier qu’il existe :
ls
Vous devez voir :
README.md
