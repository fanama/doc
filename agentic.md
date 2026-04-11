# 🤖 Agentic Framework

**Agentic** est un orchestrateur d'IA de nouvelle génération. Il combine la puissance des graphes d'états de **LangGraph** avec la flexibilité du **Model Context Protocol (MCP)** pour créer des agents autonomes capables d'interagir avec des bases de données, des systèmes de fichiers et des outils de calcul complexes.

---

## 🏗️ 1. Architecture du Système

Le framework repose sur une architecture découplée en trois couches : la **Perception** (UI), le **Raisonnement** (LangGraph + LLM) et l'**Action** (MCP Servers).

### Schéma de Classes


```mermaid
classDiagram
    direction TB

    class UserInterface {
        <<Interface>>
        +UserInput (Natural Language)
        +displayResponse(Natural Language)
    }

    class Orchestrator {
        <<LangGraph Manager>>
        +State state
        +GraphConfig config
        +step() 
        -updateState(key, value)
    }

    class LLM_Brain {
        <<Reasoning Engine>>
        +process(Context)
        -thought_process : ChainOfThought
        -parse_intent()
        -format_tool_args()
    }

    class MCPServer {
        <<MCP Host>>
        +String endpoint
        +list_tools()
        +execute_call(tool_name, params)
    }

    class MCPTool {
        <<Capability>>
        +String name
        +JSONSchema definition
        +validate(args)
    }

    class StateStore {
        <<Memory>>
        +History messages
        +ToolOutputs results
    }

    %% Flux Logique
    UserInterface --> Orchestrator : Dispatch Input
    Orchestrator ..> StateStore : Persist Context
    Orchestrator --> LLM_Brain : Request Next Action
    
    LLM_Brain --|> Orchestrator : Tool Call / Final Response
    
    Orchestrator "1" o-- "many" MCPServer : Registry
    MCPServer "1" *-- "many" MCPTool : Provides
    
    MCPTool ..> Orchestrator : Return Observation
    Orchestrator --> UserInterface : Final Synthesis
```


---

## 🔄 2. Flux de Travail & Expérience Utilisateur

Contrairement aux chatbots linéaires, Agentic utilise une **boucle de rétroaction active**. L'agent n'arrête pas son exécution tant que l'objectif n'est pas atteint ou qu'une limite d'itérations n'est pas rencontrée.

### Parcours de Données (User Journey)

Extrait de code

```mermaid
journey
    title Cycle de vie d'une requête Agentic
    section Initialisation
      Lancement de l'interface: 5: Utilisateur
      Chargement des serveurs MCP: 4: Système
    section Raisonnement
      Saisie requête naturelle: 5: Utilisateur
      Parsing sémantique & intention: 4: LLM_Brain
    section Action
      Appel dynamique d'outil: 5: Orchestrateur
      Exécution (DB, Math, Search): 4: MCPServer
    section Clôture
      Synthèse multi-sources: 5: LLM_Brain
      Affichage de la solution: 5: UserInterface
```

---




## 🧩 3. Composants Clés

|**Composant**|**Rôle**|**Emplacement**|
|---|---|---|
|**Orchestrateur**|Gestionnaire d'état (`State`) et routage logique.|`graph/langraph.py`|
|**Serveurs MCP**|Micro-services exposant des capacités spécifiques.|`mcp/*.py`|
|**Tools**|Fonctions atomiques avec schémas JSON pour le LLM.|`tools/`|
|**UI Layer**|Points d'entrée (Terminal, Web, Desktop).|`ui/`, `main.py`, `webapp.py`|

### Focus sur les Serveurs MCP

- **`database_server.py`** : Interface d'accès aux données.
    
- **`computer_server.py`** : Automatisation système et fichiers.
    
- **`math_server.py`** : Moteur de calcul déterministe.
    
- **`initialisation.py`** : Registre central des capacités de l'agent.
    

---

## 🛠️ 4. Guide de Développement (uv)

Nous utilisons **uv** pour garantir des performances de build 10x supérieures à pip et une reproductibilité totale.

### Installation Rapide

Bash

```
# Synchronisation de l'environnement et des dépendances
uv sync
```

### Ajouter une Capacité (New Tool)

1. **Création** : Créez `mcp/mon_outil.py`.
    
2. **Documentation** : Utilisez des _docstrings_ Python rigoureuses. Le LLM les utilise pour comprendre **quand** appeler votre outil.
    
3. **Enregistrement** : Déclarez votre serveur dans `mcp/initialisation.py`.
    

### Validation

Toujours valider la chaîne de liaison avant de déployer :

Bash

```
uv run python test.py
```

---

## 🖥️ 5. Interfaces Disponibles

Le projet est agnostique à l'interface. Choisissez celle adaptée à votre workflow :

- **Mode Commando** : `uv run python main.py` (CLI rapide)
    
- **Mode Analytics** : `uv run streamlit run webapp.py` (Web UI avec graphs)
    
- **Mode Standalone** : `uv run python desktop.py` (Application fenêtrée)
    

---

> **Note :** Le système de parsing est hybride. La **sémantique** est gérée par le LLM (interprétation), tandis que la **syntaxe** est validée par le schéma MCP (typage strict).
