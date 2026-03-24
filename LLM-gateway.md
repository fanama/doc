**Version :** 1.0

**Objectif :** Fournir une interface unifiée pour interagir avec divers LLM tout en gérant l'isolation par projet, l'authentification par clé API et l'injection de prompts système.

---

## 1. Architecture Système

L'application repose sur une architecture en couches (Layered Architecture) pour séparer la logique de routage de la logique métier (gestion des prompts) et de l'intégration des fournisseurs.

## Composants Principaux

- **API Gateway (Entry Point) :** Gère les requêtes entrantes, la terminaison TLS et le rate-limiting global.
    
- **Auth & Context Provider (Middleware) :** Valide la clé API, identifie le projet associé et charge les configurations (System Prompt).
    
- **Provider Engine (Adapter Pattern) :** Traduit la requête unifiée vers le format spécifique du fournisseur (OpenAI, Anthropic, Google, etc.).
    
- **Observability Layer :** Enregistre les métriques (tokens, latence, coûts) de manière asynchrone pour ne pas ralentir la réponse.
    

---

## 2. Modèle de Données (Schéma ERD)

Le cœur de votre logique repose sur la hiérarchie suivante :

```mermaid
classDiagram
    class Organization {
        +UUID id
        +String name
        +DateTime created_at
        +getProjects()
    }

    class Project {
        +UUID id
        +UUID organization_id
        +String name
        +Boolean is_active
        +getConfigs()
    }

    class API_Key {
        +UUID id
        +UUID project_id
        +String key_hash
        +String label
        +DateTime last_used_at
        +isValid()
    }

    class System_Prompt {
        +UUID id
        +UUID project_id
        +String content
        +Integer version
        +Boolean is_active
        +deploy()
    }
    
    class Model {
        +UUID id
        +UUID project_id
        +String name
    }

    class Usage_Log {
        +UUID id
        +UUID project_id
        +UUID model_id
        +Integer input_tokens
        +Integer output_tokens
        +Float cost
        +DateTime timestamp
    }

    Organization "1" *-- "many" Project : possède
    Project "1" *-- "many" API_Key : gère
    Project "1" *-- "many" System_Prompt : contient
    Project "1" *-- "many" Model : contient
    Project "1" o-- "many" Usage_Log : génère
```

---

## 3. Spécifications du Flux de Requête

Voici le cycle de vie d'une requête dans votre Gateway :

```mermaid
sequenceDiagram
    autonumber
    participant C as Client (App Externe)
    participant G as Gateway (Go/Node)
    participant Cache as Redis (Context)
    participant DB as Postgres (Source of Truth)
    participant O as Ollama (GPU Instance)

    C->>G: POST /v1/chat (Bearer sk_live_...)
    
    Note over G: Hashage de la clé (SHA-256)
    
    G->>Cache: Get Project Context (key_hash)
    
    alt Cache Miss
        Cache-->>G: null
        G->>DB: Lookup Project + Active Prompt
        DB-->>G: {proj_id, system_prompt, model_tag}
        G->>Cache: Set Context (TTL 1h)
    else Cache Hit
        Cache-->>G: {proj_id, system_prompt, model_tag}
    end

    rect rgb(230, 245, 255)
        Note over G: Pre-processing
        G->>G: Validation du quota (Rate Limit)
        G->>G: Injection System Prompt (Index 0)
        G->>G: Formatage JSON pour Ollama API
    end

    G->>O: Forward Request (/api/chat)
    
    par Stream Response
        O-->>G: SSE Chunks (Partial Content)
        G-->>C: Stream vers le client
    and Async Analytics
        Note over G: Post-processing (Background)
        G->>DB: Log Usage (tokens, duration_ms, model)
    end
```

---

## 4. Stratégie de Tests

Pour garantir la stabilité, le projet doit suivre la pyramide des tests :

### Tests Unitaires (Focus : Logique interne)

