# 🧠 Semaine 1 — Fondations IA & Workflows

> **Objectif** : Comprendre les briques essentielles d'une solution IA (LLM, RAG, Agents, Orchestration) et être capable de les expliquer à un client.
> **Format** : 1h/jour — chaque jour contient une partie **Comprendre** (obligatoire) et une partie **🔧 Démo** (optionnelle, pratique).

---

## Jour 1 — Cartographie du paysage IA

### Comprendre (30 min)

#### 1. Les 4 briques d'une solution IA

Toute solution IA que tu livreras chez un client repose sur **4 briques fondamentales**. Pense à elles comme les étages d'un immeuble :

```
┌─────────────────────────────────┐
│   ORCHESTRATION                 │  ← Le chef d'orchestre (n8n, Make...)
│   Connecte tout, gère le flux   │
├─────────────────────────────────┤
│   AGENTS                        │  ← L'autonomie (le LLM qui décide seul)
│   Décident, utilisent des outils│
├─────────────────────────────────┤
│   RAG                           │  ← La mémoire (documents, données)
│   Donne du contexte au LLM      │
├─────────────────────────────────┤
│   LLM                           │  ← Le cerveau (GPT, Claude, Mistral)
│   Comprend et génère du texte   │
└─────────────────────────────────┘
```

**Détail de chaque brique :**

| Brique | C'est quoi ? | Analogie simple | Exemples d'outils |
|--------|-------------|-----------------|-------------------|
| **LLM** | Un modèle entraîné sur des milliards de textes, capable de comprendre et produire du langage | Un stagiaire ultra-cultivé mais qui ne connaît pas TON entreprise | GPT-4o, Claude, Mistral, Llama |
| **RAG** | Un système qui va chercher des infos pertinentes dans TES documents et les donne au LLM | Tu donnes au stagiaire un dossier à lire avant de répondre | Vector DBs + Embeddings |
| **Agents** | Un LLM qui peut décider d'utiliser des outils (chercher sur le web, interroger une BDD, envoyer un email) | Le stagiaire qui a accès au téléphone, à l'imprimante et au CRM | LangGraph, CrewAI, n8n AI Agent |
| **Orchestration** | Le système qui connecte toutes ces briques entre elles et avec le reste du SI client | Le manager qui dit "d'abord fais ça, puis ça, puis envoie le résultat là" | n8n, Make, Power Automate |

#### 2. Pourquoi ces 4 briques ?

Un LLM seul (ChatGPT) est **générique**. Un client veut une solution qui :
- ✅ Connaît SES données → **RAG**
- ✅ Peut AGIR (pas juste parler) → **Agents**
- ✅ S'intègre dans SON système → **Orchestration**

**Exemple concret** : Un client veut un assistant interne pour ses commerciaux.
1. Le commercial pose une question dans Teams
2. **L'orchestrateur** (n8n) reçoit le message
3. Le **RAG** cherche dans les fiches produits et le CRM
4. **L'agent** décide s'il faut aussi vérifier le stock (appel API ERP)
5. Le **LLM** formule une réponse claire
6. **L'orchestrateur** renvoie la réponse dans Teams

#### 3. Où tu te situes aujourd'hui

Tu connais déjà **n8n** = tu maîtrises la brique **Orchestration**. C'est un énorme avantage. Le programme va maintenant t'apprendre les 3 autres briques.

### Comprendre — suite (30 min)

#### 4. Les types de solutions IA chez les clients

En consulting IA, tu rencontreras principalement **5 types de projets** :

| Type de projet | Description | Briques principales |
|---------------|-------------|-------------------|
| **Chatbot interne** | Assistant qui répond aux questions des employés sur les docs internes | RAG + LLM + Orchestration |
| **Automatisation intelligente** | Workflows qui utilisent l'IA pour traiter des emails, factures, etc. | LLM + Orchestration |
| **Agent autonome** | IA qui effectue des tâches complexes (recherche, analyse, actions) | Agent + LLM + Orchestration |
| **Analyse de données** | Extraire des insights de données non structurées | LLM + RAG |
| **Génération de contenu** | Produire des rapports, emails, documentation automatiquement | LLM + Orchestration |

#### 5. Le vocabulaire essentiel

Avant d'aller plus loin, assure-toi de comprendre ces termes :

- **Token** : l'unité de texte pour un LLM (~0.75 mot en français). C'est ce qui est facturé.
- **Prompt** : l'instruction que tu donnes au LLM.
- **System prompt** : les instructions "cachées" qui définissent le comportement du LLM (ton rôle, règles, format).
- **Context window** : la quantité max de texte qu'un LLM peut "voir" en une fois (ex: 128K tokens pour GPT-4o).
- **Embedding** : une représentation numérique (vecteur) d'un texte — permet de comparer des textes par similarité.
- **Hallucination** : quand le LLM invente des informations fausses avec aplomb.
- **Fine-tuning** : ré-entraîner un modèle sur tes propres données (avancé, rarement nécessaire).
- **Inference** : le moment où le LLM génère une réponse (= là où ça coûte des tokens).

### 🔧 Démo (optionnel, 15 min)

**Exercice : Créer ton schéma de référence**

