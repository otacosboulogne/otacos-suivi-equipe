# O'TACOS — App de saisie quotidienne + dashboard

Ce pack te permet de mettre en place :

- une page web GitHub Pages pour noter les équipiers par service,
- un Google Apps Script qui stocke les données dans Google Sheets / Drive,
- un dashboard web qui lit les données du mois.

## Fichiers inclus

- `index.html` : formulaire de saisie par service
- `dashboard.html` : dashboard mensuel
- `Code.gs` : backend Apps Script
- `appsscript.json` : manifest Apps Script

## Architecture

GitHub Pages → Google Apps Script → Google Sheets (Drive)

## 1) Créer le Google Sheet

1. Va sur Google Sheets.
2. Crée un nouveau fichier, par exemple : `OTACOS_Suivi_Equipiers`.
3. Ouvre **Extensions > Apps Script**.
4. Supprime le code existant.
5. Copie-colle `Code.gs`.
6. Ajoute aussi `appsscript.json` si besoin dans l’éditeur Apps Script.
7. Enregistre.

## 2) Initialiser le classeur

Dans Apps Script :
1. Sélectionne la fonction `setupWorkbook`
2. Clique sur **Run**
3. Autorise les accès

Ça crée :
- `Equipe`
- `Services`
- `Parametres`

## 3) Déployer le backend en web app

Dans Apps Script :
1. **Deploy > New deployment**
2. Type : **Web app**
3. Execute as : **Me**
4. Who has access : **Anyone with the link**
5. Deploy
6. Copie l’URL du web app

## 4) Brancher le front HTML

Dans `index.html` et `dashboard.html`, remplace :

```js
const APPS_SCRIPT_URL = "PASTE_YOUR_GOOGLE_APPS_SCRIPT_WEBAPP_URL_HERE";
```

par ton URL Apps Script.

## 5) Héberger sur GitHub Pages

1. Crée un repo GitHub, par exemple `otacos-equipiers`
2. Upload `index.html` et `dashboard.html`
3. Va dans **Settings > Pages**
4. Source : **Deploy from a branch**
5. Branche : `main` / dossier `/root`
6. Sauvegarde

Tu obtiens :
- `https://TONCOMPTE.github.io/otacos-equipiers/`
- `https://TONCOMPTE.github.io/otacos-equipiers/dashboard.html`

## 6) Utilisation manager

Sur `index.html` :
1. renseigner restaurant, date, service, manager
2. ajouter seulement les équipiers présents
3. noter chaque équipier de 1 à 5
4. enregistrer

Chaque ligne part dans le Google Sheet `Services`.

## 7) Dashboard

`dashboard.html` calcule automatiquement :
- le gagnant du mois
- le score moyen
- le nombre de services évalués
- le classement du mois

## 8) Colonnes utilisées dans la feuille Services

- UUID
- Horodatage
- Restaurant
- DateService
- Service
- Manager
- Equipier
- Productivite
- Qualite
- Ponctualite
- Polyvalence
- EspritEquipe
- SatisfactionClient
- PertesCasse
- ScoreService100
- Commentaire
- Mois

## 9) Recommandations

- garde les notes sur une échelle de 1 à 5
- utilise les mêmes règles toute l’année
- mets à jour la feuille `Equipe` quand quelqu’un arrive ou part
- n’édite pas les en-têtes dans `Services`

## 10) Option Looker Studio

Si tu veux un dashboard encore plus corporate, connecte le même Google Sheet à Looker Studio.
Le fichier `Services` suffit pour créer :
- score moyen par équipier
- top 10 mensuel
- comparaison matin / soir
- volume d’évaluations par manager
- historique par semaine

## 11) Important

GitHub Pages est statique :
- il peut afficher une interface,
- mais le stockage doit passer par Apps Script ou une API.

C’est pour ça que le front HTML parle au backend Google Apps Script.
