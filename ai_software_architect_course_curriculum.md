# AI Software Architect Master Course
**Course Title:** Enterprise AI Systems Engineering & Architecture  
**Level:** Advanced / Executive Architect  
**Prerequisites:** Distributed Systems, System Design, Machine Learning Fundamentals, Python/C++  

---

## Course Overview

This course provides a complete, production-ready curriculum for software architects designed to build, scale, secure, and operate enterprise Large Language Model (LLM) applications and distributed AI infrastructure.

---

# Module 1: High-Performance Vector Search & Indexing Strategy

### 1.1 Deep Dive: Vector Indexing Architectures

Modern vector databases must perform Approximate Nearest Neighbor (ANN) search over multi-million or billion-scale embeddings while balancing latency, QPS, RAM overhead, and recall.

#### Graph-Based Indexes: HNSW (Hierarchical Navigable Small World)
HNSW constructs a multi-layer graph where the top layers feature long-range links (fast navigation) and the bottom layer contains short-range links for fine-grained search.

*   **Time Complexity:** $\mathcal{O}(\log N)$ search time.
*   **Key Parameters:**
    *   `M`: Maximum number of bi-directional outgoing links per node in the graph ($16 \le M \le 64$).
    *   `efConstruction`: Depth of candidate evaluation during index building.
    *   `efSearch`: Size of the dynamic candidate list during query time.

$$\text{Recall} \propto \text{efSearch}, \quad \text{Latency} \propto \text{efSearch} \times M$$

```
Layer 2: [ Node A ] -----------------------------> [ Node Z ]   (Long jump)
              |                                         |
Layer 1: [ Node A ] --------> [ Node M ] --------> [ Node Z ]   (Medium jump)
              |                   |                     |
Layer 0: [ Node A ]->[ Node C ]->[ Node M ]->[ Node P ]->[ Node Z ] (Dense graph)
```

#### Vector Compression: Product Quantization (PQ)
High-dimensional vectors (e.g., 1536 dimensions from OpenAI `text-embedding-3-large`) consume significant RAM. PQ compresses vectors by breaking a $D$-dimensional vector into $m$ sub-vectors of dimension $d' = D/m$, running k-means clustering on each sub-space to produce codebooks.

$$\vec{x} = [x_1, x_2, \dots, x_D] \xrightarrow{\text{Split}} [\vec{u}_1, \vec{u}_2, \dots, \vec{u}_m]$$

*   **Compression Ratio:** Reduces float32 vectors ($4 \text{ bytes/dim}$) to 1 byte per sub-vector index, yielding up to **95% memory savings**.
*   **Trade-off:** Lossy compression; degrades recall accuracy slightly compared to uncompressed distance metrics (L2, Inner Product, Cosine).

---

### 1.2 Comparison Matrix: Vector Index Algorithms

| Index Strategy | Query Latency | QPS Capacity | RAM Usage | Recall Accuracy | Dynamic Updates |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **FLAT (Exhaustive)** | Very High ($\mathcal{O}(N)$) | Very Low | $100\%$ (Raw) | $100\%$ | Instant |
| **IVF (Inverted File)** | Medium ($\mathcal{O}(\sqrt{N})$) | Medium | $100\%$ | High ($90-95\%$) | Fast |
| **HNSW** | Low ($\mathcal{O}(\log N)$) | High | $150-200\%$ (Graph) | Very High ($98\%+$) | Moderate |
| **HNSW + PQ** | Low-Medium | Very High | $10-20\%$ | Good ($88-95\%$) | Complex |
| **LSH (Locality-Sensitive)** | Medium | High | Low | Low-Medium ($70-85\%$) | Instant |

---

### 1.3 Production Example: Enterprise Hybrid Search with RRF

To optimize recall across both exact keyword terms (e.g., part numbers, SKU codes) and semantic intent, enterprise applications combine sparse (BM25) and dense (Vector) retrieval using **Reciprocal Rank Fusion (RRF)**.

$$RRF\_Score(d \in D) = \sum_{m \in M} \frac{1}{k + r_m(d)}$$

