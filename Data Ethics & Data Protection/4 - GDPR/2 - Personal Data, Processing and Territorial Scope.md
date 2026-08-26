# GDPR — Personal Data, Processing and Territorial Scope

## 1. Personal Data (Art. 4.1 GDPR)

Article 4.1 defines **personal data** as any information relating to an identified or identifiable natural person (the _data subject_).

A single piece of data is not always enough to identify someone — but combined with additional information, it can become identifying.

_Example: identification depends on context_ Saying "my professor" is not enough to identify anyone, since many people had that professor. But if you add details — she is a woman, she teaches X, in this department — the data may now point to one specific person, and at that point it becomes personal data.

### Case 1 — _Amann v. Switzerland_ (ECtHR)

- The Court addressed the relationship between **privacy** and **personal data**.
- Held that data relating to a person's private life is covered by the protection of privacy.
- Privacy is not limited to the strictly intimate sphere: it also extends to **professional and business activities** and to the relationships a person builds in social and professional life.
- The European Court of Human Rights affirmed that privacy protection covers a person both in their private sphere and in their professional/business life.

### Working Party (Art. 29) — definition of personal data

- The Article 29 Working Party gave a broad interpretation of the definition of personal data, emphasising a wide scope of protection.

---

## 2. IP Addresses as Personal Data

### Case 2 — _Scarlet / SABAM_

- **SABAM** is a Spanish/Belgian collecting society for the protection of music copyright.
- Phonographic producers (record producers) wanted to protect their works against illegal downloading of songs (e.g. via **eMule**, an old peer-to-peer file-sharing system).
- The problem: to act against an infringing user, you must know **who** the infringer is — which requires their **IP address**.
- The IP address is normally held by the **Internet Service Provider (ISP)**.
- The European Court of Justice held that the **ISP cannot directly hand over the IP address** to the producers, because the IP address is personal data.

### Who is responsible for infringement?

- **Final users** illegally downloading the songs are committing an illegal activity.
- **Websites/platforms** can also bear responsibility — producers therefore also send letters to the website.
- **ISPs / platforms (e.g. YouTube)** are generally **not held fully responsible**, because they cannot check every single video, song or upload — it is practically impossible.
    - Note (student point): YouTube uses an automated content-ID / "strike" system where producers themselves flag infringing content.

### Practical consequence

- A phonographic producer **cannot directly obtain** the user's IP address.
- They must go **before a judge** and start a lawsuit (against an as-yet-unknown defendant).
- The judge then **orders the provider** (which assigned the IP address) to disclose it.
- This requires starting a legal action and spending money → no direct shortcut to the IP address.

### Case 3 — ECJ decision on IP addresses

- An IP address registered by an **online media service provider** when a person accesses a publicly accessible website **constitutes personal data** in relation to that provider — _where the provider has the legal means to identify the data subject_.
- Therefore the provider **cannot be obliged** to give the IP address directly.
- The party whose rights were infringed must go to the judge and start proceedings; the IP address cannot be passed to them without satisfying GDPR requirements, because it is data capable of **identifying the data subject**.

---

## 3. Anonymization and Pseudonymization

### Anonymization (Recital 26 GDPR)

- The principle of data protection should **not apply to anonymous information** — i.e. information that does not relate to an identified/identifiable person, or personal data rendered anonymous so that the data subject is **no longer identifiable**.
- **Example (hospital):** a hospital does not need to process patients' name + family name to describe that "a patient visited today has disease X." The disease can be described without identifying the person → the data is anonymized.
- **Key point:** anonymous data **cannot be re-identified** — no link can be recreated between the data and the data subject.

### Pseudonymization (Art. 4.5 GDPR)

- Processing personal data so it **can no longer be attributed to a specific data subject without using additional information**, provided that:
    - the additional information is **kept separately**, and
    - it is subject to **technical and organizational measures** ensuring non-attribution.
- Pseudonymization **reduces** liability but **does not eliminate** it — the data can still, in principle, be linked back to a person.

### Encryption (technical clarification from a student)

- Encrypted data can be read, **but not in a human-understandable way unless you have the keys**.
- Identifiers are replaced by individual codes; with the key, a link between the name and the assigned code allows re-identification.
- → Encryption is a form of **pseudonymization** (re-identification possible with the key), not true anonymization.

### Status under GDPR

- Data whose identifiers are merely removed/replaced (pseudonymized) is **still personal data** under the GDPR.
- Only **truly and fully anonymous** information falls **outside** the GDPR.

### Anonymization techniques (WP29 Opinion 5/2014)

Mentioned (not examined in depth):

- Encryption
- Hash functions (including **salted hash**)
- Keyed hash functions (with stored key / with key deletion)
- Deterministic encryption
- **Tokenization** — replacing values (e.g. card numbers) with tokens that are useless to an attacker; typically applied in the **financial sector** (also linked to crypto/Bitcoin and tokenized works of art); a technique for rendering data pseudonymous.
- See also EDPB guidelines on pseudonymization.

