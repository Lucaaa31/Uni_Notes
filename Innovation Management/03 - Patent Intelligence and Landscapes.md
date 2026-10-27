# Patent intelligence
Four terms often used as synonyms, but with different meanings (more structure, more interpretation →):

![[03 - Patent Intelligence and Landscapes-1793120671620.webp|650]]

- **Patent data:** the records published by offices (bibliographic fields, text, codes, citations, legal events)
- **Patent information:** data organised to **answer a question** (who owns what, where, since when)
- **Patent analytics:** statistical and computational methods that turn patent data into actionable insights
- **Patent intelligence:** the use of patent analytics to **support decisions** on technologies, competitors, partners and markets. It is a part of **technology intelligence**.

## Why patents? A forward-looking source
**Strengths**
- **Disclosure:** granted only if described in enough detail to be reproduced
- **Timing:** published ~18 months after priority, often **years before** products reach the market
- **Coverage:** ~70% of technology information is in patents, mostly public
- **Richness:** codes, dates, owners, inventors, citations, family links

**Limits**
- Not every invention is patented (secrecy, lead time)
- **Propensity to patent differs across sectors**
- **Value is highly skewed:** a few patents hold most of the value

→ Patent data measure **inventive activity, not innovation** itself. Every indicator is a modelling choice.

## Key terms
- **Active patent family:** at least one pending published application or granted patent not lapsed, withdrawn, invalidated or rejected
- **International family:** protection sought beyond the home office
- **NPL citation:** citation to non-patent literature (mostly scientific articles) → link between science and technology
- **Freedom to operate (FTO):** search to check that a product can be made and sold without infringing others' (**blocking**) patents

## Decisions it supports
A cycle: technology trends → R&D prioritization and collaboration → IP protection strategies (what, when, where to file) → monitoring (competitors, FTO) → portfolio management (keep, license, sell, abandon) → back to trends.

| User | Uses it for |
|---|---|
| Top management | long-run technological trends, competitors' strategies |
| CTOs | relative strengths of the R&D portfolio, prioritization |
| IPR managers | filing and renewal approaches, licensing in/out |
| Start-uppers | evaluating collaborations, ex-ante FTO |
| Researchers | innovation trends, knowledge sources, avoid duplicating research |

→ **Same data, different questions:** define the user before the search.

---

# Patent landscapes
**Patent landscape:** a **quantitative analysis**, combining **technological** and **economic** perspectives, based on large-scale worldwide patent data, to derive information for innovation management. It is a **snapshot** at a given date.
- **Economic perspective:** who invests in a technology, how much and where; competitors, partners, markets, value of portfolios
- **Technological perspective:** what the technology is (classes, text, claims), how it evolves (trends, maturity, recombination), where knowledge comes from (citations)

**Units of analysis:** firm level (pool of competitors), technology level (domain, product class, technical issue), geographical level (country, region, city).

**Objectives:** understand innovation trends · benchmark competitors' patenting strategies · identify key innovators · discover blocking patents (FTO) · find R&D partners, licensees or licensors · evaluate R&D projects.

## Types of information: who, what, when, where, how
| Question | Fields | Watch out |
|---|---|---|
| **Who** | applicants, assignees, inventors | the "name game" → entity disambiguation, consolidate the ultimate owner |
| **What** | IPC/CPC codes, title, abstract, claims, description | codes are multi-label and hierarchical, text needs NLP; both define the **perimeter** |
| **When** | priority, filing, publication dates | priority date is closest to the invention; 18-month publication lag → **truncation** at the end of series |
| **Where** | priority office, publication countries, inventor country | inventor address = where R&D happens; offices = where markets are |
| **How** | citations, families, legal events | citation networks, family links, ownership changes |

External data can be added: firm financials, regional data, individual-level data, regulatory/historical events.

## What a landscape measures
- **Innovation trends:** applications/families by country, ranking of innovators or sub-fields, family size, patent density
- **Competitors:** portfolios, international extension, residual life, collaborations (co-assignments), specialization, reassignments
- **High value / high risk:** citation profiles, prosecution history and litigation, science-based inventions (NPL), standard-essential patents

## Data sources
- **Free:** TIP → PATSTAT, Espacenet (EPO), PatentsView (USPTO), Google Patents, Lens.org
- **Commercial:** Orbit (available at PoliTO), Derwent, PatentSight
- **Other:** REGPAT (OECD, patents linked to regions), OECD patent quality indicators, patent-to-paper citations (Marx & Fuegi, 2020)

## The process
Complex information (140M+ documents + other sources) → **elaboration** (define the objective, define the **perimeter** through a search strategy) → easy-to-use output (database, report) → strategies (R&D, competitors, collaborations, markets).

→ **The search strategy is a classification problem:** decide which patents belong to the perimeter and which do not. When the perimeter is a technology, classification codes help.

---

# Example: WIPO (2024), Patents and the SDGs
**SDGs:** 17 goals, 169 targets, adopted by the UN in 2015 (Agenda 2030), covering social, economic and environmental issues. They are defined by **outcomes, not technologies**.

**Why map patents to SDGs:** patents are objective and forward-looking (they disclose R&D years before products), and **active** patents show commitment (owners pay renewal fees) → quantifiable measure of inventive effort per goal.