Where $k$ is a constant smoothing factor (typically $k=60$), $M$ is the set of retrievers, and $r_m(d)$ is the rank position of document $d$ in retriever $m$.

```python
import numpy as np
from typing import List, Dict, Any

def reciprocal_rank_fusion(
    dense_rankings: List[str], 
    sparse_rankings: List[str], 
    k: int = 60
) -> List[Dict[str, Any]]:
    """
    Combines dense and sparse search rankings using Reciprocal Rank Fusion (RRF).
    """
    scores: Dict[str, float] = {}

    def add_rankings(rankings: List[str]):
        for rank, doc_id in enumerate(rankings):
            if doc_id not in scores:
                scores[doc_id] = 0.0
            scores[doc_id] += 1.0 / (k + (rank + 1))

    add_rankings(dense_rankings)
    add_rankings(sparse_rankings)

    # Sort documents by combined RRF score in descending order
    reranked = sorted(scores.items(), key=lambda x: x[1], reverse=True)
    return [{"doc_id": doc_id, "rrf_score": score} for doc_id, score in reranked]

# Example execution
dense_hits = ["doc_A", "doc_B", "doc_C", "doc_D"]
sparse_hits = ["doc_B", "doc_D", "doc_E", "doc_A"]

fused_results = reciprocal_rank_fusion(dense_hits, sparse_hits)
print("RRF Combined Results:", fused_results)
```

---

# Module 2: Distributed LLM Inference & Cluster Scaling

### 2.1 Physics of LLM Inference: Prefill vs. Decode Phase

LLM inference operates in two distinct mathematical phases with drastically different hardware resource bottlenecks:

```
+-------------------------------------------------------------------+
|                        PREFILL PHASE                              |
| - Compute-Bound: Matrix multiplication across all input tokens   |
| - High GPU Tensor Core utilization                                |
| - Primary Metric: Time to First Token (TTFT)                     |
+-------------------------------------------------------------------+
                                  |
                                  v
+-------------------------------------------------------------------+
|                         DECODE PHASE                              |
| - Memory-Bandwidth Bound: Generates 1 token per forward pass      |
| - Low Compute Intensity, High GPU VRAM Memory Read demands       |
| - Primary Metric: Inter-Token Latency (Time Between Tokens - TBT) |
+-------------------------------------------------------------------+
```

#### Memory Math: KV Cache Size Calculation
The Key-Value (KV) cache stores past token attention states to prevent re-computing hidden layers during the auto-regressive decode phase.

$$\text{Memory}_{\text{KVCache}} = 2 \times b \times s \times l \times h \times d_{\text{bytes}}$$

Where:
*   $b = \text{Batch Size}$
*   $s = \text{Sequence Length (Tokens)}$
*   $l = \text{Number of Transformer Layers}$
*   $h = \text{Number of Attention Heads}$
*   $d_{\text{bytes}} = \text{Bytes per element}$ (2 bytes for FP16/BF16)

*Example:* For a 70B parameter model ($l=80, h=64$, head dimension $= 128$) with batch size 32 at context length 8,192 tokens in FP16:
$$\text{KV Cache} = 2 \times 32 \times 8192 \times 80 \times (64 \times 128) \times 2 \text{ bytes} \approx 549.75 \text{ GB}$$

---

### 2.2 Model Parallel Strategies: Tensor vs. Pipeline Parallelism

When model weights and KV caches exceed the VRAM of a single GPU (e.g., NVIDIA H100 80GB), model weights must be partitioned across devices.

```
Tensor Parallelism (TP) - Intra-Node (NVLink ~900 GB/s)
+--------------------------------------------------------+
| GPU 0: Layer 1 (Matrix Split A)  <-- AllReduce -->      |
| GPU 1: Layer 1 (Matrix Split B)  <-- AllReduce -->      |
+--------------------------------------------------------+

Pipeline Parallelism (PP) - Inter-Node (PCIe / InfiniBand)
+-------------------+      +-------------------+
| GPU Node 0        | ---> | GPU Node 1        |
| Layers 1 to 32    |      | Layers 33 to 64   |
+-------------------+      +-------------------+
```

