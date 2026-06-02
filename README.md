# PGDay France 2026 - Atelier : Créez votre premier agent IA avec PostgreSQL

## Sommaire

1. [Introduction](https://github.com/Matthieu68857/pgdayfr_2026_workshop#1-introduction)
2. [Avant de commencer](https://github.com/Matthieu68857/pgdayfr_2026_workshop#2-avant-de-commencer)
   - 2.1. [Accéder à votre environnement Google Cloud](https://github.com/Matthieu68857/pgdayfr_2026_workshop#21-accéder-à-votre-environnement-google-cloud)
3. [Créer l'instance PostgreSQL](https://github.com/Matthieu68857/pgdayfr_2026_workshop#3-créer-linstance-postgresql)
4. [Préparer la base de données météo et sécurité](https://github.com/Matthieu68857/pgdayfr_2026_workshop#4-préparer-la-base-de-données-météo-et-sécurité)
   - 4.1. [Créer la base de données dédiée et se connecter à Cloud SQL Studio](https://github.com/Matthieu68857/pgdayfr_2026_workshop#41-créer-la-base-de-données-dédiée-et-se-connecter-à-cloud-sql-studio)
   - 4.2. [Création des tables et des données d'exemple](https://github.com/Matthieu68857/pgdayfr_2026_workshop#42-création-des-tables-et-des-données-dexemple)
   - 4.3. [Mettre en œuvre la sécurité et la gouvernance (Privilèges et RLS)](https://github.com/Matthieu68857/pgdayfr_2026_workshop#43-mettre-en-œuvre-la-sécurité-et-la-gouvernance-privilèges-et-rls)
5. [Configurer MCP Toolbox for databases](https://github.com/Matthieu68857/pgdayfr_2026_workshop#5-configurer-mcp-toolbox-for-databases)
   - 5.1. [Installation de Toolbox](https://github.com/Matthieu68857/pgdayfr_2026_workshop#51-installation-de-toolbox)
   - 5.2. [Création du fichier tools.yaml](https://github.com/Matthieu68857/pgdayfr_2026_workshop#52-création-du-fichier-toolsyaml)
   - 5.3. [Comprendre la structure de configuration](https://github.com/Matthieu68857/pgdayfr_2026_workshop#53-comprendre-la-structure-de-configuration)
   - 5.4. [Lancement du serveur avec Interface UI](https://github.com/Matthieu68857/pgdayfr_2026_workshop#54-lancement-du-serveur-avec-interface-ui)
   - 5.5. [Tester les outils via l'interface visuelle (Toolbox UI)](https://github.com/Matthieu68857/pgdayfr_2026_workshop#55-tester-les-outils-via-linterface-visuelle-toolbox-ui)
6. [Partie 1 : Votre premier agent IA connecté à PostgreSQL (Mono-Agent)](https://github.com/Matthieu68857/pgdayfr_2026_workshop#6-partie-1--votre-premier-agent-ia-connecté-à-postgresql-mono-agent)
   - 6.1. [Comprendre le rôle des agents dans l'ADK](https://github.com/Matthieu68857/pgdayfr_2026_workshop#61-comprendre-le-rôle-des-agents-dans-ladk)
   - 6.2. [Initialisation du projet ADK](https://github.com/Matthieu68857/pgdayfr_2026_workshop#62-initialisation-du-projet-adk)
   - 6.3. [Créer et connecter notre premier agent dans agent.py](https://github.com/Matthieu68857/pgdayfr_2026_workshop#63-créer-et-connecter-notre-premier-agent-dans-agentpy)
   - 6.4. [Lancer et Tester l'Agent en Mode Web](https://github.com/Matthieu68857/pgdayfr_2026_workshop#64-lancer-et-tester-lagent-en-mode-web)
7. [Partie 2 : Collaboration Multi-Agents avancée (Orchestration ADK)](https://github.com/Matthieu68857/pgdayfr_2026_workshop#7-partie-2--collaboration-multi-agents-avancée-orchestration-adk)
   - 7.1. [Comprendre l'intérêt de la collaboration Multi-Agents](https://github.com/Matthieu68857/pgdayfr_2026_workshop#71-comprendre-lintérêt-de-la-collaboration-multi-agents)
   - 7.2. [Refactoriser agent.py pour implémenter le flux Multi-Agents](https://github.com/Matthieu68857/pgdayfr_2026_workshop#72-refactoriser-agentpy-pour-implémenter-le-flux-multi-agents)
   - 7.3. [Lancer et Tester l'Équipe d'Agents Météo](https://github.com/Matthieu68857/pgdayfr_2026_workshop#73-lancer-et-tester-léquipe-dagents-météo)
8. [Partie 3 : Défi - L'Agent DBA et Admin PostgreSQL (Non guidé)](https://github.com/Matthieu68857/pgdayfr_2026_workshop#8-partie-3--défi---lagent-dba-et-admin-postgresql-non-guidé)
   - 8.1. [Le but à atteindre](https://github.com/Matthieu68857/pgdayfr_2026_workshop#81-le-but-à-atteindre)
   - 8.2. [Grandes lignes pour réussir le défi](https://github.com/Matthieu68857/pgdayfr_2026_workshop#82-grandes-lignes-pour-réussir-le-défi)
   - 8.3. [Scénarios de test pour valider votre défi](https://github.com/Matthieu68857/pgdayfr_2026_workshop#83-scénarios-de-test-pour-valider-votre-défi)
9. [Défi Ultime : RAG sémantique & Embeddings 100% natifs dans PostgreSQL (Non guidé - Niveau Expert)](https://github.com/Matthieu68857/pgdayfr_2026_workshop#9-défi-ultime--rag-sémantique--embeddings-100-natifs-dans-postgresql-non-guidé---niveau-expert)
   - 9.1. [Le But et le Scénario](https://github.com/Matthieu68857/pgdayfr_2026_workshop#91-le-but-et-le-scénario)
   - 9.2. [Les Contraintes et Étapes de Réalisation](https://github.com/Matthieu68857/pgdayfr_2026_workshop#92-les-contraintes-et-étapes-de-réalisation)
   - 9.3. [Scénarios de Test pour Valider le Défi](https://github.com/Matthieu68857/pgdayfr_2026_workshop#93-scénarios-de-test-pour-valider-le-défi)
10. [Félicitations !](https://github.com/Matthieu68857/pgdayfr_2026_workshop#10-félicitations)

---

## 1. Introduction

Durée : 5:00

Dans cet atelier pratique, vous allez concevoir, sécuriser et déployer de bout en bout un **agent d'intelligence artificielle expert météo et climat** s'appuyant sur une base de données relationnelle **PostgreSQL**. 

L'objectif principal est de déployer un serveur **MCP (Model Context Protocol)** à l'aide de **MCP Toolbox for databases** afin d'exposer vos fonctions et requêtes SQL comme des outils ("tools") directement utilisables (actionnables) par un grand modèle de langage (LLM). De plus, vous découvrirez comment l'Agent Development Kit (ADK) permet d'orchestrer cet agent en Python de manière élégante et interactive.

Tout au long de cet atelier, vous suivrez une approche progressive :

1. **Architecture MCP & Provisionnement** : Provisionner une base de données PostgreSQL (sur Google Cloud SQL) qui contiendra les données climatiques historiques.
2. **Sécurité & Gouvernance PostgreSQL** : Appliquer le principe du moindre privilège, configurer les droits d'accès, utiliser des vues et mettre en œuvre la sécurité au niveau des lignes (**RLS - Row Level Security**) pour maîtriser les accès en lecture (SELECT) sur l'historique et en écriture (INSERT/UPDATE) sur les bulletins météo.
3. **SQL as a Tool** : Configurer et lancer **MCP Toolbox for databases** pour transformer des requêtes SQL de lecture et d'écriture en capacités cognitives exposées au LLM.
4. **Orchestration avec l'ADK** : Développer un agent IA assistant météo intelligent capable de requêter vos tables de relevés météo et de rédiger et publier des bulletins enrichis d'anecdotes trouvées sur le Web grâce à Google Search.
5. **Défi DBA / Admin (Non guidé)** : Concevoir un second agent expert en administration de bases de données PostgreSQL pour analyser la santé, les performances et l'optimisation de votre instance.
6. **Défi Ultime - PostgreSQL + IA (Non guidé)** : Bâtir de toutes pièces une architecture de RAG et d'embeddings 100% native dans PostgreSQL en couplant `pgvector`, `google_ml_integration` (Vertex AI) et des Triggers automatiques PL/pgSQL.

---

#### **Ce que vous allez faire**
* Concevoir et construire un agent IA assistant météo capable d'analyser des historiques climatiques, de rédiger et publier des bulletins météo enrichis d'anecdotes et de répondre aux requêtes des utilisateurs.
* Sécuriser l'accès à la base de données PostgreSQL de sorte que le LLM ne puisse jamais accéder ou altérer des données confidentielles (secret défense/stations militaires), même en cas de tentative d'injection de requêtes.
* Relever le défi de créer un agent DBA/Admin pour monitorer votre base de données PostgreSQL en langage naturel.
* Réaliser une intégration IA poussée et 100% native dans PostgreSQL (RAG, pgvector, triggers d'embeddings automatiques).

#### **Ce que vous allez apprendre**
* Provisionner et structurer une base de données Cloud SQL pour PostgreSQL (avec tables de relevés et de bulletins rédigés).
* Mettre en place une gouvernance des données stricte en lecture et en écriture (création d'un rôle dédié, RLS, politiques de sécurité).
* Configurer un serveur MCP Toolbox pour exposer vos requêtes SQL de façon sécurisée.
* Utiliser l'ADK (Agent Development Kit) en Python pour créer et exécuter un agent IA interactif et connecté à vos outils SQL.
* Créer des outils MCP interrogeant les tables système et statistiques de PostgreSQL pour surveiller la base de données.
* Configurer une architecture RAG moderne intégralement gérée par le moteur PostgreSQL via des triggers PL/pgSQL et Vertex AI.

#### **Ce dont vous aurez besoin**
* Un navigateur web
* Un projet et compte Google Cloud (fourni au préalable)

---

## 2. Avant de commencer

Durée : 5:00

### 2.1. Accéder à votre environnement Google Cloud

> [!IMPORTANT]
> Pour cet atelier, **chaque participant recevra des identifiants individuels et un projet Google Cloud dédié** au début de la session. Il n'est donc pas nécessaire de créer un nouveau projet, d'utiliser votre compte personnel ou de configurer la facturation (billing).

1. Connectez-vous à la [Console Google Cloud](https://console.cloud.google.com/) en utilisant les identifiants fournis par les organisateurs.
2. Assurez-vous que le projet Google Cloud attribué est bien sélectionné en haut de la page.
3. Activez **Cloud Shell** en cliquant sur l'icône en haut à droite de la console.
4. Activez les API requises (cela peut prendre quelques minutes) :

```bash
gcloud services enable cloudresourcemanager.googleapis.com \
                       servicenetworking.googleapis.com \
                       aiplatform.googleapis.com \
                       sqladmin.googleapis.com \
                       compute.googleapis.com
```

---

## 3. Créer l'instance PostgreSQL

Durée : 10:00

Nous utiliserons une instance **Google Cloud SQL pour PostgreSQL** pour stocker notre historique météo. 

Exécutez la commande suivante dans votre terminal Cloud Shell pour créer l'instance :

```bash
gcloud sql instances create weatherdb-instance \
--database-version=POSTGRES_17 \
--tier db-g1-small \
--region=europe-west1 \
--edition=ENTERPRISE \
--root-password=postgres
```

> [!NOTE]
> La création de l'instance prend généralement entre 3 et 5 minutes. Une fois terminée, vous verrez les détails de connexion s'afficher dans le terminal.

---

## 4. Préparer la base de données météo et sécurité

Durée : 15:00

Nous allons maintenant structurer notre base de données historique avec des relevés climatiques, puis mettre en place une gouvernance d'accès stricte pour sécuriser les données sensibles.

### 4.1. Créer la base de données dédiée et se connecter à Cloud SQL Studio

1. Rendez-vous sur la [page Cloud SQL](https://console.cloud.google.com/sql/instances) dans la console Cloud.
2. Cliquez sur **`weatherdb-instance`**.
3. Dans le menu de gauche, cliquez sur **`Cloud SQL Studio`**.
4. Connectez-vous à la base de données par défaut `postgres` en utilisant l'utilisateur `postgres` et le mot de passe `postgres`.
5. Dans l'éditeur SQL, exécutez la commande suivante pour créer notre base de données dédiée aux relevés météo :
   ```sql
   CREATE DATABASE weather_db;
   ```
6. Une fois la base de données créée, déconnectez-vous (ou rechargez la page) et connectez-vous à nouveau en choisissant cette fois-ci la base de données **`weather_db`** (avec le même utilisateur `postgres` et mot de passe `postgres`).

### 4.2. Création des tables et des données d'exemple

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

### 4.3. Mettre en œuvre la sécurité et la gouvernance (Privilèges et RLS)

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

## 5. Configurer MCP Toolbox for databases

Durée : 20:00

[MCP Toolbox for databases](https://github.com/googleapis/mcp-toolbox) est un serveur MCP open source pour les bases de données. Il a été conçu dans un esprit de qualité de production et de niveau entreprise. Il vous permet de développer des outils plus facilement, plus rapidement et de manière plus sécurisée en gérant les complexités telles que le pooling de connexions, l'authentification, et plus encore.

Toolbox vous aide à concevoir des outils d'IA générative qui permettent à vos agents d'accéder aux données de votre base de données. Toolbox fournit :

* **Un développement simplifié** : Intégrez des outils à votre agent en moins de 10 lignes de code, réutilisez des outils entre plusieurs agents ou frameworks, et déployez de nouvelles versions d'outils plus facilement.
* **De meilleures performances** : Bonnes pratiques telles que le pooling de connexions, l'authentification, et plus encore.
* **Une sécurité renforcée** : Authentification intégrée pour un accès plus sécurisé à vos données.
* **Une observabilité de bout en bout** : Métriques et traçage prêts à l'emploi avec un support intégré pour OpenTelemetry.

Toolbox se situe entre le framework d'orchestration de votre application et votre base de données, fournissant un plan de contrôle utilisé pour modifier, distribuer ou invoquer des outils. Il simplifie la gestion de vos outils en vous fournissant un emplacement centralisé pour stocker et mettre à jour les outils, vous permettant de partager des outils entre les agents et les applications et de mettre à jour ces outils sans nécessairement redéployer votre application.

Vous pouvez voir que l'une des bases de données prises en charge par MCP Toolbox for databases est Cloud SQL et nous l'avons provisionnée dans la section précédente. 

### 5.1. Installation de Toolbox

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

### 5.2. Création du fichier tools.yaml

Le fichier `tools.yaml` permet de déclarer les sources de données (bases PostgreSQL) et d'associer des requêtes SQL paramétrées sous forme de fonctions documentées (outils) interprétables par les LLMs.

Créez le fichier `tools.yaml` dans le dossier `mcp-toolbox` (par exemple avec `nano tools.yaml`) :

```yaml
kind: source
name: weather-cloud-sql-source
type: cloud-sql-postgres
project: YOUR_PROJECT_ID # Remplacer par votre ID de projet Google Cloud
region: europe-west1
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
description: "Recherche les événements météo extrêmes (ex: fortes chaleurs ou fortes pluies)."
parameters:
  - name: min_temp
    type: integer
    description: "Le seuil de température minimale en degrés Celsius (ex: 25)."
  - name: min_precip
    type: integer
    description: "Le seuil de précipitations minimales en mm (ex: 10)."
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
    type: integer
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
    type: integer
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

### 5.3. Comprendre la structure de configuration

Comprenons brièvement ce fichier :

1. **Les Sources** représentent vos différentes sources de données avec lesquelles un outil peut interagir. Une source représente une source de données avec laquelle un outil peut interagir. Vous pouvez définir les **`Sources`** sous forme de mappage dans la section `sources` de votre fichier `tools.yaml`. En règle générale, une configuration de source contiendra toutes les informations nécessaires pour se connecter et interagir avec la base de données. Dans notre cas, nous avons configuré une seule source qui pointe vers notre instance Cloud SQL pour PostgreSQL avec les identifiants de connexion de l'utilisateur sécurisé `agent_user`. Pour plus d'informations, reportez-vous à la référence [Sources](https://mcp-toolbox.dev/documentation/configuration/sources/).
 
2. **Les Tools** définissent les actions qu'un agent peut effectuer, telles que la lecture et l'écriture dans une source. Un outil représente une action que votre agent peut entreprendre, comme l'exécution d'une instruction SQL. Vous pouvez définir les **`Tools`** sous forme de mappage dans la section `tools` de votre fichier `tools.yaml`. En règle générale, un outil aura besoin d'une source sur laquelle agir. Dans notre cas, nous définissons deux outils : **`get-weather-by-city`** et **`get-extreme-weather-events`** et spécifions la source sur laquelle ils agissent, ainsi que le code SQL et les paramètres attendus. Pour plus d'informations, reportez-vous à la référence [Tools](https://mcp-toolbox.dev/documentation/configuration/tools/). 

3. **Enfin, nous avons le Toolset**, qui vous permet de définir des groupes d'outils que vous souhaitez charger ensemble. Cela peut être utile pour définir différents groupes en fonction de l'agent ou de l'application. Dans notre cas, nous avons un seul ensemble d'outils appelé **`weather_toolset`**, qui contient les deux outils météo que nous avons définis. 

### 5.4. Lancement du serveur avec Interface UI

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

### 5.5. Tester les outils via l'interface visuelle (Toolbox UI)

1. Cliquez sur le bouton **Aperçu sur le Web** dans Cloud Shell, puis sélectionnez **Changer de port** pour pointer sur le port `5000`.
2. Ajoutez manuellement **`/ui`** à la fin de l'URL de votre navigateur.
3. Vous accédez à l'interface d'administration de la Toolbox. Cliquez sur **Tools** dans le menu de gauche pour voir vos outils d'analyse météo.
4. Sélectionnez `get-weather-by-city`, renseignez le paramètre `city` avec la valeur `Brest`, puis cliquez sur **Run Tool**. Vous obtiendrez directement le retour JSON de la base de données PostgreSQL.

---

## 6. Partie 1 : Votre premier agent IA connecté à PostgreSQL (Mono-Agent)

Durée : 20:00

L'**Agent Development Kit (ADK)** de Google permet de structurer de véritables agents IA autonomes à l'aide de frameworks Python, capables d'orchestrer dynamiquement des appels d'outils, de planifier des tâches complexes et de tenir des conversations.

Dans cette première partie, nous allons initialiser notre projet Python et créer un premier agent simple (**mono-agent**) capable de se connecter directement à nos requêtes SQL exposées via MCP Toolbox.

### 6.1. Comprendre le rôle des agents dans l'ADK

Un agent utilise de grands modèles de langage (LLM) comme moteur principal pour comprendre le langage naturel, raisonner, planifier, générer des réponses et décider de manière dynamique de la marche à suivre ou des outils à utiliser, ce qui les rend idéaux pour les tâches flexibles centrées sur le langage.

### 6.2. Initialisation du projet ADK

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
3. Entrez votre ID de projet Google Cloud et la région par défaut (`europe-west1`).

Cela génère l'arborescence suivante dans le dossier `weather_agent_app` :
* `.env` : Contient les informations d'authentification Cloud.
* `__init__.py` : Initialise le module Python.
* `agent.py` : Le fichier de configuration de l'agent.

### 6.3. Créer et connecter notre premier agent dans agent.py

Ouvrez le fichier `weather_agent_app/agent.py` et remplacez son contenu par le code suivant pour orchestrer nos requêtes SQL de lecture et d'écriture :

```python
from google.adk.agents import Agent
from toolbox_core import ToolboxSyncClient

# 1. Connexion au serveur MCP local
# Nous créons un client synchrone pointant vers notre serveur Toolbox
toolbox = ToolboxSyncClient("http://127.0.0.1:5000")

# 2. Chargement dynamique du toolset configuré pour la météo
# Cela va inspecter le serveur MCP et récupérer les définitions de nos outils SQL
weather_tools = toolbox.load_toolset('weather_toolset')

# 3. Définition de l'agent IA Assistant Météo Mono-Agent
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

### 6.4. Lancer et Tester l'Agent en Mode Web

Depuis votre terminal (en vous assurant d'être dans le dossier parent `my-weather-agents`), lancez le serveur web interactif fourni par l'ADK :

```bash
adk web --allow_origins "*"
```

> [!IMPORTANT]
> **Accès via Google Cloud Shell (CORS / Erreur 403 Forbidden) :**
> Comme vous utilisez **Google Cloud Shell**, l'aperçu web passe par un proxy sécurisé (`googleusercontent.com`). Pour éviter une erreur `403 Forbidden` ou `Impossible de créer la session` lors de l'envoi de vos questions, vous **devez** impérativement ajouter l'option `--allow_origins "*"` lors du lancement de la commande `adk web`.

Une fois démarré, vous devriez voir un message indiquant :
`For local testing, access at http://127.0.0.1:8000`

> [!IMPORTANT]
> **Gestion des ports dans Cloud Shell :**
> Vous avez maintenant deux serveurs distincts qui s'exécutent en parallèle dans Cloud Shell : le serveur **MCP Toolbox (port 5000)** et l'interface **ADK Web (port 8000)**.
> Pour basculer de l'un à l'autre, utilisez l'option **Aperçu sur le Web** de Cloud Shell, sélectionnez **Changer de port** et saisissez le port désiré (`5000` ou `8000`).

1. Cliquez sur **Aperçu sur le Web** dans Cloud Shell et modifiez le port pour ouvrir le port **`8000`**.
2. Une interface web moderne d'agent conversationnel apparaît dans votre navigateur !
3. Saisissez des questions simples, par exemple :
   * *"Quelles villes ont subi des pluies supérieures à 10 mm dans l'historique ?"*
   * *"Fait-il chaud à Lyon en ce moment selon les relevés ?"*

L'agent va analyser vos questions, appeler de façon autonome les outils SQL sécurisés de lecture, récupérer les résultats et générer un compte-rendu naturel.

---

## 7. Partie 2 : Collaboration Multi-Agents avancée (Orchestration ADK)

Durée : 15:00

Pour rendre notre assistant météo encore plus captivant, nous voulons qu'il enrichisse automatiquement ses bulletins rédigés avec des **anecdotes locales historiques ou culturelles** trouvées en temps réel sur le Web grâce à l'outil intégré **`google_search`**.

### 7.1. Comprendre l'intérêt de la collaboration Multi-Agents

Pour contourner cette restriction de plateforme et modéliser une architecture robuste d'entreprise, nous allons concevoir une **équipe de micro-agents spécialisés** :
1. Un **Agent Expert SQL** (`db_specialist_agent`) qui possède *uniquement* les outils de base de données.
2. Un **Agent Chercheur Web** (`web_researcher_agent`) qui possède *uniquement* l'outil `google_search`.
3. Un **Agent Présentateur Coordinateur** (`presentateur_meteo_agent`) qui n'a aucun outil direct, mais possède nos deux spécialistes sous forme d'outils de fonctions (**`AgentTool`**).

Cette architecture avec `AgentTool` permet à l'agent coordinateur de garder le contrôle absolu : il appelle chaque spécialiste de manière séquentielle, récupère leurs résultats comme retour d'outil, et évite tout conflit de type d'outil (Grounding vs SQL) sur Vertex AI !

### 7.2. Refactoriser agent.py pour implémenter le flux Multi-Agents

Ouvrez le fichier `weather_agent_app/agent.py` et remplacez son contenu par le code d'orchestration multi-agents suivant :

```python
from google.adk.agents import Agent
from google.adk.tools import google_search, AgentTool
from toolbox_core import ToolboxSyncClient

# 1. Connexion au serveur MCP local
toolbox = ToolboxSyncClient("http://127.0.0.1:5000")
weather_tools = toolbox.load_toolset('weather_toolset')

# =====================================================================
# AGENT A : Le Spécialiste Base de Données PostgreSQL (SQL homogène)
# =====================================================================
db_agent = Agent(
    name="db_specialist_agent",
    model="gemini-2.5-flash",
    description=(
        "Expert pour interroger la base de données PostgreSQL : "
        "lire l'historique climatique ou enregistrer/mettre à jour des bulletins météo."
    ),
    instruction=(
        "Vous êtes un expert en base de données PostgreSQL. Votre rôle unique "
        "est d'utiliser vos outils SQL pour exécuter des lectures de relevés climatiques "
        "ou enregistrer/modifier des bulletins météo dans la table `weather_bulletins`."
    ),
    tools=weather_tools,
)

# =====================================================================
# AGENT B : Le Spécialiste Recherche Web (Grounding homogène)
# =====================================================================
research_agent = Agent(
    name="web_researcher_agent",
    model="gemini-2.5-flash",
    description=(
        "Expert pour chercher sur le Web en temps réel des anecdotes historiques, "
        "insolites ou culturelles sur une ville donnée."
    ),
    instruction=(
        "Vous êtes un chercheur d'anecdotes en ligne hors pair. Votre rôle unique "
        "est d'utiliser le moteur Google Search pour trouver une anecdote historique, "
        "culturelle ou insolite passionnante sur une ville donnée et de rédiger le texte final du bulletin."
    ),
    tools=[google_search],
    disallow_transfer_to_parent=True,
    disallow_transfer_to_peers=True,
)

# =====================================================================
# AGENT C : Le Coordinateur / Présentateur (Root Agent)
# =====================================================================
root_agent = Agent(
    name="presentateur_meteo_agent",
    model="gemini-2.5-flash",
    description="Le présentateur principal qui coordonne la rédaction et la publication des bulletins.",
    instruction=(
        "Vous êtes un présentateur météo extrêmement chaleureux et le chef d'orchestre de cet atelier. "
        "Votre but est d'aider l'utilisateur à concevoir et publier des bulletins météo uniques.\n\n"
        "Pour accomplir votre mission, vous devez appeler vos micro-agents spécialisés en tant qu'outils de la manière suivante :\n"
        "1. Appelez `db_specialist_agent` et demandez-lui de lire l'historique ou les prévisions météo de la ville (par exemple pour Paris au 26 mai 2026).\n"
        "2. Transmettez ces données météo à `web_researcher_agent` et demandez-lui de faire une recherche d'anecdote en ligne et de rédiger un magnifique bulletin complet.\n"
        "3. Renvoyez le bulletin rédigé final à `db_specialist_agent` pour qu'il l'enregistre de façon permanente dans la base de données PostgreSQL via l'outil `publish-weather-bulletin`.\n"
        "4. Faites une synthèse chaleureuse et structurée en Markdown à l'utilisateur de ce que vous venez d'accomplir."
    ),
    tools=[AgentTool(db_agent), AgentTool(research_agent)],  # Usage of AgentTool instead of sub_agents
)
```

### 7.3. Lancer et Tester l'Équipe d'Agents Météo

Relancez le serveur ADK (`adk web --allow_origins "*"` dans votre terminal) et ouvrez la console sur le port `8000`.

Saisissez maintenant des requêtes complexes d'orchestration et de publication :
* *"Rédige et publie un bulletin météo pour Paris à la date du 26 mai 2026 (température de 19°C et conditions ensoleillées). N'oublie pas de demander à ton chercheur d'anecdotes de trouver une super anecdote sur Paris sur le Web !"*

**Vérification et validation dans la base de données :**
Une fois que l'agent coordinateur vous a chaleureusement confirmé le succès, connectez-vous à votre console **Cloud SQL Studio** (sur la base `weather_db`) et exécutez la requête d'administration suivante :
```sql
SELECT * FROM weather_bulletins;
```
Vous verrez apparaître physiquement le bulletin météo rédigé par le LLM, enrichi de l'anecdote historique exacte trouvée en direct par l'agent web, et stocké de manière pérenne et sécurisée par l'agent SQL dans votre base PostgreSQL !

---

## 8. Défi : L'Agent DBA et Admin PostgreSQL (Non guidé)

Durée : 30:00 (À titre indicatif)

Pour aller plus loin et tester vos compétences acquises, vous allez concevoir un second agent autonome, cette fois-ci du côté **Administrateur de Base de Données (DBA)**. 

Cet agent n'aura plus pour rôle d'analyser la météo, mais d'aider un administrateur système à surveiller, diagnostiquer et optimiser l'instance PostgreSQL en interrogeant directement les vues système et le catalogue PostgreSQL.

### 8.1. Le but à atteindre
Créer un agent capable de répondre en langage naturel à des questions sur l'état de la base de données, ses performances et son intégrité (ex: tables les plus lourdes, index inutilisés, requêtes lentes actives).

---

### 8.2. Grandes lignes pour réussir le défi

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

### 8.3. Scénarios de test pour valider votre défi
Une fois l'agent fonctionnel, testez-le dans la console interactive en posant des questions comme :
1. *"Quelles sont les tables les plus volumineuses de ma base de données et combien d'espace les index consomment-ils ?"*
2. *"Y a-t-il des requêtes actives lentes ou bloquées en ce moment ?"*
3. *"Est-ce que mes tables utilisent correctement les index ou est-ce que je devrais en créer de nouveaux ?"*
4. *"Quelle est la version exacte de mon serveur PostgreSQL ?"*

> [!NOTE]
> **À vous de jouer !** Expérimentez en ajoutant d'autres requêtes système à votre fichier de configuration MCP Toolbox pour étendre les compétences cognitives de votre agent DBA.

---

## 9. Défi Ultime : RAG sémantique & Embeddings 100% natifs dans PostgreSQL (Non guidé - Niveau Expert)

> [!WARNING]
> **Zone de haute technicité :** Cette partie finale est entièrement non guidée. C'est à vous de concevoir l'architecture de base de données, d'activer les extensions appropriées, d'écrire le code PL/pgSQL nécessaire et de configurer les requêtes de recherche vectorielle.

### 9.1. Le But et le Scénario

Actuellement, votre agent météo effectue des recherches textuelles exactes (avec l'opérateur SQL `ILIKE`). Pour passer à une architecture moderne d'IA de niveau entreprise, nous voulons implémenter du **RAG (Retrieval-Augmented Generation)** avec une recherche sémantique.

Cependant, **nous voulons que la base de données PostgreSQL gère 100% de l'intelligence !** 
Plutôt que d'écrire du code Python complexe pour appeler des API d'embeddings à chaque insertion ou recherche, nous allons déléguer ces tâches de vectorisation directement au moteur de base de données PostgreSQL grâce aux extensions **`pgvector`** et **`google_ml_integration`** (Vertex AI).

---

### 9.2. Les Contraintes et Étapes de Réalisation

Pour réussir ce défi ultime, vous devez implémenter l'architecture suivante en totale autonomie :

1. **Activation des extensions d'IA dans PostgreSQL** :
   - Connectez-vous à votre base `weather_db` et activez les extensions `vector` (pour la gestion des vecteurs) et `google_ml_integration` (pour appeler Vertex AI directement depuis le SQL).
   - N'oubliez pas d'accorder les privilèges d'exécution sur ces extensions à votre utilisateur restreint `agent_user`.

2. **Stockage et calcul automatique des embeddings par Trigger (SQL)** :
   - Ajoutez une colonne `embedding` de type `vector(768)` (dimension du modèle de Vertex AI `text-embedding-005`) à votre table `weather_bulletins`.
   - Écrivez une **fonction de trigger PL/pgSQL** et associez-la à un trigger (`BEFORE INSERT OR UPDATE`) sur la table `weather_bulletins`. 
   - Ce trigger doit automatiquement appeler la fonction `google_ml.embedding('text-embedding-005', NEW.bulletin_text)` pour générer le vecteur d'embedding associé au texte du bulletin et le stocker dans la colonne `embedding`.
   - *Avantage :* Dès que l'agent ou un utilisateur écrit un bulletin via l'outil SQL classique, la base PostgreSQL génère et indexe sémantiquement la donnée en tâche de fond de façon transparente !

3. **Exposition de l'outil de recherche sémantique dans la Toolbox** :
   - Déclarez un nouvel outil dans votre fichier `tools.yaml` (par exemple `search-similar-bulletins`).
   - Cet outil doit accepter un unique paramètre de recherche textuel en langage naturel (ex. `query`).
   - La requête SQL doit appeler `google_ml.embedding` sur la requête de recherche textuelle, puis calculer la distance cosinus (`<=>`) avec les vecteurs stockés pour retourner les 3 bulletins les plus proches sémantiquement.

4. **Orchestration et RAG dans l'ADK** :
   - Connectez ce nouvel outil SQL à votre agent dans l'ADK.
   - Testez l'orchestration en lui demandant de faire de véritables synthèses sémantiques à partir de bulletins historiques existants.

---

### 9.3. Scénarios de Test pour Valider le Défi

Une fois implémenté, validez votre architecture grâce aux tests suivants :

1. **Test d'indexation automatique** :
   - Demandez à votre agent de rédiger et publier un bulletin contenant le texte suivant :
     *"Aujourd'hui à Strasbourg, le soleil brille de mille feux avec une chaleur étouffante de 32°C. Prenez vos précautions contre la canicule."*
   - Allez dans Cloud SQL Studio et vérifiez que la colonne `embedding` de ce nouveau bulletin a été **automatiquement calculée et peuplée** par votre trigger PostgreSQL.

2. **Test de recherche sémantique (RAG)** :
   - Posez à votre agent une question conceptuelle, par exemple :
     *"Recherche dans les anciens bulletins s'il y a des conseils pour se protéger de la chaleur ou des vagues de chaleur extremes."*
   - **Validation :** L'agent doit appeler votre outil de recherche sémantique, trouver le bulletin de Strasbourg (grâce à la proximité sémantique entre "vagues de chaleur" et "canicule"), récupérer le texte exact, et vous fournir une réponse synthétisée chaleureuse.

---

## 10. Félicitations !

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
