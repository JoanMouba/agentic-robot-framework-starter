# Robot Framework — Web Testing Starter Project
### Epignosis CRM Demo · SeleniumLibrary · Robot Framework Version 7

---

**Epignosis Center** · Formateur Robot Framework Accrédité
www.epignosis.center · contact@epignosis.center

---

## À propos de ce projet

Ce projet est un template de démarrage pour les tests Web avec Robot Framework Version 7
et SeleniumLibrary. Il suit l'architecture **Page Object Model (POM)** et utilise
l'application de démonstration **Epignosis CRM Demo** comme cible de tests.

Il inclut aussi deux **Agent Skills** prêts à l'emploi, pour générer et auditer des tests
avec Claude Code — voir la section [Agent Skills](#agent-skills) ci-dessous.

Conçu pour les testeurs manuels qui débutent l'automatisation et les équipes
qui veulent une base de projet propre et structurée.

---

## Structure du projet

```
agentic-robot-framework-starter/
│
├── Tests/
│   ├── connexion_tests.robot     # 9 tests — page de connexion (/signin)
│   └── clients_tests.robot       # 9 tests — page clients (/customers)
│
├── Resources/
│   ├── commun.resource           # Mots-clés communs (open/close browser)
│   ├── connexion_res.resource    # POM — Page de connexion
│   └── clients_res.resource      # POM — Page clients + formulaire
│
├── Config/
│   └── variables.py              # URL, identifiants, navigateur, timeouts
│
├── Results/                      # Généré automatiquement à l'exécution
│
├── skills-templates/             # Agent Skills prêts à copier dans .claude/skills/
│   ├── creer-tests-rf-de-user-story/
│   │   └── SKILL.md
│   └── auditer-conformite-conventions/
│       └── SKILL.md
│
├── requirements.txt              # Dépendances Python
└── README.md                     # Ce fichier
```

---

## Prérequis

### 1. Python 3.10+
Vérifiez votre version :
```bash
python --version
```

### 2. Mettre à jour pip (recommandé)
```bash
python -m pip install --upgrade pip
```

### 3. Installer les dépendances
```bash
pip install -r requirements.txt
```

> `webdriver-manager` est inclus dans `requirements.txt` et gère automatiquement
> le téléchargement du bon ChromeDriver pour votre version de Chrome.

### 4. Utiliser webdriver-manager (optionnel mais recommandé)

Si vous souhaitez gérer le WebDriver automatiquement dans vos scripts Python :
```python
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.service import Service

driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
```

Pour Robot Framework avec SeleniumLibrary, ChromeDriver doit être dans votre PATH.
La méthode la plus simple :
```bash
# Option A — ChromeDriver via webdriver-manager (en ligne de commande)
python -c "from webdriver_manager.chrome import ChromeDriverManager; print(ChromeDriverManager().install())"

# Option B — Téléchargement manuel
# https://chromedriver.chromium.org/downloads
```

---

## Application testée

| Élément        | Valeur                                          |
|----------------|-------------------------------------------------|
| URL accueil    | https://epignosis-crm-demo.onrender.com         |
| Page connexion | /signin                                         |
| Page clients   | /customers                                      |
| Nouveau client | /new-customer                                   |
| Email de test  | admin@fakeemail.com                             |
| Mot de passe   | SecretSauce!25                                  |

> ⚠️ L'application est hébergée sur Render (plan gratuit).
> Le premier chargement peut prendre 30–60 secondes si elle est en veille.
> Augmentez `TIMEOUT` dans `Config/variables.py` si nécessaire.

---

## Lancer les tests

> `--console none` évite un bug d'encodage connu sur certaines configurations
> Windows (`LookupError: unknown encoding: utf-8:surrogateescape`). Les résultats
> restent entièrement disponibles dans `Results/` même sans affichage console.

### Tous les tests
```bash
robot --console none --outputdir Results/ Tests/
```

### Tests de connexion uniquement
```bash
robot --console none --outputdir Results/ Tests/connexion_tests.robot
```

### Tests clients uniquement
```bash
robot --console none --outputdir Results/ Tests/clients_tests.robot
```

### Tests smoke uniquement (validation rapide)
```bash
robot --include smoke --console none --outputdir Results/ Tests/
```

### Tests négatifs uniquement
```bash
robot --include negatif --console none --outputdir Results/ Tests/
```

### Exclure les fonctionnalités pas encore développées
```bash
robot --exclude not-implemented --console none --outputdir Results/ Tests/
```

### Changer le navigateur
```bash
robot --variable NAVIGATEUR:firefox --console none --outputdir Results/ Tests/
robot --variable NAVIGATEUR:edge    --console none --outputdir Results/ Tests/
```

---

## Consulter les rapports

Après exécution, ouvrir dans le navigateur :

```
Results/report.html   → Rapport de synthèse
Results/log.html      → Log détaillé de chaque étape
```

---

## Architecture Page Object Model (POM)

Ce projet suit le pattern POM : chaque page a son propre fichier ressource.

```
Page Web          →    Fichier ressource
/signin           →    Resources/connexion_res.resource
/customers        →    Resources/clients_res.resource
/new-customer     →    Resources/clients_res.resource
```

**Avantages :**
- Si un localisateur change, vous ne modifiez qu'un seul fichier
- Les tests restent lisibles et indépendants de l'implémentation
- Les mots-clés sont réutilisables entre les tests

---

## Agent Skills

Ce projet inclut deux Agent Skills pour Claude Code, dans `skills-templates/` :

| Skill | Rôle |
|-------|------|
| `creer-tests-rf-de-user-story` | Génère des tests Robot Framework à partir d'une user story avec critères d'acceptation |
| `auditer-conformite-conventions` | Vérifie qu'un fichier `.robot` ou `.resource` respecte les conventions du projet |

**Pour les activer**, copiez le dossier voulu vers `.claude/skills/` :

```bash
mkdir -p .claude/skills/creer-tests-rf-de-user-story
cp skills-templates/creer-tests-rf-de-user-story/SKILL.md .claude/skills/creer-tests-rf-de-user-story/SKILL.md
```

`skills-templates/` est volontairement séparé de `.claude/skills/` : rien ne se déclenche tant que vous n'avez pas copié un fichier au bon endroit.

---

## Tags disponibles

| Tag                  | Description                              |
|-----------------------|------------------------------------------|
| `smoke`              | Tests de validation rapide               |
| `connexion`          | Tous les tests de connexion              |
| `clients`            | Tous les tests de la page clients        |
| `connexion-valide`   | Tests de connexion réussie               |
| `connexion-invalide` | Tests de connexion échouée               |
| `negatif`            | Tests avec données invalides             |
| `champs-vides`       | Tests avec champs vides                  |
| `affichage`          | Tests d'interface                        |
| `formulaire`         | Tests du formulaire nouveau client       |
| `navigation`         | Tests de navigation entre pages          |
| `ajout`              | Tests d'ajout de client                  |
| `recherche`          | Tests de recherche de client             |
| `not-implemented`    | Échec attendu — fonctionnalité pas encore développée dans l'app |

---

## Personnaliser pour votre projet

### Changer l'application cible
Modifier `Config/variables.py` :
```python
URL_BASE     = "https://votre-application.com"
EMAIL_VALIDE = "votre@email.com"
MDP_VALIDE   = "VotreMotDePasse"
```

### Ajouter une nouvelle page
1. Créer `Resources/ma_page_res.resource`
2. Définir les localisateurs dans `*** Variables ***`
3. Écrire les mots-clés dans `*** Keywords ***`
4. Créer `Tests/ma_page_tests.robot`
5. Importer : `Resource    ../Resources/ma_page_res.resource`

---

## Dépannage fréquent

| Problème                        | Solution                                                    |
|----------------------------------|--------------------------------------------------------------|
| `WebDriverException`            | Mettre à jour ChromeDriver ou utiliser webdriver-manager    |
| `ElementNotVisibleException`    | Augmenter `TIMEOUT` dans `Config/variables.py`              |
| `NoSuchElementException`        | Vérifier les localisateurs dans le fichier ressource        |
| Page lente à charger            | App Render en veille — attendre 60s et relancer             |
| Tests instables                 | Augmenter `VITESSE` dans `Config/variables.py` (ex: `0.5s`) |
| pip obsolète                    | `python -m pip install --upgrade pip`                       |

---

## Prochaine étape

Ce template est la base. Pour aller plus loin avec Epignosis Center :

- **Mémo Syntaxe RF Version 7** — Référence complète en 2 pages
- **Pack 50 Prompts IA** — Prompts Claude & ChatGPT pour RF
- **Formation Robot Framework Fondations** — epignosiscenter.podia.com

---

*Epignosis Center — Transformer vos équipes QA en Experts Robot Framework*
*www.epignosis.center · contact@epignosis.center*