### Problems of mapping patents to SDGs
- **No explicit link:** legal/technical text never mentions the SDGs
- **Hidden relevance:** a patent can be useful for an SDG without mentioning anything related
- **Unequal reach of technology:** not all SDGs can be improved by technical solutions (e.g. SDG 5)
- **Very broad goals:** e.g. SDG 9 Industry, innovation and infrastructure

## The design of the report
Read it as a template: **data → perimeter → indicators → actors** (the same sequence as the project work).
1. **Data:** PatentSight, **DOCDB simple families**, **active** families at 31/12/2023, ultimate owner consolidated (M&A, reassignments)
2. **Mapping:** UN documents → **100 patentable technology categories** → one expert **Boolean search strategy** per technology (IPC/CPC codes + text) → patents linked to one or more SDGs
	- Only **13 of 17** SDGs mapped: 8, 10, 16, 17 are socioeconomic, left out by construction
	- Queries not published in full → **not reproducible**
3. **Breakdowns:** by SDG (counts, trends, shares), by technology (WIPO concordance: 35 fields in 5 sectors), maturity
4. **Who and where:** **international families only** → proxy for value (worth protecting abroad) and reduces national biases (~48% of families are filed in one office only, about half of all families are China-only)

## Main results
- 15.2M active families, **4.7M (31.4%) SDG-related**
- **SDG 9** is by far the largest (~2.9M: electronics, manufacturing, materials), then SDG 13 and SDG 3 (~1.1M each)
- **Multi-label:** a family can belong to several SDGs (e.g. greenhouse gas emission reduction in both SDG 9 and 13) → counts by SDG do not add up to the total
- All SDGs grow; using **shares** (which control for the general growth of filings) SDG 9 goes from <10% to ~20% of all active families, SDG 13 and 7 grow fastest
- Chemistry has the highest SDG share among sectors; electrical engineering grows fastest
- Among offices, the EPO has the highest share of SDG-related patents

## The Innovation Maturity Matrix
![[03 - Patent Intelligence and Landscapes-1793120720351.webp|420]]

- **y, innovation intensity** (log scale): number of SDG-related applications
- **x, relative recency:** how recently they were filed

$$R = \frac{\sum_i w_i \cdot i}{n \cdot \sum_i w_i}$$

with $i$ = year index (1 for the first year), $w_i$ = applications filed in year $i$, $n$ = number of years. Relative recency = $R_{SDG} / R_{\text{all SDG patents}}$ (whole dataset = 1).

| | Older | More recent |
|---|---|---|
| **Many** | mature sectors | **current hot topics** |
| **Few** | modest development | **emerging interest** |

## What the report cannot tell us
- Whether an SDG patent has any real **impact** on the goal: it measures inventive effort, not outcomes
- How each patent was assigned: proprietary strategies → perimeter cannot be replicated or tested
- Anything on SDGs 8, 10, 16, 17

→ **Open question:** how do we decide, **reproducibly and at scale**, that a patent is related to an SDG?

---

# Classifying patents by SDG
**Patent-to-SDG classification:** learning a function $f(x) \rightarrow y \in [0,1]^{17}$ that maps a patent $x$ (text + metadata) to its relevance for each SDG.

**Why it is hard**
- SDGs are defined by societal outcomes; IPC/CPC describe technologies → there is no SDG class
- **Multi-label and soft:** a patent can serve several goals
- **No gold standard:** no large public labelled dataset → supervised learning can be neither trained nor evaluated; expert labelling is costly

Easier relatives: technology classification (IPC/CPC exist for every patent); circular economy or critical raw materials (partly covered by CPC Y02, Y10 + keywords).

→ **The choice of technique is a choice about where the labels come from.**

## Four families of techniques
| Approach | How it works | Main limitation |
|---|---|---|
| **Keyword-based** | SDG thesauri from Agenda 2030 (+ IPC/CPC codes), expanded with TF-IDF or embeddings. Also the expert queries of the WIPO report | manual effort; no context; polysemy and synonyms; no accepted SDG dictionary |
| **Transfer learning** | train on SDG-labelled scientific papers, apply to patents | **domain shift** (patents claim legal scope, papers report findings); one SDG per patent |
| **Supervised** | fine-tune a transformer on patents labelled by experts (e.g. 8k JPO patents) | one jurisdiction; expensive labels; unclear generalization |
| **Citation-based** | a patent citing an SDG-tagged article (NPL) inherits its SDG | citations are **noisy** (often added by examiners for legal reasons) and **incomplete** (many patents cite no science) |

- Keywords are **transparent and cheap**: every match can be explained.
- **Trade-off:** transfer learning has many labels from the wrong domain; supervised learning has the right domain but few labels.

## From noisy citations to silver labels (Ascione & Tamagnone, 2025)
Uses the weak NPL citation signal to build a **silver standard** (labels good enough to train and evaluate):

![[03 - Patent Intelligence and Landscapes-1793120769082.webp|700]]

1. **Semantic abstraction:** an LLM extracts **functions, solutions and applications** from patents and from SDG papers
2. **Similarity and ranking:** concepts are embedded and compared; rankings are fused (Reciprocal Rank Fusion) into a **17-dim SDG vector**
3. **Weakly supervised optimization:** thresholds and top-N are tuned to recover the NPL signal, **without penalizing new labels**
