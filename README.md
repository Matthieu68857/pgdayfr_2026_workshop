# PGDay France 2026 - Atelier : Créez votre premier agent IA avec PostgreSQL

## Introduction

Durée : 5:00

Dans cet atelier pratique, vous allez concevoir, sécuriser et déployer de bout en bout un **agent d'intelligence artificielle expert météo et climat** s'appuyant sur une base de données relationnelle **PostgreSQL**. 

L'objectif principal est de déployer un serveur **MCP (Model Context Protocol)** à l'aide de **MCP Toolbox for databases** afin d'exposer vos fonctions et requêtes SQL comme des outils ("tools") directement utilisables (actionnables) par un grand modèle de langage (LLM). De plus, vous découvrirez comment l'Agent Development Kit (ADK) permet d'orchestrer cet agent en Python de manière élégante et interactive.

Tout au long de cet atelier, vous suivrez une approche progressive :

1. **Architecture MCP & Provisionnement** : Provisionner une base de données PostgreSQL (sur Google Cloud SQL) qui contiendra les données climatiques historiques.
2. **Sécurité & Gouvernance PostgreSQL** : Appliquer le principe du moindre privilège, configurer les droits d'accès, utiliser des vues et mettre en œuvre la sécurité au niveau des lignes (**RLS - Row Level Security**) pour maîtriser les accès en lecture (SELECT) sur l'historique et en écriture (INSERT/UPDATE) sur les bulletins météo.
3. **SQL as a Tool** : Configurer et lancer **MCP Toolbox for databases** pour transformer des requêtes SQL de lecture et d'écriture en capacités cognitives exposées au LLM.
4. **Orchestration avec l'ADK** : Développer un agent IA assistant météo intelligent capable de requêter vos tables de relevés météo et de rédiger et publier des bulletins enrichis d'anecdotes trouvées sur le Web grâce à Google Search.
5. **Défi DBA / Admin (Non guidé)** : Concevoir un second agent expert en administration de bases de données PostgreSQL pour analyser la santé, les performances et l'optimisation de votre instance.

---

#### **Ce que vous allez faire**
* Concevoir et construire un agent IA assistant météo capable d'analyser des historiques climatiques, de rédiger et publier des bulletins météo enrichis d'anecdotes et de répondre aux requêtes des utilisateurs.
* Sécuriser l'accès à la base de données PostgreSQL de sorte que le LLM ne puisse jamais accéder ou altérer des données confidentielles (secret défense/stations militaires), même en cas de tentative d'injection de requêtes.
* Relever le défi de créer un agent DBA/Admin pour monitorer votre base de données PostgreSQL en langage naturel.

