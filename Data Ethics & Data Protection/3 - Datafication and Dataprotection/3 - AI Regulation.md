## 1. Recap: The Risk-Based Approach in the AI Act

The AI Act, recently adopted by the European Union, is built around a **risk-based approach** — a logic already familiar from the GDPR, which also centred on risk and risk management to address the challenges posed by new technologies.

The AI Act, as a "second generation" of regulation compared to the GDPR, aims to **minimise regulatory impact** by focusing only on the most critical uses of AI. The framework is structured around categories:

- **Prohibited applications**: Uses that are forbidden because they are incompatible with fundamental rights. Since these are simply not permitted, there is no further regulatory burden.
- **High-risk applications**: Uses that are allowed but subject to strict legal obligations. The AI Act includes a specific list of AI applications considered high-risk by law — if your application falls within this list, you must comply with the Act's requirements.
- **Non-high-risk / General Purpose AI**: For applications outside the high-risk list (including most general-purpose AI), regulation is minimal. There are a few transparency provisions (e.g., for AI-generated content), but no comprehensive framework applies.

---

## 2. Definition of Artificial Intelligence

The AI Act adopts a definition of AI largely drawn from the work of the **OECD** (an international organisation with European and North American membership), avoiding the need to reinvent definitions from scratch.

Key elements of the definition:

- **Automated system**: AI is a system with components that enable autonomous, machine-driven operation.
- **Varying level of autonomy**: AI is not necessarily fully automated. It can operate across a spectrum, from systems requiring significant human interaction to fully autonomous systems (e.g., real-time energy management or emergency response systems). Many AI systems — particularly in medicine or finance — simply present options or recommendations to human decision-makers rather than acting independently.

---

## 3. Human Supervision: Promises and Limits

The AI Act places great emphasis on **human supervision** of AI systems. Various frameworks describe this role: "human in the loop," "human over the loop," etc. While important in principle, there are significant practical limitations to effective oversight.

### The Problem of Scale and Time
In many real-world deployments, the volume of data processed by AI vastly exceeds what a human supervisor can realistically review. For example, a creditworthiness assessment may be based on hundreds of documents. A human supervisor cannot revisit all the underlying information in the same timeframe as the AI, which undermines the concept of "true" supervision.

###  The Psychology of Human Supervisors
Humans embedded in organisations often face **asymmetric incentives** when deciding whether to override AI recommendations:

- If a human overrides the AI and something goes wrong, they bear full personal responsibility.
- If a human follows the AI's recommendation and something goes wrong, blame is diffused toward the tool.

This creates **risk aversion**: individuals are less likely to challenge AI outputs even when they disagree, because going against the machine places sole responsibility on their shoulders. Genuine supervision requires both the *ability* and the *willingness* to question AI — both of which are frequently undermined in practice.

---

## 4. The AI Act's Risk Classification System

### Prohibited Practices (Art. 5)
Certain AI applications are outright banned because they are incompatible with EU values and fundamental rights. Examples include:

- Social scoring systems by public authorities
- Real-time remote biometric identification in public spaces (with narrow exceptions)
- Subliminal manipulation techniques

### High-Risk AI (Annex III)
High-risk AI applications are listed in Annex III of the Act and span domains such as:

- Biometric identification and categorisation
- Critical infrastructure management
- Education and vocational training
- Employment and workforce management
- Access to essential services (credit, insurance, etc.)
- Law enforcement
- Migration and border control
- Administration of justice

Organisations deploying high-risk AI must comply with obligations including: risk management systems, data governance, technical documentation, transparency, human oversight measures, accuracy and robustness requirements, and registration in an EU database.

### General Purpose AI (GPAI)
General-purpose AI models (such as large language models) are subject to a separate, lighter regime. Providers must maintain technical documentation, comply with copyright law, and publish summaries of training data. Models with **systemic risk** (above a defined computational threshold) face additional obligations including adversarial testing and incident reporting.

---

## 5. Fundamental Rights Impact Assessment (FRIA)

Beyond the AI Act's compliance obligations, there is a broader tool specifically designed for assessing the impact of AI on **fundamental rights**: the **Fundamental Rights Impact Assessment (FRIA)** model.

###  Origins and Endorsements
This model was originally developed and adopted by the **Spanish Data Protection Authority**. It has since been endorsed by:
- The **European Commission** (for research projects)
- The **United Nations** (as a model for impact assessment)

### Structure of the Model
The FRIA follows three standard phases of risk assessment:

1. **Planning and Scoping**: Understanding the AI system in question — its type, context, and purpose.
2. **Analysis**: Identifying potentially affected categories of people and the specific fundamental rights at stake (e.g., non-discrimination, data protection, access to essential services).
3. **Risk Assessment and Mitigation**: For each right, assessing the level of impact and identifying mitigation measures; then reassessing to determine **residual risk**.

### The Questionnaire
The initial scoping phase uses a structured questionnaire, designed to be used by experts. It focuses on four key areas:

1. **Description of the AI project**
2. **Fundamental rights framework** of the country/countries where the AI will be deployed (important if deployment extends beyond jurisdictions with strong rights protections)
3. **Existing controls** (e.g., if GDPR compliance has already been verified for training data, that exercise need not be repeated)
4. **Stakeholders**

### The Impact Assessment Matrix
Rather than assigning numerical scores (which introduce false precision), the model uses **qualitative ranges**: *low*, *medium*, and *high* — consistent with the language used by courts when ruling on fundamental rights cases.

Two matrices are used:

- **Probability matrix**: Combines *likelihood of the event* with *exposure* (number of people potentially affected).
- **Severity matrix**: Combines *degree of rights impact* with *the effort required to overcome the negative effect*.

These two matrices yield an overall impact level for each right assessed. After mitigation measures are implemented, the process is repeated to determine residual risk.

> The model (including a guide and four worked case studies) is available online in **English, Catalan, and Spanish**. One case study addresses a university AI system predicting students' learning outcomes.

### Why This Approach Matters in Court
The FRIA is not only a design tool — it can serve as a **legal defence** if an organisation is accused of infringing fundamental rights through its AI system. For this reason, the assessment logic must align with how courts reason about fundamental rights, making the qualitative, rights-centred approach more persuasive than a purely quantitative one.