1. Ouvre [Excalidraw](https://excalidraw.com/) (gratuit, rien à installer)
2. Dessine les 4 briques (LLM, RAG, Agents, Orchestration) sous forme de blocs
3. Ajoute des flèches pour montrer comment elles communiquent
4. Ajoute un exemple concret (le cas du commercial ci-dessus)
5. Sauvegarde — tu réutiliseras ce schéma tout au long du programme

> 💡 **Ce schéma sera ton outil de vente** : les clients comprennent une image, pas un discours technique.

---

## Jour 2 — Maîtriser le Prompting

### Comprendre (30 min)

#### 1. Pourquoi le prompting est critique

Le prompting, c'est **la compétence n°1** en IA appliquée. Un bon prompt peut transformer une réponse médiocre en réponse excellente, sans changer de modèle ni écrire une ligne de code.

En tant que consultant, tu écriras des prompts pour :
- Les workflows automatisés (n8n, Make)
- Les chatbots clients
- Les agents qui doivent suivre des règles métier

#### 2. Les 5 patterns de prompting

**Pattern 1 — Zero-shot** (le plus simple)
> Demander directement, sans exemple.

```
Résume ce texte en 3 bullet points.
```
✅ Rapide | ❌ Peu précis sur des tâches complexes

---

**Pattern 2 — Few-shot** (le plus utile)
> Donner 2-3 exemples du résultat attendu.

```
Classe ces emails en "urgent" ou "normal".

Exemple 1 :
Email : "Le serveur est down, les clients ne peuvent plus se connecter"
Classification : urgent

Exemple 2 :
Email : "Peux-tu m'envoyer le rapport mensuel ?"
Classification : normal

Email à classer : "Notre plus gros client menace de résilier son contrat demain"
Classification :
```
✅ Très efficace | ✅ Le modèle comprend le format attendu

---

**Pattern 3 — Chain-of-Thought (CoT)** (pour les tâches de raisonnement)
> Demander au modèle de réfléchir étape par étape.

```
Un client a acheté 3 articles à 25€ et 2 articles à 40€.
Il a un code promo de 15%.
Calcule le total final.

Raisonne étape par étape avant de donner la réponse finale.
```
✅ Réduit les erreurs de calcul/logique | ✅ Traçabilité du raisonnement

---

**Pattern 4 — System Prompt** (le cadrage)
> Définir le rôle, les règles et les contraintes du LLM.

```
SYSTEM:
Tu es un assistant juridique spécialisé en droit du travail français.
Règles :
- Réponds uniquement sur le droit du travail français
- Cite les articles de loi pertinents
- Si tu n'es pas sûr, dis-le explicitement
- Ne donne jamais de conseil juridique définitif, recommande de consulter un avocat
Format : réponds en bullet points concis.
```
✅ Indispensable en production | ✅ Contrôle le comportement

---

**Pattern 5 — Output formatting** (contrôler le format)
> Demander un format de sortie précis (JSON, tableau, liste).

```
Extrais les informations suivantes de ce texte et retourne-les en JSON :
{
  "nom": "",
  "email": "",
  "entreprise": "",
  "besoin": ""
}
```
✅ Essentiel pour les workflows automatisés (n8n a besoin de données structurées)

#### 3. Les erreurs courantes en prompting

| ❌ Erreur | ✅ Correction |
|-----------|-------------|
| Prompt vague : "Résume ça" | Prompt précis : "Résume en 3 points clés de max 20 mots chacun" |
| Pas de rôle défini | Ajouter un system prompt avec rôle + contraintes |
| Trop d'instructions en un bloc | Découper en étapes numérotées |
| Pas d'exemple | Ajouter 2-3 exemples (few-shot) |
| Pas de format de sortie | Spécifier JSON, tableau, bullet points |

### Comprendre — suite (15 min)

#### 4. Le concept de "température"

La **température** contrôle la créativité du LLM :

```
Température = 0.0  → Déterministe, toujours la même réponse (idéal pour extraction de données)
Température = 0.3  → Peu de variabilité (bon pour du résumé, classification)
Température = 0.7  → Équilibre (par défaut, bon pour la conversation)
Température = 1.0+ → Très créatif, peut halluciner (brainstorming uniquement)
```

**Règle simple** : pour un client en production → température **entre 0 et 0.3**. La créativité, c'est pour le brainstorming, pas pour un chatbot qui répond sur des contrats.

### 🔧 Démo (optionnel, 15 min)

**Exercice : Construire ta bibliothèque de prompts**

1. Ouvre un document (Notion, Google Docs, ou un simple fichier texte)
2. Crée 3 system prompts réutilisables :

**Prompt 1 — Résumeur de documents**
```
SYSTEM:
Tu es un assistant de synthèse professionnelle.
- Résume le document fourni en maximum 5 bullet points
- Chaque point fait maximum 2 phrases
- Identifie les actions requises s'il y en a
- Utilise un langage professionnel et concis
- Si le document est ambigu, signale-le
```

**Prompt 2 — Extracteur de données (emails, formulaires)**
```
SYSTEM:
Tu es un extracteur de données structurées.
À partir du texte fourni, extrais les informations dans le format JSON suivant :
{
  "expediteur": "",
  "date": "",
  "sujet": "",
  "action_demandee": "",
  "urgence": "haute/moyenne/basse",
  "pieces_jointes": true/false
}
Si une information est absente, mets null.
Ne commente pas, retourne uniquement le JSON.
```

**Prompt 3 — Assistant client**
```
SYSTEM:
Tu es l'assistant virtuel de [NOM ENTREPRISE], spécialisé en [DOMAINE].
Règles :
- Réponds uniquement avec les informations de la base de connaissances fournie
- Si la réponse n'est pas dans la base, dis : "Je n'ai pas cette information, je vous mets en relation avec un conseiller."
- Ton : professionnel, amical, concis
- Ne donne jamais d'information personnelle sur les clients
- Propose toujours une action suivante ("Souhaitez-vous que je...")
```

3. Teste chaque prompt dans ChatGPT ou Claude
4. Ajuste-les selon les résultats

---

## Jour 3 — Comprendre les APIs (sans coder)

### Comprendre (30 min)

#### 1. C'est quoi une API ?

**API** = Application Programming Interface = un **guichet** entre deux logiciels.

Analogie du restaurant :
```
Toi (le client)  →  demande un plat  →  SERVEUR (l'API)  →  transmet à la cuisine  →  CUISINE (le serveur/LLM)
                 ←  reçoit le plat   ←                   ←  prépare la réponse     ←
```

Quand tu utilises n8n et que tu ajoutes un nœud "HTTP Request", **tu appelles une API**. Tu le fais déjà.

#### 2. Les concepts clés

**Endpoint** = l'adresse du guichet
```
https://api.openai.com/v1/chat/completions
```
- `api.openai.com` = le restaurant
- `/v1/chat/completions` = le menu que tu consultes

**Méthodes HTTP** = ce que tu veux faire
| Méthode | Action | Analogie |
|---------|--------|---------|
| **GET** | Lire des données | "Montre-moi la carte" |
| **POST** | Envoyer des données | "Je commande ce plat" |
| **PUT** | Modifier des données | "Changez la cuisson de mon steak" |
| **DELETE** | Supprimer | "Annulez ma commande" |

Pour les LLMs → on utilise presque toujours **POST** (on envoie un prompt, on reçoit une réponse).

**Headers** = les informations d'identification
```
Authorization: Bearer sk-abc123...   ← ta clé API (ton ticket d'entrée)
Content-Type: application/json        ← le format des données
```

**Body** = le contenu de ta demande (en format JSON)
```json
{
  "model": "gpt-4o",
  "messages": [
    {"role": "system", "content": "Tu es un assistant utile."},
    {"role": "user", "content": "Explique-moi le RAG en une phrase."}
  ],
  "temperature": 0.3
}
```

**Réponse** = ce que le serveur te renvoie (aussi en JSON)
```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "Le RAG consiste à enrichir le prompt d'un LLM avec des documents pertinents récupérés dans une base de données vectorielle."
      }
    }
  ],
  "usage": {
    "prompt_tokens": 25,
    "completion_tokens": 32,
    "total_tokens": 57
  }
}
```

#### 3. Pourquoi c'est important pour toi

Même sans coder, comprendre les APIs te permet de :
- **Configurer n8n correctement** (nœud HTTP Request, headers, body)
- **Debugger** quand un workflow ne marche pas ("le code 401 = clé API invalide")
- **Comprendre les coûts** (`usage.total_tokens` × prix par token)
- **Parler le même langage** que les développeurs chez ton client

#### 4. Les codes de réponse HTTP à connaître

| Code | Signification | Que faire ? |
|------|--------------|-------------|
| **200** | ✅ OK, tout va bien | Rien, c'est parfait |
| **400** | ❌ Mauvaise requête | Vérifie le format de ton body/JSON |
| **401** | 🔒 Non autorisé | Vérifie ta clé API |
| **403** | 🚫 Interdit | Tu n'as pas accès à cette ressource |
| **404** | 🔍 Non trouvé | Vérifie l'URL/endpoint |
| **429** | ⏳ Trop de requêtes | Tu as dépassé la limite → attends et réessaie |
| **500** | 💥 Erreur serveur | Problème côté fournisseur, pas de ta faute |

> **Le plus fréquent en IA** : le **429**. Tous les providers (OpenAI, Azure, etc.) limitent le nombre de requêtes par minute. Ton orchestrateur doit gérer les retries.

### 🔧 Démo (optionnel, 20 min)

**Exercice : Appeler l'API OpenAI avec Hoppscotch**

1. Va sur [hoppscotch.io](https://hoppscotch.io/) (gratuit, rien à installer)
2. Configure :
   - **Méthode** : POST
   - **URL** : `https://api.openai.com/v1/chat/completions`
   - **Headers** :
     - `Authorization` : `Bearer sk-TA_CLE_API` (obtiens-la sur [platform.openai.com](https://platform.openai.com))
     - `Content-Type` : `application/json`
   - **Body** (raw JSON) :
```json
{
  "model": "gpt-4o-mini",
  "messages": [
    {"role": "system", "content": "Tu es un expert en IA qui explique simplement."},
    {"role": "user", "content": "C'est quoi un embedding en une phrase ?"}
  ],
  "temperature": 0.3
}
```
3. Clique **Send**
4. Observe la réponse :
   - Lis le champ `choices[0].message.content` → la réponse du LLM
   - Lis le champ `usage` → combien de tokens consommés
5. Modifie la température à 1.0 et renvoie → compare la réponse
6. Change le `model` en `gpt-4o` et renvoie → compare qualité et tokens

> 💡 **Ce que tu viens de faire** c'est exactement ce que fait le nœud "OpenAI" de n8n en coulisses.

---

## Jour 4 — RAG (Retrieval-Augmented Generation)

### Comprendre (30 min)

#### 1. Le problème que RAG résout

Un LLM comme GPT-4 connaît beaucoup de choses **générales**, mais :
- ❌ Il ne connaît pas les documents internes de ton client
- ❌ Ses connaissances ont une date de coupure
- ❌ Il peut halluciner s'il n'a pas l'info

**RAG = donner au LLM les bons documents AVANT qu'il réponde.**

Sans RAG :
```
Utilisateur : "Quelle est notre politique de remboursement ?"
LLM : "En général, les entreprises offrent 30 jours..." ← HALLUCINATION, il invente
```

Avec RAG :
```
Utilisateur : "Quelle est notre politique de remboursement ?"
Système RAG : [cherche dans les docs internes → trouve "politique_remboursement.pdf"]
LLM + contexte : "Selon votre politique interne, le remboursement est possible sous 14 jours..." ← CORRECT
```

#### 2. Le pipeline RAG en 6 étapes

```
PHASE D'INDEXATION (une seule fois, ou à chaque mise à jour des docs)
═══════════════════════════════════════════════════════════════════

  📄 Documents            →   ✂️ Chunking          →   🔢 Embeddings        →   🗄️ Vector DB
  (PDF, Word, Web...)         Découper en             Transformer chaque        Stocker les
                              morceaux de              morceau en                vecteurs
                              ~500 mots               vecteur numérique

PHASE DE REQUÊTE (à chaque question de l'utilisateur)
═══════════════════════════════════════════════════════

  ❓ Question             →   🔢 Embedding          →   🔍 Recherche         →   🤖 LLM
  de l'utilisateur            de la question             dans Vector DB           Génère la réponse
                                                         (similarité)             avec le contexte
```

#### 3. Détail de chaque étape

**Étape 1 — Documents sources**
- Les données du client : PDFs, Word, pages web, Google Drive, Confluence, Notion, emails...
- Enjeu : accéder à ces données et les nettoyer (retirer les tableaux cassés, headers/footers, etc.)

**Étape 2 — Chunking (découpage)**
- Un document de 50 pages ne rentre pas dans le contexte du LLM
- On le découpe en morceaux ("chunks") d'environ 500-1000 tokens
- **Stratégies de chunking** :
  - Par paragraphes (le plus simple)
  - Par sections/titres (plus intelligent)
  - Avec chevauchement ("overlap") pour ne pas couper une idée en deux

```
Document : "La politique de remboursement... [50 pages]"

Chunk 1 : "La politique de remboursement s'applique à tous les produits achetés..."
Chunk 2 : "Les exceptions incluent les produits personnalisés et les logiciels..."
Chunk 3 : "Pour effectuer un remboursement, le client doit fournir..."
```

**Étape 3 — Embeddings (vectorisation)**
- Chaque chunk est transformé en un vecteur (une liste de nombres)
- Ce vecteur capture le **sens** du texte, pas les mots exacts
- Modèles d'embedding courants : OpenAI `text-embedding-3-small`, Cohere, modèles open-source

```
"politique de remboursement" → [0.12, -0.45, 0.89, 0.03, ..., 0.67]  (1536 dimensions)
"return policy"              → [0.11, -0.44, 0.87, 0.04, ..., 0.65]  ← vecteurs proches !
"météo à Paris"              → [0.95, 0.22, -0.33, 0.78, ..., -0.12] ← vecteur très différent
```

> 💡 C'est comme ça que le système comprend que "politique de remboursement" et "return policy" parlent de la même chose, même si les mots sont différents.

**Étape 4 — Vector Database (stockage)**
- Les vecteurs sont stockés dans une base spécialisée
- Cette base permet de chercher par **similarité** ("trouve les vecteurs les plus proches de ma question")
- Options : Pinecone, Qdrant, ChromaDB, Weaviate, **Azure Cosmos DB** (vector search intégré), Supabase (pgvector)

**Étape 5 — Recherche par similarité**
- La question de l'utilisateur est aussi transformée en vecteur
- On cherche les chunks dont les vecteurs sont les plus proches
- On récupère les 3-5 chunks les plus pertinents

**Étape 6 — Génération de la réponse**
- Les chunks récupérés sont injectés dans le prompt du LLM :

```
SYSTEM: Tu es un assistant. Réponds uniquement en te basant sur le contexte fourni.

CONTEXTE :
[Chunk 1 : "La politique de remboursement s'applique à tous les produits..."]
[Chunk 2 : "Pour effectuer un remboursement, le client doit fournir..."]

QUESTION : Quelle est la politique de remboursement ?
```

#### 4. Les métriques clés du RAG

| Métrique | Description | Pourquoi c'est important |
|----------|-------------|------------------------|
| **Recall** | Le % de documents pertinents retrouvés | Si on rate des docs, la réponse sera incomplète |
| **Precision** | Le % de docs retrouvés qui sont pertinents | Trop de bruit = le LLM est confus |
| **Faithfulness** | La réponse est-elle fidèle aux sources ? | Détecter les hallucinations |
| **Latence** | Temps total de réponse | L'utilisateur attend... |

### 🔧 Démo (optionnel, 20 min)

**Exercice : Tester un RAG sans rien installer**

**Option A — ChatPDF**
1. Va sur [chatpdf.com](https://www.chatpdf.com/)
2. Uploade un PDF (ex : un contrat, une fiche produit, un document interne non-confidentiel)
3. Pose des questions :
   - Question factuelle : "Quelle est la durée du contrat ?"
   - Question de synthèse : "Résume les obligations du prestataire"
   - Question piège (pas dans le doc) : "Quel est le chiffre d'affaires de l'entreprise ?"
4. Observe : est-ce qu'il hallucine sur la question piège ?

**Option B — n8n (si tu as une instance)**
1. Crée un workflow avec :
   - Nœud **Chat Trigger**
   - Nœud **AI Agent** avec un **Vector Store** (In-Memory ou Supabase)
   - Configure un **Document Loader** pour charger un fichier
2. Teste en posant des questions sur le document chargé

> 💡 Ce que tu viens de tester, c'est exactement le produit que tu vendras aux clients. La différence pro = sécurité des données, choix du modèle, et intégration dans leur SI.

---

## Jour 5 — Les Agents IA

### Comprendre (30 min)

#### 1. Agent vs Chatbot : quelle différence ?

| | Chatbot classique | Agent IA |
|---|---|---|
| **Capacité** | Répond à des questions | Répond ET agit |
| **Outils** | Aucun | Peut utiliser des APIs, BDD, outils |
| **Décision** | Suit un script linéaire | Décide dynamiquement quoi faire |
| **Boucle** | Question → Réponse (1 tour) | Peut itérer (réfléchir → agir → observer → réfléchir...) |
| **Exemple** | FAQ automatique | Assistant qui cherche dans le CRM, vérifie le stock, et envoie un email |

#### 2. Architecture d'un Agent IA

Voici comment un agent est structuré (c'est LE schéma à retenir) :

```
┌───────────────┐      ┌─────────────────────────────┐      ┌─────────────────┐
│    INPUT      │      │          AGENT              │      │     OUTPUT      │
│               │      │                             │      │                 │
│ • Événements  │─────▶│  ┌─────────────────────┐    │─────▶│ • Messages de   │
│   système     │      │  │       LLM           │    │      │   l'agent       │
│ • Messages    │      │  │  (le cerveau)       │    │      │ • Résultats     │
│   utilisateur │      │  └─────────────────────┘    │      │   des outils    │
│ • Messages    │      │  ┌─────────────────────┐    │      │                 │
│   d'agents    │      │  │   Instructions      │    │      └─────────────────┘
│               │      │  │  (system prompt)    │    │
└───────────────┘      │  └─────────────────────┘    │
                       │  ┌─────────────────────┐    │
                       │  │      Tools          │    │
                       │  │  (outils dispo)     │    │
                       │  └──────────┬──────────┘    │
                       └─────────────┼───────────────┘
                                     │ ▲
                                     ▼ │
                       ┌─────────────────────────────┐
                       │       TOOL CALLS             │
                       │                             │
                       │  🔍 Retrieval (recherche)   │
                       │  ⚡ Actions (faire qqch)    │
                       │  🧠 Memory (se souvenir)    │
                       └─────────────────────────────┘
```

**Les 3 composants internes de l'agent :**
- **LLM** : le modèle qui raisonne (GPT-4o, Claude, Mistral...)
- **Instructions** : le system prompt qui définit son comportement et ses règles
- **Tools** : la liste des outils que l'agent peut décider d'utiliser

**Les 3 types de Tool Calls :**
- **Retrieval** (recherche) : chercher dans des docs, une BDD, le web → c'est le RAG
- **Actions** : faire quelque chose (envoyer un email, créer un ticket, appeler une API)
- **Memory** : stocker/récupérer des infos d'une conversation précédente

> 💡 L'agent **décide seul** quel outil utiliser, quand, et dans quel ordre. C'est ce qui le distingue d'un workflow rigide.

#### 3. Comment fonctionne un agent (boucle ReAct)

ReAct = **Re**asoning + **Act**ing. L'agent suit une boucle :

```
BOUCLE DE L'AGENT :

1. 🤔 RÉFLÉCHIR : "L'utilisateur veut savoir si le produit X est en stock"
   → "Je dois d'abord chercher le produit dans le catalogue"

2. 🔧 AGIR : Appelle l'outil "recherche_catalogue" avec le nom du produit
   → Résultat : "Produit X - Réf: P123 - Catégorie: Électronique"

3. 👀 OBSERVER : "J'ai la référence. Maintenant je dois vérifier le stock"

4. 🔧 AGIR : Appelle l'outil "verifier_stock" avec la référence P123
   → Résultat : "Stock: 5 unités - Entrepôt: Lyon"

5. 👀 OBSERVER : "J'ai toutes les infos nécessaires"

6. ✅ RÉPONDRE : "Le produit X (Réf P123) est disponible : 5 unités en stock à Lyon."
```

#### 4. Les "Tools" (outils) d'un agent

Un agent est aussi puissant que ses outils. Voici les catégories d'outils courants :

| Catégorie | Exemples d'outils | Cas d'usage |
|-----------|-------------------|-------------|
| **Recherche** | Google, Bing, Serper | "Cherche les dernières news sur ce sujet" |
| **Base de données** | SQL query, CRM lookup | "Trouve ce client dans Salesforce" |
| **Documents** | RAG / Vector search | "Que dit notre politique sur ce point ?" |
| **Communication** | Email, Slack, Teams | "Envoie un résumé au manager" |
| **Calcul** | Calculator, Code interpreter | "Calcule la marge sur cette commande" |
| **APIs métier** | ERP, Facturation, RH | "Crée un ticket dans Jira" |

#### 5. Quand utiliser un agent vs un workflow ?

C'est LA question importante en consulting :

```
                        La tâche est-elle prévisible ?
                              /           \
                            OUI            NON
                            /                \
                    WORKFLOW               AGENT
                  (n8n, Make)         (AI Agent node)
                      /                      \
        "Toujours les mêmes          "Le chemin dépend
         étapes dans le même          du contenu/contexte"
         ordre"
```

**Exemples** :

| Tâche | Workflow ou Agent ? | Pourquoi |
|-------|-------------------|----------|
| Recevoir un email → l'archiver dans Drive | **Workflow** | Toujours les mêmes étapes |
| Analyser un email et décider si on escalade ou répond | **Agent** | La décision dépend du contenu |
| Synchroniser CRM → Tableau tous les jours | **Workflow** | Processus fixe |
| "Trouve-moi les 3 meilleurs fournisseurs pour ce besoin" | **Agent** | Nécessite recherche + comparaison + jugement |

#### 6. Les limites des agents (à connaître pour les clients)

- ⚠️ **Coût** : un agent peut faire 5-10 appels LLM par question (vs 1 pour un chatbot)
- ⚠️ **Latence** : chaque boucle = quelques secondes supplémentaires
- ⚠️ **Fiabilité** : un agent peut se tromper dans sa décision d'outil
- ⚠️ **Sécurité** : un agent avec accès au CRM peut potentiellement lire des données sensibles

> **Conseil consultant** : commence toujours par un workflow simple. Ajoute un agent uniquement quand la logique conditionnelle devient trop complexe pour un workflow.

### 🔧 Lab Agent (optionnel, 30 min) — 100% gratuit

Tu vas créer et tester un agent IA avec des tools connectés, **sans payer**. Trois options selon tes préférences :

---

**Option A — Agent dans n8n (recommandé si tu as une instance, 30 min)**

1. Crée un nouveau workflow dans n8n
2. Ajoute un nœud **Chat Trigger** (ou Webhook)
3. Ajoute un nœud **AI Agent** :
   - Modèle : GPT-4o-mini (ou ton modèle configuré)
   - System prompt :
     ```
     Tu es un assistant de recherche. Tu as accès à des outils pour chercher
     sur le web et faire des calculs. Utilise-les quand nécessaire.
     Réponds toujours en français.
     ```
4. Connecte **3 outils** à l'agent :
   - **Calculator** (nœud n8n) → pour les calculs
   - **HTTP Request** → appeler une API gratuite (ex : `https://api.quotable.io/quotes/random`)
   - **Wikipedia** (nœud n8n, s'il existe) ou un 2e HTTP Request vers une autre API
5. Teste avec des questions qui forcent l'agent à **choisir** le bon outil :
   - "Combien font 1547 × 23.5 ?" → doit utiliser le calculateur
   - "Donne-moi une citation inspirante" → doit utiliser l'API de citations
   - "Raconte-moi une blague" → ne devrait PAS utiliser d'outil
   - "Combien coûtent 15 articles à 23.50€ avec 20% de remise ?" → doit utiliser le calculateur
6. **Observe dans les logs de l'agent** (c'est la partie la plus importante) :
   - Quels outils a-t-il choisi ? Pourquoi ?
   - A-t-il raisonné avant d'agir ? (tu verras la "pensée" de l'agent)
   - S'est-il trompé d'outil ? → ajuste le system prompt pour le guider

---

**Option B — Agent dans Dify.ai (gratuit, rien à installer, 30 min)**

[Dify.ai](https://dify.ai/) est une plateforme gratuite (tier cloud gratuit : 200 messages) pour construire des agents visuellement.

1. Crée un compte sur [cloud.dify.ai](https://cloud.dify.ai/) (gratuit)
2. Clique **"Create from Blank"** → choisis **"Agent"**
3. Configure l'agent :
   - **Model** : choisis un modèle gratuit disponible (GPT-3.5 ou celui proposé)
   - **Instructions** : colle ce system prompt :
     ```
     Tu es un assistant professionnel polyvalent.
     Tu as accès à des outils. Utilise-les quand la question le nécessite.
     Si tu n'as pas besoin d'outil, réponds directement.
     Réponds en français.
     ```
4. **Ajoute des Tools** (barre de gauche → "Tools") :
   - **Web Search** (recherche web intégrée) → activer
   - **Wikipedia** → activer
   - **Calculator / Math** → activer
   - **Current Time** → activer
5. Teste dans le **Preview** :
   - "Quel est le président actuel de la France ?" → doit chercher sur le web
   - "Résume l'article Wikipedia sur le RAG" → doit utiliser Wikipedia
   - "Quelle heure est-il ?" → doit utiliser Current Time
   - "Calcule 2^10" → doit utiliser le calculateur
6. Observe le **panneau de trace** à droite :
   - Tu vois chaque étape : réflexion → choix d'outil → appel → résultat → réponse
   - C'est exactement la **boucle ReAct** expliquée plus haut !

> 💡 Dify est un excellent outil pour prototyper un agent AVANT de le construire dans n8n — tu valides le concept rapidement.

---

**Option C — Agent dans ChatGPT "GPTs" (gratuit avec un compte OpenAI, 20 min)**

1. Va sur [chatgpt.com](https://chatgpt.com/)
2. Menu en haut à gauche → **"Explore GPTs"** → **"Create"**
3. Dans l'onglet **Configure** :
   - **Name** : "Mon Assistant de Recherche"
   - **Instructions** :
     ```
     Tu es un assistant de recherche professionnel.
     Quand on te pose une question factuelle, utilise la recherche web.
     Quand on te demande d'analyser une image ou un document, utilise tes capacités de vision.
     Réponds toujours en français, de manière concise.
     ```
   - **Capabilities** : active **Web Browsing** ✅ et **Code Interpreter** ✅
4. Teste dans le preview :
   - "Quelles sont les dernières nouvelles sur l'IA en France ?" → doit chercher sur le web
   - "Calcule la moyenne de 23, 45, 67, 89, 12" → doit utiliser Code Interpreter
5. Observe comment le GPT **choisit dynamiquement** quel outil utiliser

---

**Ce qu'il faut retenir de ce lab**

```
AGENT = LLM + Instructions + Tools

              ┌── Retrieval (chercher de l'info)     → Web Search, RAG, Wikipedia
Tool calls ───┼── Actions (faire quelque chose)      → Calculer, envoyer un email, appeler une API
              └── Memory (se souvenir)               → Chat history, base de données

L'agent DÉCIDE quels outils utiliser en fonction de la question.
C'est le LLM qui raisonne, pas un workflow pré-programmé.
```

> Quelque soit l'outil (n8n, Dify, ChatGPT), le **principe est toujours le même**. C'est ça la valeur de comprendre l'architecture plutôt que juste l'outil.

---

## Jour 6 — Le Cloud pour l'IA

### Comprendre (30 min)

#### 1. Pourquoi le cloud est incontournable en IA

Les LLMs sont trop gros et trop gourmands pour tourner sur un PC classique :
- GPT-4 → impossible à héberger soi-même
- Même les modèles open-source (Llama 70B) nécessitent des GPU coûteux

**Le cloud te donne** :
- Accès aux meilleurs modèles via API (pas besoin de GPU)
- Scalabilité (100 ou 100 000 utilisateurs, même architecture)
- Services managés (pas besoin de maintenir l'infra)
- Conformité (certifications sécurité pour les clients entreprise)

#### 2. Les 3 clouds et leurs services IA

**Microsoft Azure** (le plus pertinent pour l'entreprise)

| Service | Ce qu'il fait | Quand l'utiliser |
|---------|--------------|-----------------|
| **Azure OpenAI** | GPT-4o, GPT-4, embeddings hébergés sur Azure | Quand le client veut GPT mais avec la sécurité Azure |
| **Azure AI Search** | Recherche hybride (texte + vecteur) | Composant RAG pour les gros volumes |
| **Azure AI Foundry** | Plateforme complète pour construire des apps IA | Projets IA structurés, hub central |
| **Azure Cosmos DB** | Base NoSQL + vector search intégré | Stockage de chat history + RAG |
| **Azure AI Document Intelligence** | OCR intelligent, extraction de données de documents | Factures, contrats, formulaires |
| **Azure Container Apps** | Héberger des apps conteneurisées | Déployer tes solutions IA |
| **VS Code AI Toolkit** | Extension VS Code gratuite pour télécharger, tester et fine-tuner des modèles IA (locaux ou cloud) | Prototyper rapidement avec des modèles open-source sans quitter ton éditeur |

**AWS**

| Service | Équivalent Azure |
|---------|-----------------|
| **Amazon Bedrock** | Azure OpenAI (multi-modèles : Claude, Llama, Titan) |
| **Amazon Kendra** | Azure AI Search |
| **Amazon SageMaker** | Azure AI Foundry |

**Google Cloud**

| Service | Équivalent Azure |
|---------|-----------------|
| **Vertex AI** | Azure AI Foundry |
| **Gemini API** | Azure OpenAI |
| **AlloyDB** | Azure Cosmos DB (vector search) |

#### 3. Les concepts cloud à connaître

**Région** : où sont physiquement les serveurs. Important pour :
- La latence (choisir une région proche des utilisateurs)
- La conformité (RGPD → données en Europe)
- La disponibilité des modèles (GPT-4o pas dispo partout)

**Pricing IA** : comment ça coûte

```
COÛT = Nombre de tokens × Prix par token

Modèle          | Prix Input (1M tokens) | Prix Output (1M tokens)
GPT-4o          | ~$2.50                 | ~$10.00
GPT-4o-mini     | ~$0.15                 | ~$0.60
Claude 3.5      | ~$3.00                 | ~$15.00

1M tokens ≈ 750 000 mots ≈ ~3000 pages

Exemple : un chatbot qui traite 1000 conversations/jour
  - ~2000 tokens/conversation (prompt + réponse)
  - 2M tokens/jour × $10/1M = ~$20/jour avec GPT-4o
  - Ou ~$1.20/jour avec GPT-4o-mini
```

**Clé API vs Managed Identity** :
- Clé API = un mot de passe pour accéder au service (simple mais risqué si exposé)
- Managed Identity = Azure gère l'authentification automatiquement (meilleure pratique en production)

#### 4. Comment choisir le bon cloud pour un client ?

```
Le client utilise déjà Microsoft 365 / Azure AD ?
  → OUI → Azure (intégration native, Entra ID, conformité)
  → NON →
      Le client utilise déjà AWS ?
        → OUI → AWS
        → NON → Azure ou GCP selon le besoin
```

> **Réalité du marché** : 80% des entreprises en France sont dans l'écosystème Microsoft. Azure est presque toujours le bon choix pour le consulting IA.

### 🔧 Démo (optionnel, 20 min)

**Exercice : Explorer Azure AI Foundry**

1. Crée un compte Azure gratuit : [azure.microsoft.com/free](https://azure.microsoft.com/free)
   - Tu obtiens $200 de crédits gratuits pendant 30 jours
2. Va sur [ai.azure.com](https://ai.azure.com) (Azure AI Foundry)
3. Crée un projet
4. Dans le **Playground** :
   - Sélectionne un modèle (GPT-4o-mini pour économiser les crédits)
   - Teste ton system prompt du Jour 2
   - Joue avec les paramètres (température, max tokens)
   - Observe les métriques : tokens utilisés, latence
5. Note les différences avec l'API directe d'OpenAI :
   - L'endpoint est différent (`.openai.azure.com` au lieu de `api.openai.com`)
   - Il faut spécifier un "deployment name" en plus du modèle
   - La sécurité est gérée par Azure (Entra ID, réseau privé possible)

**Exercice bonus : Tester un modèle localement avec VS Code AI Toolkit (gratuit, 15 min)**

[VS Code AI Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-windows-ai-studio.windows-ai-studio) est une extension gratuite de Microsoft qui te permet de tester des modèles IA directement dans VS Code.

1. Installe l'extension **AI Toolkit** dans VS Code (recherche "AI Toolkit" dans les extensions)
2. Ouvre le panneau AI Toolkit (icône dans la barre latérale)
3. Va dans **Model Catalog** → parcours les modèles disponibles :
   - Modèles depuis **Hugging Face** (open-source, gratuits)
   - Modèles depuis **Azure AI** (certains gratuits, certains payants)
4. Télécharge un petit modèle (ex : **Phi-3-mini** ou **Mistral 7B**) → il tourne **en local sur ta machine**
5. Teste-le dans le **Playground intégré** :
   - Pose une question simple : "Explique le RAG en 2 phrases"
   - Compare la qualité avec GPT-4o (spoiler : c'est moins bon, mais c'est gratuit et privé)
6. Observe les avantages :
   - ✅ **Gratuit** : aucun coût par token
   - ✅ **Privé** : aucune donnée ne quitte ta machine
   - ✅ **Hors-ligne** : fonctionne sans internet
   - ❌ **Qualité** : les petits modèles sont moins performants que GPT-4o
   - ❌ **Ressources** : nécessite un PC avec suffisamment de RAM (8 Go minimum)

> 💡 **Usage consultant** : AI Toolkit est parfait pour (1) prototyper rapidement un prompt, (2) montrer au client qu'on peut faire tourner de l'IA en local (données sensibles), et (3) comparer les modèles open-source avant de choisir.

---

## Jour 7 — Mini-projet de synthèse

### Objectif (60 min)

Construire un **assistant RAG fonctionnel dans n8n** qui combine tout ce que tu as appris cette semaine.

#### Architecture cible

```
┌──────────┐     ┌───────────┐     ┌──────────────┐     ┌──────────┐
│  Chat     │────▶│  Vector   │────▶│   AI Agent   │────▶│ Réponse  │
│  Trigger  │     │  Store    │     │  (avec tools) │     │          │
└──────────┘     │  Search   │     └──────────────┘     └──────────┘
                  └───────────┘
                       ▲
                       │
                 ┌─────┴──────┐
                 │  Document   │
                 │  Loader     │
                 │  (PDF/URL)  │
                 └────────────┘
```

#### Étapes pas à pas

**Étape 1 — Préparer les documents (10 min)**
1. Choisis 2-3 documents de test (PDFs, pages web, ou même du texte copié)
2. Idées : FAQ d'une entreprise, documentation produit, politique interne fictive
3. Stocke-les dans un dossier accessible (Google Drive, ou en local)

**Étape 2 — Créer le workflow d'indexation (15 min)**
1. Nouveau workflow : "Indexation Documents"
2. Nœuds :
   - **Manual Trigger** (pour lancer manuellement)
   - **Read File** ou **Google Drive** (charger les documents)
   - **Document Loader** (Default Data Loader ou PDF)
   - **Text Splitter** (Recursive Character Text Splitter, chunk size: 500, overlap: 50)
   - **Embeddings** (OpenAI Embeddings)
   - **Vector Store** (Supabase, Pinecone, Qdrant, ou In-Memory pour tester)
3. Exécute le workflow → tes documents sont maintenant indexés

**Étape 3 — Créer le chatbot RAG (20 min)**
1. Nouveau workflow : "Chatbot RAG"
2. Nœuds :
   - **Chat Trigger** (interface de chat intégrée)
   - **AI Agent** :
     - Modèle : GPT-4o-mini
     - System prompt :
       ```
       Tu es un assistant intelligent.
       Utilise l'outil de recherche documentaire pour trouver les informations
       pertinentes avant de répondre.
       Si tu ne trouves pas l'information dans les documents, dis-le clairement.
       Réponds en français, de manière concise et professionnelle.
       ```
   - **Tool : Vector Store** (connecté au même store que l'étape 2)
3. Active le workflow

**Étape 4 — Tester et itérer (10 min)**
1. Pose des questions sur tes documents :
   - ✅ Question dont la réponse est dans les docs
   - ✅ Question de synthèse ("résume les points principaux")
   - ❌ Question piège (pas dans les docs) → vérifie qu'il ne hallucine pas
2. Vérifie dans les logs de l'agent :
   - Est-ce qu'il utilise bien l'outil de recherche ?
   - Quels chunks sont récupérés ?
   - La réponse est-elle fidèle aux sources ?

**Étape 5 — Documenter (5 min)**
1. Screenshot du workflow
2. Note ce qui marche et ce qui ne marche pas
3. Liste les améliorations possibles :
   - Meilleur chunking ?
   - Ajouter plus de documents ?
   - Ajouter d'autres outils à l'agent ?

### Checklist de fin de Semaine 1

À la fin de cette semaine, tu dois pouvoir :

- [ ] **Expliquer** les 4 briques IA (LLM, RAG, Agents, Orchestration) à quelqu'un de non-technique
- [ ] **Écrire** un system prompt structuré et efficace
- [ ] **Comprendre** une réponse d'API JSON et identifier tokens/coûts
- [ ] **Décrire** le pipeline RAG complet (chunking → embeddings → vector DB → query → LLM)
- [ ] **Distinguer** quand utiliser un agent vs un workflow
- [ ] **Identifier** les services cloud Azure pertinents pour un projet IA
- [ ] **Construire** un chatbot RAG fonctionnel dans n8n

> 📌 **Prochaine étape** : Semaine 2 — Intégration, Data & Solutions Client (pipelines de données, bases de données, déploiement, monitoring, cadrage projet client)
