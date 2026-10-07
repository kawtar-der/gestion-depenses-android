# Gestion Dépenses (application Android)

Application Android de **suivi des dépenses personnelles** : on ajoute ses dépenses (même en écrivant une phrase comme « 50 DH restaurant hier »), on les classe par catégorie, on consulte des statistiques et on exporte ses données en PDF ou CSV.

Les données sont stockées dans le cloud avec **Firebase**, ce qui permet de retrouver son compte et ses dépenses sur n'importe quel téléphone.

## Fonctionnalités

- **Compte utilisateur** : inscription et connexion par e-mail et mot de passe (Firebase Authentication). À l'inscription, l'utilisateur choisit son pays et la devise est déterminée automatiquement.
- **Ajout intelligent de dépenses** : une phrase en langage naturel est analysée pour en extraire le montant, la devise, la description, la catégorie et la date.
  - L'analyse utilise une IA (**Groq**, modèles Llama) avec **Google Gemini** en secours, puis une analyse locale par mots-clés si aucune IA n'est disponible.
  - Si la catégorie détectée n'existe pas, l'application propose de la créer.
- **Catégories** : catégories par défaut (Alimentation, Transport...) et catégories personnalisées avec leur couleur.
- **Liste des dépenses** : recherche par texte, filtres par période et par catégories, total des résultats affichés, suppression.
- **Statistiques** : graphique en camembert par catégorie et histogramme, par semaine, mois ou année (MPAndroidChart).
- **Profil** : informations du compte, **export PDF** (iText) et **export CSV**, déconnexion.

## Technologies

| Domaine | Outils |
|---|---|
| Langage | Java (Android, minSdk 26, targetSdk 34) |
| Architecture | MVVM : `model`, `repository`, `viewmodel`, `ui` |
| Base de données | Cloud Firestore (`users/{id}/expenses` et `users/{id}/categories`) |
| Authentification | Firebase Authentication |
| Graphiques | MPAndroidChart |
| IA | API Groq (Llama), Google Gemini, OkHttp, Gson |
| Export | iText 7 (PDF) |

## Structure du projet

```
app/src/main/java/com/example/gestiondepenses/
├── MainActivity.java            # Liste des dépenses, recherche, filtres
├── data/
│   ├── model/                   # User, Expense, Category
│   └── repository/              # Accès à Firestore
├── viewmodel/                   # ExpenseViewModel, CategoryViewModel
├── ui/                          # Connexion, inscription, ajout, catégories, stats, profil
│   └── adapter/                 # Adaptateurs des listes
└── utils/ExpenseAIAssistant.java  # Analyse des phrases par IA
```

## Installation

### 1. Prérequis
- [Android Studio](https://developer.android.com/studio) récent (JDK 17)
- Un téléphone Android (8.0 ou plus) ou un émulateur

### 2. Configurer Firebase (obligatoire)
Le fichier `google-services.json` n'est **pas** publié dans ce dépôt, car il contient la configuration de mon projet Firebase.
1. Créer un projet sur la [console Firebase](https://console.firebase.google.com).
2. Ajouter une application Android avec le nom de paquet `com.example.gestiondepenses`.
3. Activer **Authentication** (méthode e-mail / mot de passe) et **Cloud Firestore**.
4. Télécharger `google-services.json` et le placer dans le dossier `app/`.

### 3. Clés pour l'IA (optionnel)
Créer ou modifier le fichier `local.properties` à la racine du projet (il n'est pas publié) :

```
GROQ_API_KEY=ta_cle_groq
GEMINI_API_KEY=ta_cle_gemini
```

Sans clés, l'application fonctionne quand même : l'analyse des phrases se fait localement avec des mots-clés.

### 4. Lancer
Ouvrir le projet dans Android Studio, attendre la synchronisation Gradle, puis cliquer sur **Run**.

## Auteur

Kawthar Derouich