#### Tensor Parallelism (TP)
*   **Mechanism:** Splits individual linear weight matrices (e.g., $Q, K, V$ projections and Multi-Layer Perceptron layers) *column-wise* or *row-wise* across GPUs.
*   **Communication:** Requires high-frequency `AllReduce` operations at **every transformer layer**.
*   **Interconnect:** Requires ultra-high bandwidth intra-node links (Nvidia NVLink / NVSwitch).

#### Pipeline Parallelism (PP)
*   **Mechanism:** Group model layers sequentially across different physical devices (e.g., Layers 1–32 on Node 1, Layers 33–64 on Node 2).
*   **Communication:** Point-to-point activations transfer between layer boundaries.
*   **Trade-off:** Introduces "Pipeline Bubbles" (idle GPU waiting cycles), mitigated via 1F1B (One Forward, One Backward) scheduling algorithms.

---

### 2.3 Memory Management: PagedAttention & Continuous Batching

Standard inference frameworks suffer from internal memory fragmentation due to pre-allocating contiguous memory blocks for max context lengths. **vLLM with PagedAttention** solves this by partitioning the KV cache into fixed-size physical memory pages, mapped dynamically via a logical page table.

```
Logical KV Memory (Virtual):    [ Page 0 ] [ Page 1 ] [ Page 2 ]
                                    |          |          |
                                    v          v          v
Physical GPU VRAM (Non-contiguous): [ Block 7 ] [ Block 2 ] [ Block 19 ]
```

*   **Continuous Batching (Iteration-level Scheduling):** Instead of waiting for an entire batch to complete generation, new requests enter the execution batch immediately at the next token generation step, while completed requests vacate their physical pages dynamically.

---

# Module 3: Security, Guardrails & Governance Architectures

### 3.1 Security Threat Topology

LLM applications introduce new attack surfaces defined by OWASP Top 10 for LLMs:
1.  **Direct Prompt Injection:** Overriding system prompts via malicious user input.
2.  **Indirect Prompt Injection:** Hiding malicious commands inside retrieved unstructured documents (PDFs, websites, raw DB fields).
3.  **Data Poisoning:** Corrupting training or embedding datasets.
4.  **SOP & SSRF via Rogue Tool Execution:** Inducing an LLM agent to execute unauthorized internal API commands.

---

### 3.2 Dual-LLM Security Pattern

To isolate execution environments from untrusted context, the **Dual-LLM Architecture** separates processing into two distinct risk zones:

```
User Query ---> [ API Gateway / Edge ]
                       |
                       v
            [ Privilege Level 0: Execution Agent ]
            (NO access to unvalidated retrieved data)
                       |
                       +---> Invokes Retriever
                                   |
                                   v
            [ Untrusted Data Source (DB/Web) ]
                                   |
                                   v
            [ Privilege Level 1: Data Reader LLM ]
            (Extracts raw facts into strict JSON schema)
                                   |
                                   v
            [ Input/Output Guardrail Proxy ]
            (Validates JSON schema & checks for injections)
                                   |
                                   v
            [ Privilege Level 0: Execution Agent ] ---> [ Safe Response ]
```

---

### 3.3 Production Example: Enterprise PII Redaction Proxy

To ensure GDPR/HIPAA compliance, all prompts routed to commercial LLM providers or long-term observability logs must pass through an inline sanitization proxy.