- **Parser de Prompt :** Vérifier que l'injection du prompt système ne corrompt pas la structure JSON.
    
- **Générateur de Hash :** S'assurer que les clés API sont correctement hashées avant stockage.
    

### Tests d'Intégration (Focus : Communication)

- **Mock Providers :** Simuler une réponse 429 (Too Many Requests) d'OpenAI et vérifier que la Gateway bascule bien sur le fournisseur de backup (Fallback).
    
- **Persistence :** Vérifier qu'une clé supprimée en base de données rejette immédiatement les appels.
    

### Tests de Performance

- **Latence d'overhead :** La Gateway ne doit pas ajouter plus de **20-50ms** à la requête originale.
    

---

## 5. Sécurité et Performances

- **Sécurité des Clés :** Ne stockez jamais les clés API de vos projets en clair. Utilisez un hash (type SHA-256) pour la comparaison. Chaque clef doit avoir une durée de vie.
    
- **Caching :** Utilisez **Redis** pour mettre en cache les associations `Clé API -> Projet -> System Prompt` afin d'éviter une requête SQL à chaque appel.
    
- **Timeout :** Configurez des timeouts stricts (ex: 30s) pour éviter que des requêtes LLM suspendues ne bloquent vos ressources serveur.
    
## Schéma global

```mermaid
classDiagram

%% --- Couche Infrastructure / Gateway ---

class APIGateway {

-rateLimiter: RateLimiter

-authMiddleware: AuthMiddleware

+handleRequest(req: Request): Response

-routeToProvider(payload: UnifiedPayload): Stream

}

  

class AuthMiddleware {

-cache: ICacheProvider

-db: IDatabase

+validateKey(apiKey: String): ProjectContext

-hashKey(key: String): String

}

  

%% --- Couche Coeur Métier (Domaine) ---

class ProjectContext {

+projectId: UUID

+activePrompt: SystemPrompt

+selectedModel: ModelConfig

+quotas: QuotaLimits

}

  

class PromptManager {

+injectSystemPrompt(messages: List, prompt: String): List

+formatTemplate(content: String, vars: Map): String

}

  

%% --- Pattern Adapter pour les LLM ---

class ILLMProvider {

<<interface>>

+chat(payload: UnifiedRequest): Stream

+embeddings(text: String): List~float~

+mapResponse(rawResponse: Any): UnifiedResponse

}

  

class OpenAIProvider {

-apiKey: String

+chat(payload: UnifiedRequest): Stream

}

  

class AnthropicProvider {

-apiKey: String

+chat(payload: UnifiedRequest): Stream

}

  

class OllamaProvider {

-endpoint: String

+chat(payload: UnifiedRequest): Stream

}

  

%% --- Couche Observabilité ---

class MetricsCollector {

-buffer: List~UsageLog~

+logUsage(metrics: UsageMetrics)

+flushAsync()

}

  

%% --- Modèles de Données (Entités) ---

class Organization {

+id: UUID

+name: String

+projects: List~Project~

}

  

class Project {

+id: UUID

+name: String

+apiKeys: List~APIKey~

+systemPrompts: List~SystemPrompt~

}

  

class SystemPrompt {

+id: UUID

+content: String

+version: int

+isActive: boolean

}

  

%% --- Relations ---

APIGateway --> AuthMiddleware : utilise

AuthMiddleware ..> ProjectContext : crée

APIGateway --> PromptManager : utilise pour pre-processing

APIGateway --> ILLMProvider : délégue via Factory

ILLMProvider <|.. OpenAIProvider : implémente

ILLMProvider <|.. AnthropicProvider : implémente

ILLMProvider <|.. OllamaProvider : implémente

  

ProjectContext o-- SystemPrompt : contient

Project "1" *-- "many" SystemPrompt : possède

Organization "1" *-- "many" Project : possède

APIGateway --> MetricsCollector : notifie en asynchrone
```