#### **Ce que vous allez apprendre**
* Provisionner et structurer une base de données Cloud SQL pour PostgreSQL (avec tables de relevés et de bulletins rédigés).
* Mettre en place une gouvernance des données stricte en lecture et en écriture (création d'un rôle dédié, RLS, politiques de sécurité).
* Configurer un serveur MCP Toolbox pour exposer vos requêtes SQL de façon sécurisée.
* Utiliser l'ADK (Agent Development Kit) en Python pour créer et exécuter un agent IA interactif et connecté à vos outils SQL.
* Créer des outils MCP interrogeant les tables système et statistiques de PostgreSQL pour surveiller la base de données.

#### **Ce dont vous aurez besoin**
* Un navigateur web
* Un projet et compte Google Cloud (fourni au préalable)

---

## Avant de commencer

Durée : 5:00

### Accéder à votre environnement Google Cloud

> [!IMPORTANT]
> Pour cet atelier, **chaque participant recevra des identifiants individuels et un projet Google Cloud dédié** au début de la session. Il n'est donc pas nécessaire de créer un nouveau projet, d'utiliser votre compte personnel ou de configurer la facturation (billing).

1. Connectez-vous à la [Console Google Cloud](https://console.cloud.google.com/) en utilisant les identifiants fournis par les organisateurs.
2. Assurez-vous que le projet Google Cloud attribué est bien sélectionné en haut de la page.
3. Activez **Cloud Shell** en cliquant sur l'icône en haut à droite de la console.
4. Authentifiez-vous et configurez votre projet actif dans Cloud Shell :

```bash
gcloud auth list
gcloud config list project
```

Si le projet actif n'est pas le bon, définissez-le :

```bash
gcloud config set project <YOUR_PROJECT_ID>
```

5. Activez les API requises (cela peut prendre quelques minutes) :

```bash
gcloud services enable cloudresourcemanager.googleapis.com \
                       servicenetworking.googleapis.com \
                       aiplatform.googleapis.com \
                       sqladmin.googleapis.com \
                       compute.googleapis.com
```

---

## Créer l'instance PostgreSQL

Durée : 10:00

Nous utiliserons une instance **Google Cloud SQL pour PostgreSQL** pour stocker notre historique météo. 

Exécutez la commande suivante dans votre terminal Cloud Shell pour créer l'instance :

```bash
gcloud sql instances create weatherdb-instance \
--database-version=POSTGRES_15 \
--tier db-g1-small \
--region=us-central1 \
--edition=ENTERPRISE \
--root-password=postgres
```

> [!NOTE]
> La création de l'instance prend généralement entre 3 et 5 minutes. Une fois terminée, vous verrez les détails de connexion s'afficher dans le terminal.

---

## Préparer la base de données météo & Sécurité

Durée : 15:00

Nous allons maintenant structurer notre base de données historique avec des relevés climatiques, puis mettre en place une gouvernance d'accès stricte pour sécuriser les données sensibles.

### 1. Créer la base de données dédiée et se connecter à Cloud SQL Studio

1. Rendez-vous sur la [page Cloud SQL](https://console.cloud.google.com/sql/instances) dans la console Cloud.
2. Cliquez sur **`weatherdb-instance`**.
3. Dans le menu de gauche, cliquez sur **`Cloud SQL Studio`**.
4. Connectez-vous à la base de données par défaut `postgres` en utilisant l'utilisateur `postgres` et le mot de passe `postgres`.
5. Dans l'éditeur SQL, exécutez la commande suivante pour créer notre base de données dédiée aux relevés météo :
   ```sql
   CREATE DATABASE weather_db;
   ```
6. Une fois la base de données créée, déconnectez-vous (ou rechargez la page) et connectez-vous à nouveau en choisissant cette fois-ci la base de données **`weather_db`** (avec le même utilisateur `postgres` et mot de passe `postgres`).

### 2. Création des tables et des données d'exemple

Dans l'éditeur SQL, exécutez le script suivant pour créer la table des relevés historiques `climat_history` et la table des bulletins rédigés `weather_bulletins` :

```sql
-- Table d'historique climatique (lecture seule pour l'agent)
CREATE TABLE climat_history (
 id               SERIAL PRIMARY KEY,
 measure_date     DATE NOT NULL,
 city             VARCHAR NOT NULL,
 temperature      NUMERIC NOT NULL, -- Celsius
 precipitation    NUMERIC NOT NULL, -- mm
 humidity         INTEGER NOT NULL, -- %
 wind_speed       NUMERIC NOT NULL, -- km/h
 conditions       VARCHAR NOT NULL, -- ex: 'Ensoleillé', 'Pluvieux', 'Brouillard'
 is_confidential  BOOLEAN DEFAULT FALSE -- Flag de confidentialité
);

-- Table des bulletins météo rédigés (lecture/écriture pour l'agent)
CREATE TABLE weather_bulletins (
 id               SERIAL PRIMARY KEY,
 bulletin_date    DATE NOT NULL,
 city             VARCHAR NOT NULL,
 temperature      NUMERIC NOT NULL, -- Température prévue
 conditions       VARCHAR NOT NULL, -- Conditions météo
 bulletin_text    TEXT NOT NULL,    -- Texte complet rédigé du bulletin avec anecdotes
 is_confidential  BOOLEAN DEFAULT FALSE -- Flag de confidentialité
);
```

Alimentons ensuite la table historique avec des données climatiques variées (villes françaises) incluant des mesures publiques et un relevé confidentiel, et insérons un bulletin d'exemple :

```sql
INSERT INTO climat_history(measure_date, city, temperature, precipitation, humidity, wind_speed, conditions, is_confidential)
VALUES
 ('2026-05-25', 'Paris', 18.5, 0.0, 55, 12.5, 'Ensoleillé', FALSE),
 ('2026-05-25', 'Marseille', 26.0, 0.0, 40, 25.0, 'Ensoleillé', FALSE),
 ('2026-05-25', 'Lyon', 21.2, 2.5, 65, 10.0, 'Nuageux', FALSE),
 ('2026-05-25', 'Brest', 14.0, 18.2, 90, 35.0, 'Pluie', FALSE),
 ('2026-05-25', 'Strasbourg', 17.8, 4.0, 75, 8.0, 'Orageux', FALSE),
 ('2026-05-26', 'Paris', 19.0, 0.0, 50, 15.0, 'Ensoleillé', FALSE),
 ('2026-05-26', 'Marseille', 27.5, 0.0, 35, 18.0, 'Ensoleillé', FALSE),
 ('2026-05-26', 'Lyon', 22.0, 0.0, 58, 12.0, 'Ensoleillé', FALSE),
 ('2026-05-26', 'Brest', 15.0, 5.0, 85, 22.0, 'Averses', FALSE),
 ('2026-05-26', 'Strasbourg', 16.5, 12.0, 80, 15.0, 'Pluie', FALSE),
 -- Donnée hautement confidentielle
 ('2026-05-26', 'Zone Militaire Nord', 5.2, 0.0, 95, 45.0, 'Brouillard', TRUE);

-- Exemple de bulletin météo rédigé existant
INSERT INTO weather_bulletins(bulletin_date, city, temperature, conditions, bulletin_text, is_confidential)
VALUES 
 ('2026-05-25', 'Paris', 18.5, 'Ensoleillé', 'Bonjour Paris ! Aujourd''hui, un soleil radieux vous attend avec des températures printanières agréables de 18.5°C. C''est l''occasion parfaite pour flâner près de la Seine ou visiter le fameux Jardin du Luxembourg.', FALSE);
```

### 3. Mettre en œuvre la Sécurité et la Gouvernance (Privilèges et RLS)

> [!IMPORTANT]
> **Principe de moindre privilège :** L'agent IA ne doit jamais utiliser le compte administrateur (`postgres`) pour requêter la base de données. Si l'agent subissait une injection d'instructions ("Prompt Injection"), un attaquant pourrait altérer ou supprimer des tables.
> De plus, nous devons garantir que les données confidentielles (`is_confidential = TRUE`) ne sont jamais visibles par l'agent météo grand public.

Exécutez le script SQL suivant pour créer un utilisateur dédié restreint et activer la sécurité au niveau des lignes (Row Level Security) :

```sql
-- 1. Création d'un utilisateur PostgreSQL restreint pour le serveur MCP
CREATE USER agent_user WITH PASSWORD 'SecureAgentPassword123!';

-- 2. Attribution des droits sur nos tables (SELECT sur l'historique, SELECT/INSERT/UPDATE sur les bulletins)
GRANT SELECT ON climat_history TO agent_user;
GRANT SELECT, INSERT, UPDATE ON weather_bulletins TO agent_user;

-- Accorder l'utilisation de la séquence pour l'auto-incrément de l'ID lors des INSERT
GRANT USAGE, SELECT ON SEQUENCE weather_bulletins_id_seq TO agent_user;

-- 3. Activation de la sécurité au niveau des lignes (RLS) sur les deux tables
ALTER TABLE climat_history ENABLE ROW LEVEL SECURITY;
ALTER TABLE weather_bulletins ENABLE ROW LEVEL SECURITY;

-- 4. Définition des politiques RLS
-- RLS pour climat_history (Relevés historiques - lecture seule)
CREATE POLICY climat_select_policy ON climat_history
    FOR SELECT TO agent_user USING (is_confidential = FALSE);

-- RLS pour weather_bulletins (Bulletins rédigés - lecture/écriture)
CREATE POLICY bulletins_select_policy ON weather_bulletins
    FOR SELECT TO agent_user USING (is_confidential = FALSE);

CREATE POLICY bulletins_insert_policy ON weather_bulletins
    FOR INSERT TO agent_user WITH CHECK (is_confidential = FALSE);

CREATE POLICY bulletins_update_policy ON weather_bulletins
    FOR UPDATE TO agent_user USING (is_confidential = FALSE) WITH CHECK (is_confidential = FALSE);
```

#### **Comment ça fonctionne ?**
* **Lecture de l'historique météo :** Le serveur MCP, en tant que `agent_user`, ne peut lire que les données historiques météo non confidentielles. Les relevés confidentiels (ex: *Zone Militaire Nord*) sont automatiquement et hermétiquement filtrés par PostgreSQL.
* **Écriture des bulletins :** L'agent peut rédiger et enregistrer de nouveaux bulletins météo ou corriger des prévisions existantes. Cependant, les politiques RLS d'écriture garantissent qu'il ne pourra jamais écrire un bulletin associé à une zone confidentielle (`WITH CHECK (is_confidential = FALSE)`), garantissant une isolation des données sans aucune ligne de code applicative !

---

## Configurer MCP Toolbox for databases

Durée : 20:00

[MCP Toolbox for databases](https://github.com/googleapis/mcp-toolbox) est un serveur MCP open source pour les bases de données. Il a été conçu dans un esprit de qualité de production et de niveau entreprise. Il vous permet de développer des outils plus facilement, plus rapidement et de manière plus sécurisée en gérant les complexités telles que le pooling de connexions, l'authentification, et plus encore.

Toolbox vous aide à concevoir des outils d'IA générative qui permettent à vos agents d'accéder aux données de votre base de données. Toolbox fournit :

* **Un développement simplifié** : Intégrez des outils à votre agent en moins de 10 lignes de code, réutilisez des outils entre plusieurs agents ou frameworks, et déployez de nouvelles versions d'outils plus facilement.
* **De meilleures performances** : Bonnes pratiques telles que le pooling de connexions, l'authentification, et plus encore.
* **Une sécurité renforcée** : Authentification intégrée pour un accès plus sécurisé à vos données.
* **Une observabilité de bout en bout** : Métriques et traçage prêts à l'emploi avec un support intégré pour OpenTelemetry.

Toolbox se situe entre le framework d'orchestration de votre application et votre base de données, fournissant un plan de contrôle utilisé pour modifier, distribuer ou invoquer des outils. Il simplifie la gestion de vos outils en vous fournissant un emplacement centralisé pour stocker et mettre à jour les outils, vous permettant de partager des outils entre les agents et les applications et de mettre à jour ces outils sans nécessairement redéployer votre application.

Vous pouvez voir que l'une des bases de données prises en charge par MCP Toolbox for databases est Cloud SQL et nous l'avons provisionnée dans la section précédente. 

### **Installation de Toolbox**

Ouvrez le terminal Cloud Shell et créez un dossier nommé **`mcp-toolbox`**. 

```bash
mkdir mcp-toolbox
cd mcp-toolbox
```

Téléchargez le binaire de MCP Toolbox pour Linux (si vous êtes sur Mac ou Windows, téléchargez le binaire correspondant depuis les [Releases de MCP Toolbox](https://github.com/googleapis/mcp-toolbox/releases)) :

```bash
export VERSION=1.1.0
curl -L -o toolbox https://storage.googleapis.com/mcp-toolbox-for-databases/v$VERSION/linux/amd64/toolbox
chmod +x toolbox
```

Vérifiez que l'installation fonctionne en affichant la version :

```bash
./toolbox -v
```

### 2. Création du fichier `tools.yaml`

Le fichier `tools.yaml` permet de déclarer les sources de données (bases PostgreSQL) et d'associer des requêtes SQL paramétrées sous forme de fonctions documentées (outils) interprétables par les LLMs.

Créez le fichier `tools.yaml` dans le dossier `mcp-toolbox` (par exemple avec `nano tools.yaml`) :

```yaml
kind: source
name: weather-cloud-sql-source
type: cloud-sql-postgres
project: YOUR_PROJECT_ID # Remplacer par votre ID de projet Google Cloud
region: us-central1
instance: weatherdb-instance
database: weather_db
user: agent_user       # Utilisation du compte sécurisé avec RLS activé !
password: SecureAgentPassword123!
---
kind: tool
name: get-weather-by-city
type: postgres-sql
source: weather-cloud-sql-source
description: Récupère l'historique météo et les conditions climatiques d'une ville spécifique.
parameters:
  - name: city
    type: string
    description: Le nom de la ville (par exemple 'Paris', 'Marseille', 'Lyon').
statement: SELECT measure_date, city, temperature, precipitation, humidity, wind_speed, conditions FROM climat_history WHERE city ILIKE $1;
---
kind: tool
name: get-extreme-weather-events
type: postgres-sql
source: weather-cloud-sql-source
description: Recherche les événements météo extrêmes (ex: fortes chaleurs ou fortes pluies).
parameters:
  - name: min_temp
    type: number
    description: Le seuil de température minimale en degrés Celsius (ex: 25).
  - name: min_precip
    type: number
    description: Le seuil de précipitations minimales en mm (ex: 10).
statement: |
  SELECT measure_date, city, temperature, precipitation, conditions
  FROM climat_history
  WHERE temperature >= $1 OR precipitation >= $2
  ORDER BY temperature DESC;
---
kind: tool
name: publish-weather-bulletin
type: postgres-sql
source: weather-cloud-sql-source
description: Rédige et publie un nouveau bulletin météo chaleureux pour une ville et une date données.
parameters:
  - name: bulletin_date
    type: string
    description: La date du bulletin météo (format YYYY-MM-DD).
  - name: city
    type: string
    description: Le nom de la ville concernée.
  - name: temperature
    type: number
    description: La température prévue en degrés Celsius.
  - name: conditions
    type: string
    description: Les conditions météo (ex. 'Ensoleillé', 'Averses de pluie').
  - name: bulletin_text
    type: string
    description: Le texte rédigé complet du bulletin météo incluant les prévisions et anecdotes locales.
statement: |
  INSERT INTO weather_bulletins (bulletin_date, city, temperature, conditions, bulletin_text, is_confidential)
  VALUES ($1, $2, $3, $4, $5, FALSE)
  RETURNING id, bulletin_date, city;
---
kind: tool
name: update-weather-bulletin
type: postgres-sql
source: weather-cloud-sql-source
description: Met à jour ou corrige le texte et les prévisions d'un bulletin météo existant.
parameters:
  - name: temperature
    type: number
    description: La nouvelle température prévue en degrés Celsius.
  - name: conditions
    type: string
    description: Les nouvelles conditions météo.
  - name: bulletin_text
    type: string
    description: Le nouveau texte rédigé complet du bulletin.
  - name: city
    type: string
    description: Le nom de la ville.
  - name: bulletin_date
    type: string
    description: La date du bulletin à modifier (format YYYY-MM-DD).
statement: |
  UPDATE weather_bulletins
  SET temperature = $1, conditions = $2, bulletin_text = $3
  WHERE city ILIKE $4 AND bulletin_date = $5
  RETURNING id, city, bulletin_date, temperature, conditions;
---
kind: toolset
name: weather_toolset
tools:
  - get-weather-by-city
  - get-extreme-weather-events
  - publish-weather-bulletin
  - update-weather-bulletin
```

> [!TIP]
> Remarquez l'usage des variables `$1`, `$2`, etc. dans l'attribut `statement`. MCP Toolbox utilise des **requêtes préparées/paramétrées** sous le capot. Cela empêche toute vulnérabilité d'injection SQL au niveau applicatif !

### **Comprendre la structure de configuration**

Comprenons brièvement ce fichier :

1. **Les Sources** représentent vos différentes sources de données avec lesquelles un outil peut interagir. Une source représente une source de données avec laquelle un outil peut interagir. Vous pouvez définir les **`Sources`** sous forme de mappage dans la section `sources` de votre fichier `tools.yaml`. En règle générale, une configuration de source contiendra toutes les informations nécessaires pour se connecter et interagir avec la base de données. Dans notre cas, nous avons configuré une seule source qui pointe vers notre instance Cloud SQL pour PostgreSQL avec les identifiants de connexion de l'utilisateur sécurisé `agent_user`. Pour plus d'informations, reportez-vous à la référence [Sources](https://mcp-toolbox.dev/documentation/configuration/sources/).
 
2. **Les Tools** définissent les actions qu'un agent peut effectuer, telles que la lecture et l'écriture dans une source. Un outil représente une action que votre agent peut entreprendre, comme l'exécution d'une instruction SQL. Vous pouvez définir les **`Tools`** sous forme de mappage dans la section `tools` de votre fichier `tools.yaml`. En règle générale, un outil aura besoin d'une source sur laquelle agir. Dans notre cas, nous définissons deux outils : **`get-weather-by-city`** et **`get-extreme-weather-events`** et spécifions la source sur laquelle ils agissent, ainsi que le code SQL et les paramètres attendus. Pour plus d'informations, reportez-vous à la référence [Tools](https://mcp-toolbox.dev/documentation/configuration/tools/). 

3. **Enfin, nous avons le Toolset**, qui vous permet de définir des groupes d'outils que vous souhaitez charger ensemble. Cela peut être utile pour définir différents groupes en fonction de l'agent ou de l'application. Dans notre cas, nous avons un seul ensemble d'outils appelé **`weather_toolset`**, qui contient les deux outils météo que nous avons définis. 

### 3. Lancement du serveur avec Interface UI

Lancez le serveur MCP Toolbox en activant l'option UI locale :

```bash
./toolbox --config "tools.yaml" --ui
```

Vous devriez voir des logs confirmant la connexion et l'exposition des deux outils météo :

```
INFO "Initialized 1 sources: weather-cloud-sql-source" 
INFO "Initialized 4 tools: get-weather-by-city, get-extreme-weather-events, publish-weather-bulletin, update-weather-bulletin" 
INFO "Initialized 2 toolsets: default, weather_toolset" 
INFO "Server ready to serve!" 
INFO "Toolbox UI is up and running at: http://127.0.0.1:5000/ui"
```

### 4. Tester les outils via l'interface visuelle (Toolbox UI)

1. Cliquez sur le bouton **Aperçu sur le Web** dans Cloud Shell, puis sélectionnez **Changer de port** pour pointer sur le port `5000`.
2. Ajoutez manuellement **`/ui`** à la fin de l'URL de votre navigateur.
3. Vous accédez à l'interface d'administration de la Toolbox. Cliquez sur **Tools** dans le menu de gauche pour voir vos outils d'analyse météo.
4. Sélectionnez `get-weather-by-city`, renseignez le paramètre `city` avec la valeur `Brest`, puis cliquez sur **Run Tool**. Vous obtiendrez directement le retour JSON de la base de données PostgreSQL.

---

## Écrire notre agent avec l'Agent Development Kit (ADK)

Durée : 20:00

L'**Agent Development Kit (ADK)** de Google permet de structurer de véritables agents IA autonomes à l'aide de frameworks Python, capables d'orchestrer dynamiquement des appels d'outils, de planifier des tâches complexes et de tenir des conversations.

### **Comprendre le rôle des agents dans l'ADK**

Selon la [documentation de l'ADK](https://adk.dev/agents/), un agent est une unité d'exécution autonome conçue pour agir de manière autonome afin d'atteindre des objectifs spécifiques. Les agents peuvent effectuer des tâches, interagir avec les utilisateurs, utiliser des outils externes et se coordonner avec d'autres agents.

Plus précisément, un **`LLMAgent`**, communément appelé Agent, utilise de grands modèles de langage (LLM) comme moteur principal pour comprendre le langage naturel, raisonner, planifier, générer des réponses et décider de manière dynamique de la marche à suivre ou des outils à utiliser, ce qui les rend idéaux pour les tâches flexibles centrées sur le langage. Pour en savoir plus sur les agents LLM, vous pouvez consulter la [documentation ADK sur les agents](https://adk.dev/agents/llm-agents/).

### 1. Initialisation du projet ADK

Ouvrez un nouveau terminal et créez votre espace de développement Python :

```bash
mkdir my-weather-agents
cd my-weather-agents
python -m venv .venv
source .venv/bin/activate
```

Installez les dépendances requises (ADK et le client Toolbox) :

```bash
pip install google-adk toolbox-core
```

Générez le squelette de votre application d'agent via la CLI d'ADK :

```bash
adk create weather_agent_app
```

Lors de l'initialisation :
1. Choisissez le modèle **`gemini-2.5-flash`** pour l'agent racine.
2. Sélectionnez **`Vertex AI`** comme fournisseur de backend.
3. Entrez votre ID de projet Google Cloud et la région par défaut (`us-central1`).

Cela génère l'arborescence suivante dans le dossier `weather_agent_app` :
* `.env` : Contient les informations d'authentification Cloud.
* `__init__.py` : Initialise le module Python.
* `agent.py` : Le fichier de configuration de l'agent.

---

## Connecter notre agent à nos outils de données météo

Durée : 10:00

Nous allons configurer notre agent ADK pour qu'il charge dynamiquement l'ensemble d'outils (Toolset) de notre serveur MCP Toolbox, puis l'utiliser pour répondre aux questions des utilisateurs.

### 1. Modifier le fichier `agent.py`

Ouvrez le fichier `weather_agent_app/agent.py` et remplacez son contenu par le code suivant pour orchestrer nos requêtes SQL :

```python
from google.adk.agents import Agent
from toolbox_core import ToolboxSyncClient

# 1. Connexion au serveur MCP local
# Nous créons un client synchrone pointant vers notre serveur Toolbox
toolbox = ToolboxSyncClient("http://127.0.0.1:5000")

# 2. Chargement dynamique du toolset configuré pour la météo
# Cela va inspecter le serveur MCP et récupérer les définitions de nos outils SQL
weather_tools = toolbox.load_toolset('weather_toolset')

# 3. Définition de l'agent IA Assistant Météo
root_agent = Agent(
    name="expert_meteo_agent",
    model="gemini-2.5-flash",
    description=(
        "Un agent IA assistant météo capable d'analyser des historiques climatiques "
        "et de rédiger ou mettre à jour des bulletins météo dans la base de données."
    ),
    instruction=(
        "Vous êtes un présentateur météo chaleureux et expert climatologue de renom. Votre rôle "
        "est d'aider les utilisateurs à analyser les historiques de relevés météo, ainsi qu'à rédiger "
        "et publier des bulletins météo attrayants pour des villes à des dates données.\n\n"
        "Règles importantes :\n"
        "- Utilisez systématiquement les outils SQL à votre disposition pour lire l'historique météo "
        "ou publier/modifier des bulletins météo dans la base de données.\n"
        "- Rédigez des textes de bulletins complets, fluides et agréables à lire (stockés dans le champ `bulletin_text`).\n"
        "- Formatez vos réponses finales de façon claire et professionnelle avec du Markdown."
    ),
    tools=weather_tools,
)
```

### 2. Lancer et Tester l'Agent en Mode Web

Depuis votre terminal (en vous assurant d'être dans le dossier parent `my-weather-agents`), lancez le serveur web interactif fourni par l'ADK :

```bash
adk web
```

Une fois démarré, vous devriez voir un message indiquant :
`For local testing, access at http://127.0.0.1:8000`

> [!IMPORTANT]
> **Gestion des ports dans Cloud Shell :**
> Vous avez maintenant deux serveurs distincts qui s'exécutent en parallèle dans Cloud Shell : le serveur **MCP Toolbox (port 5000)** et l'interface **ADK Web (port 8000)**.
> Pour basculer de l'un à l'autre, utilisez l'option **Aperçu sur le Web** de Cloud Shell, sélectionnez **Changer de port** et saisissez le port désiré (`5000` ou `8000`). Ne fermez pas vos terminaux actifs, les deux services doivent continuer de tourner en même temps !

1. Cliquez sur **Aperçu sur le Web** dans Cloud Shell et modifiez le port pour ouvrir le port **`8000`**.
2. Une interface web moderne d'agent conversationnel apparaît dans votre navigateur !
3. Saisissez des questions complexes, par exemple :
   * *"Quelles villes ont subi des pluies supérieures à 10 mm dans l'historique ?"*
   * *"Fait-il chaud à Lyon en ce moment selon les relevés ?"*
   * *"Peux-tu rédiger un bulletin météo simple pour Marseille le 26 mai 2026 (27.5°C, ensoleillé) et l'enregistrer dans la base ?"*
   * *"Il y a une erreur sur le bulletin de Paris du 25 mai 2026 : la température était de 20°C (au lieu de 18.5°C) et c'était 'Très Ensoleillé'. Peux-tu corriger le bulletin existant ?"*

L'agent va analyser vos questions, appeler de façon autonome les outils SQL sécurisés de lecture ou d'écriture, récupérer les résultats et générer un compte-rendu naturel et synthétique.

---

### 3. Étape supplémentaire : Intégrer Google Search pour enrichir les bulletins d'anecdotes

Pour rendre notre assistant météo encore plus captivant, nous allons lui permettre d'enrichir automatiquement ses bulletins rédigés avec des **anecdotes locales historiques ou culturelles** trouvées sur le Web en temps réel.

Pour cela, nous allons combiner nos outils SQL de base de données avec l'outil de recherche en ligne intégré à l'ADK : **`google_search`**.

#### **Modifier à nouveau le fichier `agent.py`**
Ouvrez le fichier `weather_agent_app/agent.py` et remplacez son contenu par le code suivant :

```python
from google.adk.agents import Agent
from google.adk.tools import google_search
from toolbox_core import ToolboxSyncClient

# 1. Connexion au serveur MCP local
toolbox = ToolboxSyncClient("http://127.0.0.1:5000")

# 2. Chargement dynamique du toolset configuré pour la météo
weather_tools = toolbox.load_toolset('weather_toolset')

# 3. Fusion des outils SQL et de Google Search
# Nous ajoutons l'outil google_search à notre liste d'outils existants
all_tools = list(weather_tools) + [google_search]

# 4. Définition de l'agent IA Assistant Météo avec Grounding Web
root_agent = Agent(
    name="expert_meteo_agent",
    model="gemini-2.5-flash",
    description=(
        "Un agent IA assistant météo capable de requêter la base de données "
        "et d'effectuer des recherches d'anecdotes sur le Web en temps réel."
    ),
    instruction=(
        "Vous êtes un présentateur météo extrêmement chaleureux et passionné d'histoire. "
        "Votre rôle est d'aider les utilisateurs à concevoir et publier des bulletins météo uniques.\n\n"
        "Règles strictes d'orchestration :\n"
        "1. Utilisez les outils SQL pour interroger l'historique météo ou enregistrer/mettre à jour "
        "les bulletins dans la table `weather_bulletins`.\n"
        "2. Lorsque vous devez rédiger un bulletin météo pour une ville, utilisez TOUJOURS l'outil "
        "`google_search` en arrière-plan pour chercher une anecdote insolite, culturelle ou historique "
        "intéressante sur cette ville.\n"
        "3. Intégrez harmonieusement cette anecdote dans votre texte de bulletin météo (le champ `bulletin_text`).\n"
        "4. Enregistrez le bulletin rédigé ainsi enrichi dans la base de données puis faites un résumé à l'utilisateur."
    ),
    tools=all_tools,
)
```

#### **Tester l'agent avec l'intégration Google Search**
Relancez le serveur (`adk web` si vous l'aviez arrêté) et demandez-lui :
* *"Rédige et publie un bulletin météo pour Paris à la date du 26 mai 2026 (température de 19°C et conditions ensoleillées). N'oublie pas d'utiliser Google Search pour y inclure une super anecdote sur Paris !"*

**Validation dans la base de données :**
Une fois que l'agent a confirmé la publication, rendez-vous dans la console **Cloud SQL Studio** (en vous connectant sur la base **`weather_db`**) et exécutez la requête SQL suivante en tant qu'administrateur `postgres` :
```sql
SELECT * FROM weather_bulletins;
```
Vous verrez physiquement apparaître dans votre table SQL le bulletin météo complet contenant l'anecdote historique trouvée en direct par l'intelligence artificielle sur le Web !

---

## Partie 2 (Défi) : L'Agent DBA / Admin PostgreSQL (Non guidé)

Durée : 30:00 (À titre indicatif)

Pour aller plus loin et tester vos compétences acquises, vous allez concevoir un second agent autonome, cette fois-ci du côté **Administrateur de Base de Données (DBA)**. 

Cet agent n'aura plus pour rôle d'analyser la météo, mais d'aider un administrateur système à surveiller, diagnostiquer et optimiser l'instance PostgreSQL en interrogeant directement les vues système et le catalogue PostgreSQL.

### **Le But à atteindre**
Créer un agent capable de répondre en langage naturel à des questions sur l'état de la base de données, ses performances et son intégrité (ex: tables les plus lourdes, index inutilisés, requêtes lentes actives).

---

### **Grandes lignes pour réussir le défi**

#### **1. Sécurité et Privilèges (SQL)**
L'agent DBA a besoin d'accéder aux statistiques système de PostgreSQL. Plutôt que de lui donner le rôle de superutilisateur `postgres` (ce qui serait une mauvaise pratique de sécurité), utilisez le principe du moindre privilège.
* **Astuce :** PostgreSQL dispose d'un rôle prédéfini très pratique appelé `pg_read_all_stats`. Il permet de lire toutes les statistiques et tables système sans être superutilisateur.
* **Action :** Créez un nouvel utilisateur restreint `dba_user` et attribuez-lui ce rôle :
  ```sql
  CREATE USER dba_user WITH PASSWORD 'SecureDbaPassword123!';
  GRANT pg_read_all_stats TO dba_user;
  ```

#### **2. Configurer les outils système dans la Toolbox (`tools-admin.yaml`)**
Créez un nouveau fichier de configuration `tools-admin.yaml` (ou enrichissez le premier) pour définir des outils SQL qui interrogent le catalogue.
Voici des exemples de requêtes SQL utiles à implémenter comme outils MCP :

* **Taille des tables & index (`get-database-size-metrics`)** :
  ```sql
  SELECT 
      relname AS table_name, 
      pg_size_pretty(pg_total_relation_size(relid)) AS total_size,
      pg_size_pretty(pg_relation_size(relid)) AS table_size,
      pg_size_pretty(pg_total_relation_size(relid) - pg_relation_size(relid)) AS index_size
  FROM pg_catalog.pg_statio_user_tables
  ORDER BY pg_total_relation_size(relid) DESC;
  ```

* **Requêtes en cours / Requêtes lentes (`get-active-queries`)** :
  ```sql
  SELECT pid, age(clock_timestamp(), query_start), usename, state, query
  FROM pg_stat_activity
  WHERE state != 'idle' AND query NOT LIKE '%pg_stat_activity%'
  ORDER BY query_start ASC;
  ```

* **Utilisation des index et détection des index manquants (`get-index-usage-stats`)** :
  ```sql
  SELECT 
      schemaname, relname, seq_scan, seq_tup_read, idx_scan, idx_tup_fetch
  FROM pg_stat_user_tables
  WHERE seq_scan > 0;
  ```

* **Version et uptime de la base de données (`get-db-system-info`)** :
  ```sql
  SELECT version(), pg_postmaster_start_time() as uptime;
  ```

#### **3. Créer le nouvel Agent dans l'ADK**
* Dans votre projet Python ADK, créez un nouvel agent `dba_agent` (dans un nouveau script ou dans `agent.py`).
* Configurez la source MCP pour se connecter en tant que `dba_user`.
* Donnez-lui des instructions adaptées : *"Vous êtes un Administrateur de Base de Données (DBA) PostgreSQL expert. Votre rôle est d'aider le développeur à diagnostiquer sa base..."*

---

### **Scénarios de test pour valider votre défi**
Une fois l'agent fonctionnel, testez-le dans la console interactive en posant des questions comme :
1. *"Quelles sont les tables les plus volumineuses de ma base de données et combien d'espace les index consomment-ils ?"*
2. *"Y a-t-il des requêtes actives lentes ou bloquées en ce moment ?"*
3. *"Est-ce que mes tables utilisent correctement les index ou est-ce que je devrais en créer de nouveaux ?"*
4. *"Quelle est la version exacte de mon serveur PostgreSQL ?"*

> [!NOTE]
> **À vous de jouer !** Expérimentez en ajoutant d'autres requêtes système à votre fichier de configuration MCP Toolbox pour étendre les compétences cognitives de votre agent DBA.

---

## Félicitations !

Vous avez construit avec succès un agent IA **expert météo de bout en bout** connecté à une base de données **PostgreSQL** via l'architecture **MCP**.

### **Ce que vous avez accompli :**
1. **Déploiement d'une base PostgreSQL** : Provisionnement et peuplement d'une base Cloud SQL.
2. **Sécurité & Gouvernance avancée** : Application du principe de moindre privilège avec un utilisateur dédié et configuration de politiques **Row Level Security (RLS)** directement dans la base de données pour filtrer les données sensibles.
3. **Intégration MCP Toolbox** : Exposition des requêtes SQL paramétrées comme des outils standardisés.
4. **Orchestration ADK** : Développement d'une application d'agent IA interactive en Python.

### **Documents de référence pour aller plus loin :**
* [Agent Development Kit (ADK) Documentation](https://adk.dev/)
* [MCP Toolbox for Databases Github](https://github.com/googleapis/mcp-toolbox)
* [Model Context Protocol (MCP) Specifications](https://modelcontextprotocol.io/)
* [Google Cloud SQL for PostgreSQL Security Best Practices](https://cloud.google.com/sql/docs/postgres/security)
