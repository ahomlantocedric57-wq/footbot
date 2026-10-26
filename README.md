# FootBot — version indépendante

Application web de suivi et d'analyse du football, conçue pour être hébergée sans Manus.

## Fonctionnalités
- Matchs par date, scores, statuts, logos et compétitions.
- Filtre par championnat et recherche d'équipes.
- Catalogue de compétitions disponibles chez le fournisseur.
- Classements par championnat.
- Prédictions du fournisseur et, si les données le permettent, cinq scores exacts estimés par une distribution de Poisson.
- Mise en cache côté serveur pour limiter les appels API.
- Clé API conservée côté serveur, jamais dans le navigateur.
- Interface responsive en français.

## 1. Obtenir une clé API
1. Créez un compte sur https://dashboard.api-football.com/
2. Copiez votre clé API.
3. Les quotas et compétitions disponibles dépendent de votre forfait. Tous les championnats ne fournissent pas nécessairement toutes les statistiques.

Documentation officielle : https://www.api-football.com/documentation_v3

## 2. Lancer sur Windows
Installez Python 3.11 ou 3.12 depuis https://www.python.org/downloads/ (cochez « Add Python to PATH »).

Ouvrez PowerShell dans le dossier du projet, puis exécutez :

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

Ouvrez `.env` avec le Bloc-notes et remplacez `COLLEZ_VOTRE_CLE_API_ICI` par votre vraie clé. Ne partagez pas ce fichier et ne l'envoyez jamais dans un dépôt public.

Démarrez l'application :

```powershell
python app.py
```

Puis ouvrez http://127.0.0.1:5000

Si PowerShell bloque l'activation de l'environnement, utilisez directement :
```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe app.py
```

## 3. Hébergement indépendant avec Render
1. Créez un dépôt Git privé et ajoutez les fichiers de ce projet, sauf `.env`.
2. Dans Render, choisissez **New → Web Service** et reliez le dépôt.
3. Render détectera `render.yaml`. Vous pouvez aussi saisir :
   - Build command : `pip install -r requirements.txt`
   - Start command : `gunicorn app:app`
4. Dans les variables d'environnement du service, définissez `API_FOOTBALL_KEY` avec votre clé.
5. Déployez puis ouvrez l'URL fournie par Render.

Guide Render : https://render.com/docs/deploy-flask

Ne placez jamais la clé API dans `static/index.html`, dans un dépôt public ou dans une capture d'écran.

## Routes API
- `GET /api/health`
- `GET /api/fixtures?date=YYYY-MM-DD`
- `GET /api/fixtures?live=all`
- `GET /api/leagues?season=2026`
- `GET /api/standings?league=39&season=2026`
- `GET /api/predictions/ID_DU_MATCH`
- `GET /api/status`

## Notes importantes
- Les données réelles ne seront disponibles qu'après configuration d'une clé valide et selon les droits/quotas de votre abonnement API.
- Les horaires sont demandés en fuseau `Africa/Porto-Novo`.
- Le calcul de scores exacts ne s'affiche que si les données nécessaires sont réellement retournées par le fournisseur. Sinon, FootBot explique pourquoi il ne peut pas les calculer.
- Les prédictions sportives comportent de l'incertitude et ne garantissent aucun résultat.
- Les logos et données de compétitions peuvent être soumis à des droits détenus par leurs propriétaires respectifs.