```python
import re
from typing import Dict, Tuple

class PIISanitizationProxy:
    def __init__(self):
        # Regex patterns for demonstration (Use specialized NER models in production)
        self.patterns = {
            "EMAIL": r'[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+',
            "PHONE": r'\b\d{3}[-.]?\d{3}[-.]?\d{4}\b',
            "SSN": r'\b\d{3}-\d{2}-\d{4}\b'
        }

    def sanitize_prompt(self, prompt: str) -> Tuple[str, Dict[str, str]]:
        """
        Redacts sensitive PII terms, replacing them with unique session tokens.
        """
        redaction_map = {}
        sanitized_prompt = prompt

        token_counter = 0
        for pii_type, regex in self.patterns.items():
            matches = re.findall(regex, sanitized_prompt)
            for match in matches:
                token = f"<REDACTED_{pii_type}_{token_counter}>"
                redaction_map[token] = match
                sanitized_prompt = sanitized_prompt.replace(match, token, 1)
                token_counter += 1

        return sanitized_prompt, redaction_map

    def restore_response(self, llm_response: str, redaction_map: Dict[str, str]) -> str:
        """
        Restores redacted tokens back into the client-facing response payload.
        """
        restored_response = llm_response
        for token, original_value in redaction_map.items():
            restored_response = restored_response.replace(token, original_value)
        return restored_response

# Execution Example
proxy = PIISanitizationProxy()
user_prompt = "Please send financial updates to john.doe@company.com or call 555-123-4567."

clean_prompt, token_mapping = proxy.sanitize_prompt(user_prompt)
print("Sanitized Prompt for LLM:", clean_prompt)
print("Active Token Mapping:", token_mapping)

# Simulated LLM processing
mock_llm_out = f"Acknowledged. I will update {clean_prompt.split()[-1]} and send email to {clean_prompt.split()[5]}."
final_user_output = proxy.restore_response(mock_llm_out, token_mapping)
print("Restored Output for Client:", final_user_output)
```

---

# Module 4: Advanced Context Optimization & RAG Pipelines

### 4.1 Resolving the "Lost in the Middle" Effect

LLMs exhibit a U-shaped recall degradation curve: information placed near the **very beginning** or **very end** of a long context window is retrieved with high fidelity, whereas information embedded in the **middle 60%** suffers severe accuracy drops.

```
Recall Accuracy (%)
100% |  \                                   /
     |   \                                 /
 50% |    \       "Lost in Middle"        /
     |     \_____________________________/
  0% +---------------------------------------
     0% (Start)     Context Window     100% (End)
```

#### Remediation Strategy: Two-Stage Retrieval with Cross-Encoder Reranking
1.  **Stage 1 (Bi-Encoder Retrieval):** Retrieve top-$K$ ($K=50$) candidate chunks rapidly using fast vector distance (Cosine/Inner Product).
2.  **Stage 2 (Cross-Encoder Reranking):** Pass `(Query, Chunk)` pairs into a full cross-attention transformer model to compute deep interaction scores.
3.  **Context Construction:** Select top-$N$ ($N=5$) reranked chunks and place the highest scoring chunks at the absolute start and end of the prompt context.

---

### 4.2 Fine-Tuning vs. RAG: Architectural Decision Framework

Architects must evaluate whether to use Fine-Tuning (PEFT/LoRA) or RAG based on system requirements:

```
               Dynamic / Changing Data?
                     /          \
                   YES           NO
                   /              \
            [ Implement RAG ]     Is the goal changing Style, Format, 
                                  or Structural Syntax?
                                     /          \
                                   YES           NO
                                   /              \
                        [ Fine-Tune Model ]   [ Prompt Engineering ]
```

| Dimension | Retrieval-Augmented Generation (RAG) | Fine-Tuning (LoRA / SFT) |
| :--- | :--- | :--- |
| **Primary Purpose** | Injecting dynamic external factual knowledge | Teaching style, specialized format, and syntax rules |
| **Knowledge Recency** | Real-time / Up to the second | Static as of training execution time |
| **Hallucination Control**| High (Verifiable ground-truth citations) | Moderate to Low (Implicit parametric weights) |
| **Serving Cost** | Higher latency per request (large context payload) | Lower per-request context cost; higher GPU compute pre-training |
| **Domain Adaptation** | Dynamic document updates via database CDC | Embeds specialized language tasks or DSL grammars |

---

# Module 5: Agentic Frameworks, Data Streams & Caching

### 5.1 ReAct Agent Loop with Self-Reflection

An **Agentic System** extends basic LLM pipelines by adding autonomous decision loops combining Reasoning, Acting (Tool Call Execution), and Observation (Environment Feedback).

