## 1. Evolution of Data Protection Laws

### 1st Generation: Focus on Government Control
The concern was about the impact on government and on the governance of individuals. In terms of risk management, the tools were:

- **Independent authority** — a sort of standard control able to check whether the government in power properly collected personal data, based on consent or on a position recognized by the laws. This is the first element to control the potential risk.
- **Transparency** — providing information about who collected the data, for which purpose data are collected, and also the specific rights of data subjects, i.e. the way in which data subjects can check who is collecting what data.
- **Internal organization** — those who process data should properly process them based on legal principle and purpose limitation, but also based on an internal organization that prevents any misuse or illicit access to the data by those who are part of the organization but do not have the task that makes it legitimate to access this data.

The risk-based approach was already reflected in the first data protection law, not just from the general perspective that we are concerned about the impact of digitalization. Within the law there was a series of instruments designed to tackle risk: from external control, from internal control, and from the role of the data subject.

### 2nd Generation: Balance of Interests

There is no major change in terms of measuring risk, except for the **balance of interests**. This is not trivial in terms of risk, because it means that not all limitations to data protection are possible due to the fact that there are other competing interests. But competing interests can prevail over data protection only if they have the same level of strength in terms of protection — the rights or other interests that, according to the law, have the same level of protection. The second generation is not so impactful in terms of organization and risk management.

### 3rd Generation: Self-Assessment and Consent
- **Context:** Introduces a layer of individual responsibility.
- **Consent as Risk Assessment:** The data subject evaluates whether to provide data based on trust (e.g., assessing a website's trustability before use).
- **Challenges:** The complexity of AI and large-scale data processing makes it difficult for individuals to truly understand risks (e.g., psychological impacts or addiction).
- **Shift:** Moving from individual consent to internal governance and systemic risk prevention (similar to tobacco or alcohol regulation).

---

## 2. Risk Management & Data Protection Impact Assessment (DPIA)

### The Current State
- **Accountability:** Demonstrating that necessary internal processes are in place to manage risk.
- **Self-Assessment Strategy:** Typical in technology regulation because external supervision of every activity is impossible. It requires:
    1. Sanctions for improper assessment.
    2. Active supervision/inspection by authorities.

### Formal Assessment (Article 35 GDPR)
- **General Principle:** Every data process requires some level of assessment (e.g., data minimization, legal ground).
- **Data Protection by Design:** Integrating protection into the design phase (e.g., choosing not to use invasive body chips for general university access).
- **High Risk Threshold:** Formal DPIA is required only for "High Risk" cases (significant impact on fundamental rights, not just data protection).
    - *Examples:* Systems predicting medical outcomes (cancer), tracking apps during COVID, or large-scale surveillance (Article 35(3)).

### DPIA Components
1. **Project Description:** Goals, means, and operations.
2. **Impact Analysis:** Affected people and rights (beyond just privacy).
3. **Mitigation:** Identifying the best design to minimize impact.

---

## 3. Case Studies & Examples

### 1. Exoskeletons & Quasi-Biometric Data
- **Problem:** A company wanted to do full body scans to create perfectly fitted exoskeletons for employees.
- **Risk:** Body scans are "quasi-biometric"—highly invasive and difficult to change. High risk of future misuse (surveillance).
- **Solution (DPIA):**
    - Realized full scans were disproportionate.
    - Switched to **adaptable exoskeletons** (adjustable sizes).
    - Used anonymous measurement distribution instead of individual scans.
    - *Exception:* For employees with specific physical diseases, a partial, limited scan might be justified by the safety benefit (proportionality).

### 2. The Afghanistan Biometric Database
- **Context:** US Army used biometrics (iris, fingerprints) to distribute aid in Afghanistan to prevent fraud.
- **Risk:** After the US withdrawal, the database fell into the hands of the Taliban.
- **Lesson:** Biometrics are permanent and highly risky if control shifts.

### 3. Industrial Monitoring
- **Fingerprints for Access:** Disproportionate for canteens; potentially justified for high-security areas (military bases, chemical plants) where emergency access speed is critical.
- **Subcutaneous Chips:** Historically proposed for all-in-one access/payment. Extremely invasive due to real-time tracking potential.
- **Safety Wearables:** Using gyroscopes to detect falls in lone night workers.
    - *Issues:* Must not monitor continuously (labor law); must be isolated from personal smartphone data if using an app.

### 4. Smart City Tracking
- **The Case:** A city tracked tourist flows via Bluetooth IDs.
- **Failure:** The IDs remained permanent, allowing individual tracking.
- **Sanction:** Imposed because the *design* allowed tracking, even if the city didn't actually track individuals.

### 5. The BIP Card (Piedmont Transportation)
- **Goal:** Track passenger flows and verify usage for government funding.
- **Initial Risk:** Unique IDs on cards could track a person's entire movements across the region (mass surveillance).
- **Redesign (Progressive Anonymization):**
    - **Split Databases:** Separated "Contractual Data" (identity) from "Mobility Data" (travel).
    - **Hash IDs:** Mobility data used a hashed ID that changed over time.
    - **Stages:**
        1. Days 1-3: Reversible (for lost cards).
        2. Up to 60 days: Pseudo-anonymous (track segments stay connected to the same randomized ID).
        3. After 60 days: Fully anonymous (segments are broken; impossible to reconstruct a full journey).

---

## 4. Introduction to the AI Act

### Philosophy: Responsible Innovation
- **The "Third Way":** Innovate while mitigating risk.
- **Global Comparison:**
    - **USA:** Market-oriented, political pressure rather than strict law (e.g., Open AI & Defense Dept).
    - **China:** Political pressure for social stability (e.g., TikTok's different domestic version).
    - **EU (AI Act):** Regulatory approach to protect the market and citizens since the EU lacks the leading AI companies to exert political pressure directly.

### Risk-Based Framework
- **Prohibited Uses (Article 5):** Unacceptable risks (e.g., social scoring).
- **High-Risk Systems:** Requires formal risk management systems (similar to chemical industry regulation).
- **General Purpose AI (GPAI):** Addressed late in the process (after ChatGPT). Mostly focused on systemic risk and transparency for generated content.
- **The "List" Approach:** Unlike GDPR (which relies on case-by-case assessment), the AI Act lists specific high-risk applications (Annex III). This provides industry clarity but may be less flexible.