---

## 4. What is NOT Personal Data

The GDPR protects only **natural (living) persons**. The following are **not** personal data:

- **Deceased persons** — they were natural persons but have **no rights** and no need for protection; the purpose of the GDPR is to protect the data subject who needs protection.
    - Example: writing "my grandfather worked at Fiat, had brown hair, was a politician in X party" → processing data of a deceased person → **not** protected by GDPR.
- **Anonymous website statistics** — tracking the number of visits without identifying visitors (purely statistical purposes).
- **Data about companies / public authorities** — or any data not linkable, directly or indirectly, to a person.

### When company-related data IS personal data

Data is **personal** when it relates to **individuals who are individually identifiable**, e.g. sole traders, employees, partners, company directors.

_Example: email addresses_

|Email address|Personal data?|Reason|
|---|---|---|
|`amministrazione@polito.it` / `segreteria@polito.it` / `rettore@polito.it`|**No**|Generic office/role address — no link to a specific person|
|`anna.saraceno@...`|**Yes**|Contains the name of a natural person|
|`stefano.corneati@polito.it`|**Yes**|Name + family name → must follow GDPR, regardless of the person's role|
|`mario.rossi@fiat.it`, `giovanni.verdi@ferrari.it`|**Yes**|Even when acting as CEO/CFO/HR director representatives, the name is personal data|

- The protection does **not** depend on the context or on the content being criminal — if the name + family name appear, you are processing personal data.

### Is the content of an email personal data?

- A generic message is **not** personal data.
- It **becomes** personal data if it contains personal data of a natural person.

_Example: purpose limitation in emails_ 
Suppose I am the lawyer of Ferrari's counterparty, negotiating a contract with **Mario Rossi**. Writing to `mario.rossi@ferrari.it` to send him the contract draft is allowed, because there is a legal reason connected to the company's activity. But if in the same email I add "I'd also like to invite **you** to a seminar on automotive patents," that part is not allowed: it uses his personal data as a natural person for a purpose unrelated to the negotiation. The very same invitation sent to a generic role address such as `HR@ferrari.it` would instead be fine.

---

## 5. Special Categories of Data (Sensitive Data)

### Art. 9.1 — general prohibition

Processing of data revealing the following is **prohibited** in general:

- racial or ethnic origin
- political opinions
- religious or philosophical beliefs
- trade union membership
- genetic data
- biometric data (for uniquely identifying a person)
- data concerning health
- data concerning sex life or sexual orientation

### Art. 9.2 — exceptions

The prohibition does **not** apply when one of the listed grounds applies:

1. **Explicit consent** of the data subject for one or more specified purposes (the most important case).
    - ⚠️ Do **not** confuse consent here with the rights of the data subject (different part of the course). Here, consent is the ground that **lifts the prohibition** on processing sensitive data.
    - Examples: signing consents at a hospital; giving consent to the Politecnico (e.g. for **taxes**, which require sensitive information).
2. Processing necessary for **obligations/rights in the field of employment and social security**.
    - Example: enrolling an employee (registration, payroll lists) — no consent needed because the employer is bound to do it.
3. Processing necessary to **protect the vital interests** of the data subject.
    - Example: a car accident, a person is dying → the ambulance acts immediately **without asking for consent**.
4. Data **manifestly made public** by the data subject.
    - Example: on Instagram/Facebook you publicly reveal your religion or political opinions → no consent needed to process what is already public.
    - **BUT** purpose limitation still applies: if I know you'll vote for my party, I **cannot** send you marketing/communications asking for your vote; I may only note (internally) that "Mr. Rossi votes for us," not use the data to solicit you.

### Research purposes (discussion)

- Processing sensitive data for **statistical/research** purposes may be possible (e.g. genetic data for a researcher; studying discrimination based on non-Italian names).
- **However**, whenever the data **can be anonymized**, it **must** be — otherwise the processing is not permitted.

### Art. 10 — criminal convictions and offences

- Processing of data relating to criminal convictions, offences and related security measures may be carried out **only under the control of an official authority**, or when authorized by law.
- _ Example:_ a policeman investigating a suspect does **not** need the suspect's consent (otherwise no investigation would be possible), **but needs authorization** from the **judicial authority / the court** (the judge), **not** from the data subject.

---

## 6. Data Processing (Art. 4.2)

### Definition

**Processing** = any operation (or set of operations) performed on personal data, whether or not by automated means, such as: collection, recording, organization, structuring, storage, adaptation, alteration, retrieval, consultation, use, disclosure by transmission, dissemination or otherwise making available, alignment or combination, restriction, **erasure or destruction**.

- May be a single operation or a set of operations; automated **or** non-automated.
- **Key point:** even **deleting/erasing/destroying** data without permission is processing and infringes the GDPR. E.g. if you give me consent to store and list your data, but I delete it without your permission, I am infringing the GDPR.

### What does NOT constitute (relevant) processing under the GDPR

The GDPR does **not** apply to:

1. Processing for **national security** purposes (Recital 16 — protection of Member States' security/policy).
2. Processing by individuals **purely for personal or household activities** (Recital 18) — no connection to a professional or commercial activity.
    - Example: sending an email to a friend → household activity, GDPR not applicable.

---

## 7. The "Household Exception" — Case Law

### Case — _Jehovah's Witnesses_ (door-to-door preaching, 2018)

- Members of a religious community went **door-to-door**, collecting names, addresses and other information about the people contacted, structured for easy later retrieval (a **filing system**).
- The ECJ held this is **NOT** a purely personal/household activity:
    - the data was structured according to specific criteria enabling easy retrieval → a filing system → falls under the GDPR.
- Contrast: writing on a piece of paper "I love Maria Rossi / the church..." → **no structured processing** → GDPR does **not** apply.

### Case — list of colleagues published online

- A woman created, on her home PC, a webpage with personal information about **colleagues** who were unaware and did not consent.
- She argued it was a **household activity** done at home.
- The Court **rejected** the household exception: only activity carried out **within private/family life** — and **never** through publishing a list of data accessible to an indefinite number of people — counts as household activity.

### Case — _Buivids_ (filming police officers)

- Mr. Buivids filmed police officers at a station and **uploaded the video to YouTube**.
- The Latvian DPA ordered removal; he appealed; the Latvian Supreme Court referred the question to the ECJ.
- The ECJ held it was **NOT** a household activity, because the video was **published on the internet** and could be seen by all YouTube users.
- **Counter-question:** if he had filmed but **not published** the video → arguably a household activity for **data protection** purposes (though it may raise separate **criminal-law** issues about filming in a police station — not a data-protection matter).

**General rule from these cases:** publication to an indefinite/unlimited audience removes the household exception.

---

## 8. Territorial Scope of the GDPR (Art. 3)

### Background: EU vs US approach

- US privacy legislation is generally **weaker / less protective** than the GDPR.
- This caused friction in EU–US data-transfer arrangements ("Privacy Shield"-type agreements), creating ongoing problems for EU citizens whose data is processed by US automated systems.

Art. 3 sets out **two criteria** for territorial application.

### Criterion 1 — Establishment criterion (Art. 3.1)

The GDPR applies to processing in the context of the **activities of an establishment** of a controller/processor **in the EU**, regardless of where the processing physically takes place.

_Example_ — An Italian wine company processes its data on a server located in the **US**. The GDPR still applies, because the company is an EU establishment, regardless of where the server sits. By contrast, a purely US company, established only in the US, that sells wine **only to US customers** and processes their data in the US falls outside the GDPR — there is no EU establishment and no EU data subject.

**What counts as an "establishment":**

- A **stable arrangement** exercising real and effective activity through stable arrangements (the legal form is not decisive).
- Examples:
    - A US company's EU subsidiary (e.g. **"Fiat US Limited"** vs. the Italian **"Ecol S.r.l."**) → a stable establishment in the EU.
    - The criterion focuses on the **legal seat** plus stable, effective activity in the EU.
- Even **without a formal stable presence**, a non-EU entity can fall under the GDPR if it has a **representative, employee or agent acting with a sufficient degree of stability** in the EU (e.g. an employee based in Italy responsible for the transactions) — confirmed by the European data protection authorities.

### Criterion 2 — Targeting criterion (Art. 3.2)

The GDPR applies to a controller/processor **not established in the EU** when it processes personal data of **individuals who are in the EU**, where the processing relates to:

- (a) the **offering of goods or services** to those individuals (irrespective of whether payment is required); or
- (b) the **monitoring of their behaviour**, as far as that behaviour takes place within the EU.

_Example_ — A US company that, via a US server, offers services to Italian individuals is subject to the GDPR. Likewise, monitoring the behaviour of subjects located in Germany triggers the GDPR. The most familiar instrument of behavioural monitoring is social media (e.g. **Instagram** or **TikTok**): every video you watch shapes what you are shown next, which is precisely the monitoring of EU citizens' behaviour — so even a non-EU company must respect the GDPR.

_Example: US city-mapping startup_ 
A US startup with **no EU establishment** offers a city-mapping app for tourists, processing **location data** to show advertising for nearby restaurants, shops, monuments and museums. The app is available to tourists while they visit cities such as New York, San Francisco, Toronto, **London** and **Paris**. The GDPR applies: the startup offers services to (and processes the data of) individuals **who are in the EU**, so the processing of their location data falls within its scope.

### Controller / Processor combinations across borders

- **EU controller + non-EU processor:** The EU controller is bound by the GDPR and must sign a **Data Processing Agreement (DPA)** with the non-EU processor (e.g. a Chinese, US or Egyptian provider) to bind it to the GDPR.
- **Non-EU controller + EU processor:** The non-EU controller does **not** itself become subject to the GDPR merely by choosing an EU processor; but the **EU processor IS bound** by the GDPR, so the processing is nonetheless carried out under GDPR rules.