```
   +-----------------------------------------------------------+
   |                       User Query                          |
   +-----------------------------------------------------------+
                                 |
                                 v
   +-----------------------------------------------------------+
|--> 1. THOUGHT: Reason about state & determine tool needed   |
|  +-----------------------------------------------------------+
|                                |
|                                v
|  +-----------------------------------------------------------+
|  | 2. ACTION: Call External Tool API with Arguments           |
|  +-----------------------------------------------------------+
|                                |
|                                v
|  +-----------------------------------------------------------+
|  | 3. OBSERVATION: Parse execution output / catch errors     |
|  +-----------------------------------------------------------+
|                                |
|        Success or Fail?        |
|        /              \        |
|     FAILURE          SUCCESS   |
|       /                \       |
| [Append Error Context]  [Final Answer] ----------------------> Output
+-------+
```

---

### 5.2 Real-Time Data Pipelines: Streaming CDC to Vector Stores

To keep vector indexes in sync with enterprise relational databases without full table scans, implement **Change Data Capture (CDC)** event streaming pipelines.

```
[ PostgreSQL DB ] ---> [ Debezium CDC Connector ] ---> [ Apache Kafka Topic ]
                                                               |
                                                               v
                                                    [ Flink / Spark Streaming ]
                                                    (Text Chunking & Embedding)
                                                               |
                                                               v
                                                    [ Vector Index (HNSW) ]
```

---

### 5.3 Semantic Caching Architecture

Exact-match key-value caches (like Redis) fail when queries vary slightly in wording (e.g., *"How do I reset my password?"* vs *"Password reset instructions"*). **Semantic Caching** evaluates query similarity against pre-stored query vector embeddings.

#### Threshold Logic
1.  Generate query vector $\vec{q}$ for input query $Q$.
2.  Search cache index for nearest neighbor query vector $\vec{c}_i$ with distance score $S$.
3.  If $S \ge \text{Threshold}_{\text{exact}}$ (e.g., $0.96$): Return cached response immediately (**Cache Hit - Low Latency**).
4.  If $S < \text{Threshold}_{\text{exact}}$: Route request to LLM, obtain response, and insert tuple $(\vec{q}, Q, \text{Response})$ into cache index (**Cache Miss**).

---

# Module 6: End-to-End Enterprise Case Study & Architecture Blueprint

### 6.1 Enterprise Financial Assistant System Architecture

This production blueprint demonstrates a complete, highly available, secure AI architecture integrating all course modules:

```
                                  [ CLIENT USER ]
                                         |
                                         v
                            [ Enterprise API Gateway ]
                            (Rate Limiting / OAuth2)
                                         |
                                         v
                         [ PIISanitizationProxy Middleware ]
                                         |
                                         v
                            [ Semantic Caching Gateway ]
                                  /              \
                       (Cache Hit)                (Cache Miss)
                           /                            \
              [ Return Cache Payload ]           [ Dual-LLM Guardrail Proxy ]
                                                         |
                                                         v
                                              [ Agent Reasoning Engine ]
                                              (Orchestration / ReAct Loop)
                                                /                  \
                                               /                    \
                     [ Stage 1 Retrieval: Vector DB ]        [ Tool Execution ]
                     (HNSW + PQ Index / Hybrid RRF)          (Internal REST APIs)
                                       |                             |
                                       v                             v
                     [ Stage 2: Cross-Encoder Rerank ]     [ Output Validation ]
                                       |                             |
                                       +--------------+--------------+
                                                      |
                                                      v
                                        [ vLLM Cluster Serving Model ]
                                        (Tensor Parallelism / PagedAttention)
                                                      |
                                                      v
                                           [ Sanitized Response ]
```

---

## Course Review & Mastery Verification

To verify complete mastery of this curriculum, ensure you can independently design, code, and evaluate:
1.  Mathematical sizing of GPU VRAM capacity for mixed precision inference workloads.
2.  Vector index selection trade-offs between memory footprint, recall accuracy, and QPS requirements.
3.  Multi-layer guardrail networks preventing prompt injection and data exfiltration.
4.  Two-stage retrieval pipelines combining dense/sparse search with re-ranking models.