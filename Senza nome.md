# Data Protection: The General Data Protection Regulation (GDPR)

_Politecnico di Torino – A.Y. 2025/2026 – Avv. Anna Saraceno_

> Scope of this lecture: after a short recap of the background, the class focuses on **what counts as personal data**, the **justification grounds (lawful bases)** for processing, the **special rules for sensitive data**, and the **subjects involved in processing (controller, processor, joint controllers)**. The lecture stops at slide 50 and will continue with processor/joint-controller rules (artt. 28–29) in the next class.

---

## 1. Background and sources of the GDPR

### What the GDPR is

The General Data Protection Regulation concerns the protection of natural persons with regard to the processing of personal data and the free movement of such data. It was adopted by the Council of the EU and the European Parliament in **April 2016** and entered into effect on **25 May 2018**. It aims to protect the fundamental rights and freedoms of individuals with respect to personal data, and it sets rules that organisations must follow when processing personal data in the EU (or the personal data of EU citizens), while promoting the free movement of such data within the EU.

### The chain of sources leading to the GDPR

The right to data protection has deep roots in human-rights instruments and earlier data-protection law:

- **Universal Declaration of Human Rights (1948)** – set standards for each individual, including the right to a private life and to freedom of expression.
- **European Convention on Human Rights (ECHR)** – adopted 1950, in force 1953. The **European Court of Human Rights** was set up in Strasbourg in 1959 to ensure observance. Data protection forms part of **Article 8 ECHR** (right to respect for private and family life, home and correspondence).
- **OECD Guidelines (1980)** – first outlined general principles for data protection (Guidelines on the Protection of Privacy and Transborder Flows of Personal Data).
- **Convention 108 (Council of Europe, 1981)** – applies to all data processing by both private and public sectors (including judiciary and law enforcement); protects sensitive data and gives the right to know if information is stored.
- **Data Protection Directive 95/46/EC (1995)** – built on Convention 108; a framework that EU Member States had to transpose into national law (so it depended on national implementation).
- **GDPR (2016, enforceable 2018)** – replaced and repealed the Directive, harmonising data-protection law across the EU. It preserves the Directive's core principles and data-subject rights, but adds new obligations: **privacy by design and by default**, the **Data Protection Officer (DPO)**, the **right to data portability**, and the principle of **accountability**. Key features: no need for national implementation, update of existing national law, comprehensive rules on territorial scope.

### The role of the CJEU

The Court of Justice of the EU has jurisdiction to determine whether a Member State has fulfilled its obligations under EU data-protection law and to interpret EU legislation for its effective and uniform application. Importantly, even though the Directive was repealed, **pre-existing case law remains relevant and valid** for interpreting EU data-protection principles, to the extent that the core principles were carried over into the GDPR.

### The right to data protection is not absolute

Under Art. 8 of the **Charter of Fundamental Rights of the EU** (as interpreted by the ECtHR and CJEU), processing personal data is a lawful interference with private life only if it: is in accordance with the law; respects the essence of the right; is necessary (subject to proportionality); and pursues an objective of general interest recognised by the EU, or the need to protect the rights of others.

### Why the GDPR matters ("data is everywhere")

The GDPR gives citizens back control over their personal data and, by simplifying the monitoring environment for international business, facilitates international data flows and ends distortions of competition. Its main novelties: extended territorial scope; new basic principles; new obligations (also for processors); new data-subject rights; rules on children's consent; and **high fines — up to €20 million or 4% of total worldwide turnover**.

---

## 2. What constitutes "personal data"

### Definition

"Personal data" is defined in **Art. 4.1 and Recital 26 GDPR**: any information relating to an identified or identifiable natural person. Identification can be **direct or indirect**, e.g. by reference to a name, an identification number, location data, an online identifier, etc. Data about a natural person is what the GDPR protects.

> **Core test of the lecture:** _am I processing data about a natural person?_ If I write an email to _Stefano.Corniati@polito.it_, I am processing the name and surname of the rector — i.e. data about a natural person — so the GDPR applies.

### When a person is "identifiable"

A single piece of data (hair colour, occupation, car) might not be enough to identify someone. A **combination** of data, however, can change the situation and make the person identifiable.

### Difference from "privacy"

Personal data is **not limited to the private sphere** of an individual. _Example: ECtHR No. 27798, 16 February 2000, Amann v. Switzerland_ — confirming that the notion of personal data goes beyond strictly private life. The broad notion is also set out in **Opinion 4/2007 of the Article 29 Working Party (ART29WP)**.

_Example (IP address as personal data):_ the Court treated IP addresses as personal data in _CJEU C-70/10, 24 November 2011, Scarlet v. SABAM_ and _CJEU C-582/14, 19 October 2016, Breyer v. Bundesrepublik Deutschland_.

### Anonymisation and pseudonymisation

Both are encouraged but treated differently (Recital 26; Art. 4.5):

- **Pseudonymisation** merely reduces linkability but still allows some re-identification (e.g. encryption, or replacing identifiers with individual codes).
- **Anonymisation** means the data can no longer be linked to a person at all.

Data whose identifiers have only been **removed (anonymised) or replaced (pseudonymised)** is **still personal data** for GDPR purposes. Only information that is **truly and fully anonymous even before being processed** falls outside the GDPR.

The most-used pseudonymisation techniques (ART29WP **Opinion 5/2014** on Anonymisation Techniques): encryption with a secret key; hash function (including salted-hash); keyed-hash function with stored key; deterministic encryption or keyed-hash with deletion of the key; and **tokenisation** (typical in the financial sector, replacing card ID numbers with values of reduced usefulness to an attacker). See also **EDPB Guidelines 1/2025 on Pseudonymisation**.

### What is NOT personal data

Information on **deceased persons**; cookies merely counting website visits without identifying visitors (statistics); data about **companies or public authorities**; and any data that cannot be linked directly or indirectly to a person. However, information about individuals acting as **sole traders, employees, partners and company directors** _is_ personal data when it relates to them as individuals and they are individually identifiable.

---

## 3. Data processing

### Definition (Art. 4.2)

A very broad definition: any operation or set of operations performed on personal data, whether or not by automated means. Examples listed include: recording, collecting, organising, structuring, adapting or altering, storing, retrieving, consulting, disclosing by transmission, using, disseminating or otherwise making available, aligning or combining, erasing or destructing, restricting. **Pseudonymisation and anonymisation are themselves processing.**

### What does NOT constitute processing

The GDPR does not apply to processing for **national security** purposes or to processing carried out by individuals purely for **personal/household activities** (Recitals 16 and 18). In the absence of a processing activity, the GDPR does not apply.

_Example (household exception denied): CJEU C-101/01, 6 July 2003, Bodil Lindqvist._ Mrs Lindqvist, a catechist in Sweden, set up a web page on her home PC for church-goers that included personal information about catechist colleagues (who were unaware and did not appreciate it). She was criminally accused of unlawful processing and claimed the household exception. The CJEU rejected it: the exception covers only private/family-life activities and never the publication of data on the internet, which makes data accessible to an indefinite number of people.

_Example (household exception denied): CJEU C-345/17, 14 February 2019, Sergejs Buivids v. Datu valsts inspekcija._ Mr Buivids filmed police officers at a police station and uploaded the video to YouTube. The Latvian DPA ordered removal. The CJEU held this was **not** household activity — no exemption — because the video was uploaded to the internet.

_Reference: CJEU C-25/17, 10 July 2018, Tietosuojavaltuutettu v. Jehovan todistajat_ (Jehovah's Witnesses), cited here on the notion of processing and later on controllership.

---

## 4. Where the GDPR applies (territorial scope, Art. 3)

Art. 3 defines territorial scope on the basis of **two main criteria**.

### The "establishment criterion"

The GDPR applies if the enterprise (Art. 4.18) — acting as controller **or** processor — is **established in the Union**, regardless of whether the processing itself takes place in the Union. An enterprise is established in the Union if it has a **stable presence** there carrying out real and effective activities, considering the nature of the economic activity. _Reference: CJEU C-230/14, 1 October 2015, Weltimmo s.r.o. v. NAIH._

The threshold can be low: per **EDPB Guidelines 3/2018 on Territorial Scope**, where a controller's activities centre on online services, even a **single employee or agent** of a non-EU entity acting with sufficient stability may be enough to constitute a stable arrangement. Lacking a branch or subsidiary in a Member State does not preclude having an establishment there.

### The "targeting criterion"

The GDPR also applies where the enterprise is **established outside the Union** but processes data of individuals **who are in the Union**, where the processing relates to: (a) the **offering of goods or services** to such data subjects in the Union (whether or not payment is required); or (b) the **monitoring of their behaviour**, as far as it takes place within the Union.

_Example: US city-mapping start-up._ A start-up established in the USA, with no business presence in the EU, offers a city-mapping app for tourists that processes location data to offer targeted advertising for nearby restaurants, bars, shops and hotels. The app is usable while visiting New York, San Francisco, Toronto — but also London, Paris or Rome. By offering services to individuals in the Union, the processing of those EU-located data subjects' data **falls within the scope of the GDPR**.

### Two cross-border configurations

- **EU enterprise + processor outside the GDPR's reach** → handled through a **Data Processing Agreement** (see §8).
- **Processor falls under the GDPR, but the controller does not** → the non-EU controller does **not** become subject to controller obligations simply because it chooses an EU processor; but the **processor itself must comply** with the GDPR if it meets the applicability criteria.

---

## 5. Who is protected, and the justification grounds

### Who is protected

The GDPR protects **natural persons** (the "**data subject**", Art. 4.1). It applies to **controllers and processors**, who may be natural persons, legal persons, government bodies, etc., irrespective of size, sector, number of employees or turnover.

### Justification grounds (lawful bases) — general rule

For **any** processing activity, the controller must identify one or more valid grounds (a "lawful basis") to justify the collection, use and other processing. **Art. 6** gives an **exhaustive list**:

- contract
- legal obligation
- legitimate interest
- consent
- vital interest
- task carried out in the public interest / exercise of official authority

> A recurring exam point: **do not confuse** the _justification grounds_ with the _principles_ on which the GDPR is built and with the _rights_ of the data subject.

---

## 6. The justification grounds in detail

### 6.1 Contract

Available when processing personal data is needed to comply with **contractual obligations** — some obligations simply cannot be performed without collecting certain personal data. It also covers **pre-contractual requests** from potential clients.

"Contractual necessity" must be interpreted **strictly**: the controller must show the processing is **objectively necessary** for a purpose integral to performing the contract (or to the pre-contractual step). Processing that is merely _useful_ but not objectively necessary is **not** covered. The mere fact that data processing is _mentioned in a contract_ does not make it necessary for performance.

_Example (pre-contractual request)._ If a potential client asks for a quote, I need their name and email to reply — that is a pre-contractual step, and there is not yet a contract. But asking _how they found me_ is **not necessary** to provide the quote, so that extra data cannot be justified on this ground.

_Example (laptop delivery)._ If a customer orders a laptop that the shop must ship, the customer's **address is necessary** for delivery and may be processed on the contract basis. If, instead, the laptop is handed over in the shop, the address is **not** necessary. Crucially, the **same address collected for delivery cannot then be used to send promotional material** — that would need another lawful basis (e.g. consent), not the contract.

_Example (bank account / pre-contractual checks)._ When opening a bank account, the bank may process not only name and surname but also the customer's financial situation, because that is integral to the contemplated contract. By contrast, requiring a **health certificate** would generally not be justified by the contract unless it is genuinely integral to the specific service.

### 6.2 Legal obligation

Available when data is processed to comply with a **legal obligation** imposed by **EU law or applicable national law** of a Member State (including secondary legislation or a binding decision of a public authority); the obligation must be **mandatory**. Consent is irrelevant here — the obligation governs.

Examples of valid legal obligations: banks consulting an **official list of registered debtors**; employers reporting employees' **salary data** to social-security or tax authorities using the social-security number; financial institutions reporting **suspicious transactions** to competent authorities under **anti-money-laundering** rules.

_Example (real-estate agency / bank, AML)._ A real-estate agency or a bank processes clients' personal data (name, date and place of birth, address) to carry out anti-money-laundering verification. Even with **no consent**, even if the account is not yet open or the client has not yet decided to buy, the agency or bank **must** carry out the verification under AML legislation — so processing is lawful on the legal-obligation ground.

### 6.3 Legitimate interest

Processing may be justified to pursue the **legitimate interests** of the enterprise or a third party, **provided** the interests and fundamental rights of the individual do not override those interests. The interest must be **real and present** (expected in the very near future), ruling out vague or speculative interests, and must be in accordance with applicable EU and national law. A **case-by-case assessment** is required for every processing activity relying on this ground — the **3-step assessment / Legitimate Interest Assessment (LIA)**.

There is **no exhaustive list**, but the GDPR and ART29WP give examples: preventing fraud; ensuring staff health and safety; employee monitoring for safety/management; physical, IT and network security; **(direct) marketing** and other advertising; unsolicited non-commercial messages (including political campaigns or charitable fundraising); **exercise of freedom of expression or information**, including in the media and the arts; scientific research; and historical, scientific or statistical purposes.

> **The 3-step / LIA test** (sketched in the lecture): (1) **Purpose test** — identify the legitimate interest (is it a real interest? does it raise ethical issues? is it lawful?); (2) **Necessity test** — is the processing necessary for that identified interest, and could it be achieved with less data? (3) **Balancing test** — weigh the interest against the data subject's interests, rights and reasonable expectations (nature of the data, whether it is sensitive, the relationship with the subject, possible impact, safeguards). The controller must document the reasoning that justifies proceeding.

_Example (freedom of expression)._ I may use the name "Mr Mario Rossi" when expressing a legitimate opinion about him (without defamation) — this is a legitimate interest. The same applies to **media** giving news on specific subjects, who may use names, surnames and city of residence. I cannot, however, falsely state that he committed a criminal offence — that would be a criminal offence (defamation).

_Example (Google Spain)._ The professor cited a well-known case involving Google Spain and the Spanish data-protection authority, where processing of certain user data was found to be supported by a legitimate interest connected to freedom of expression. _(See "parts to verify".)_

_Example (video surveillance)._ Where there is an **imminent danger** — banks, jewellery shops, petrol stations or other typical crime scenes for property offences — using **cameras/CCTV** is legitimated by legitimate interest. Images are personal data, but I may capture them to prevent dangerous situations without another ground. **But a balancing is always required** (Art. 29 WP): if I capture images of someone in their private life (e.g. with a person other than their spouse) I am not preventing any danger and I infringe their rights, so the images must be **immediately destroyed**; if I capture someone attempting a theft in a bank, I may use the images.

_Example (copyright / IP addresses)._ The professor referred to cases where **phonogram producers** sought, from telephone/internet companies (an Italian and a Spanish case), the **IP addresses** of users who allegedly infringed copyright. Although the producers had a legitimate interest in knowing who infringed their rights, the **balancing favoured the users' right to private life over copyright protection**, and the companies refused / were not required to communicate the IP addresses. _(Case names garbled in the transcript — see "parts to verify".)_

### 6.4 Consent

Often wrongly treated as the most important basis. **Misconception warning:** if you are already entitled to process on the basis of **contract** or **legitimate interest**, you must **not** ask for consent "just to be safe." Asking for consent where another basis applies is treated by data-protection authorities as a **mistake** and a breach of the **privacy-by-design** principle, because it shows you have not correctly identified your lawful basis.

Consent is the lawful basis when the data subject has **given consent** (Art. 7.1). To be valid, consent must be (Art. 4.11): **freely given, specific, informed, and unambiguous.**

#### Consent must be freely given

There must be a genuine choice to agree or not.

- **No bundling:** consent cannot be made a condition for providing a contract or service, because "consent" and "contract" are **two distinct lawful grounds**.
- **Granularity:** individuals must be free to choose which specific processing activities/purposes they consent to and which they do not. When you sign up for a bank account, a phone/internet contract, etc., you typically tick **separate** consent lines (e.g. promotional material from the company; transfer of data to third parties; profiling). The controller must **separate the purposes** and obtain consent for each.
- **No detriment:** the person must not be harmed for refusing.

_Example (real-estate phone number)._ The agency asks for the client's phone number to call if the agent is late for the apartment viewing — that is the **specific** purpose. If the agency's real intention is to **sell those numbers** to renovation companies for marketing, it needs the clients' **specific and free consent** to transfer the numbers. Without it, the transfer is unlawful, because consent was given only to be contacted in case of lateness.

_Example (detriment via insecure handling)._ If you write the client's number on a piece of paper and leave it on a shared desk used by ten people, the processing is done without any security measure — a possible **detriment** to the data subject.

_Example (profiling and equal treatment)._ An online retailer asking visitors to consent to profiling of their online behaviour **may not punish** those who refuse — e.g. by giving them a worse price or less favourable conditions than users who consented. That would be a detriment.

#### Cookies and paywalls

A **cookie wall** lets a user access a website **only** by consenting to all cookies. The **EDPB** has held this is **not** freely given consent: access to services and functionalities must not be made conditional on consenting to the storing of, or access to, information in the user's terminal equipment. A **paywall** offers an alternative (payment or subscription) instead of consenting to cookies — raising the question whether consent "as an alternative to paying" is truly free.

_Example (Italian newspapers, October 2022)._ Major Italian newspapers/magazines (Repubblica, La Stampa, etc.) showed non-subscribers a message requiring them either to **pay an annual subscription** or to **accept all cookies (including profiling/tracking cookies)** for personalised advertising, in order to read full articles. The Italian Garante's position is that consent obtained this way is **not freely given**. The professor noted that the **Austrian and French** authorities have so far treated cookie-paywall ("pay or consent") schemes as valid, though users have started challenging those decisions. _(See "parts to verify" on the exact status.)_

#### Specific consent

Consent must be for a **specific purpose**, never a general authorisation to process "for any purpose." A consent request must be specific, **separated** from information on other matters, prominent, and **not hidden** behind lengthy terms and conditions.

#### Informed consent

The data subject must receive the information needed to make a meaningful choice (Art. 7.2). Information to provide:

- identity of the **controller** and **purposes** of the processing;
- **type of data** collected and used;
- existence of the **right to withdraw** consent;
- if data is **transferred to / processed by other controllers** relying on the same consent, those controllers must be **named**;
- the fact that the information is part of the **privacy policy**.

How to provide it: the request must be **intelligible, easily accessible, in clear and plain language** (Art. 7.2), considering the audience. "Easily accessible" also means the signed information (or the information given for performing the contract) must remain **easily findable** on the website.

#### Unambiguous consent

Consent needs a **statement or a clear affirmative action**.

- **Do:** require **ticking an un-ticked box**; let individuals actively choose technical settings (e.g. essential vs. non-essential cookies) or other conduct clearly indicating acceptance.
- **Don't:** interpret **silence or inactivity** as consent; use **pre-ticked boxes**. These are unlawful under the GDPR.

#### Drafting and withdrawing consent

An **informed-consent form** (Italian _informativa privacy_) is a document setting out specific information about the data processed and for which consent is required.

**Withdrawal (Art. 7.3):** the data subject may withdraw consent **at any time**, and **withdrawal must be as easy as giving consent** (no undue effort). Withdrawal does **not** affect the lawfulness of processing done **before** withdrawal — processing is lawful up to that moment and becomes unlawful only if the controller keeps processing afterwards. The subject must be informed of the right **before** giving consent. _Example:_ if consent was given by ticking a box on a website, it cannot be required that withdrawal happen by **calling a call centre** — that is not "as easy."

When consent is withdrawn (or no longer valid), data processed on that basis must be **deleted immediately**, **unless** another lawful basis now applies (e.g. legitimate interest or a legal obligation). _Example:_ a customer gives consent for the bank to use financial data, then withdraws it — but the bank may continue processing under **AML legal obligations**, no longer on the consent basis.

**Records:** the controller must keep records proving consent — **who** gave consent, **when**, **how**, and **what exactly** the individual agreed to — and records of any **withdrawal/deletion requests**.

### 6.5 Other grounds (Art. 6)

- **Vital interests** (Art. 6.1.d): processing necessary to protect the vital interests of the data subject or another natural person — e.g. an emergency ward processing a patient's data to protect their life.
- **Public interest / official authority** (Art. 6.1.e): processing necessary for a task carried out in the public interest or in the exercise of official authority vested in the controller — e.g. activities carried out by the police.

---

## 7. Justification grounds for sensitive data (special categories)

### General prohibition (Art. 9.1)

There is a distinction between **'regular' personal data** and **special categories**, which need extra protection. Processing of data revealing **racial or ethnic origin, political opinions, religious or philosophical beliefs, or trade-union membership**, plus **genetic data, biometric data** used to uniquely identify a person, **data concerning health**, or data concerning a person's **sex life or sexual orientation**, is **in principle prohibited**. **Art. 10** separately covers **criminal convictions and offences**.

### Lifting the prohibition (Art. 9.2)

The prohibition is lifted only if — **in addition to** a lawful basis under Art. 6 — **one** of the specific conditions exhaustively listed in **Art. 9.2** is met. Before processing sensitive data, the controller must **determine which ground applies and document it**; otherwise the processing is unlawful.

Grounds it is commonly advisable to rely on (Art. 9.2):

- **Explicit consent** of the data subject, indicating that they understand and agree that special categories will be processed (unless prohibited under national law). This is a **stricter** form of consent — it must still be specific, informed, unambiguous, etc., and in practice (the professor estimated "around the rule rather than the exception" for sensitive data) is usually obtained in **written form**, because the **evidence** of consent must be clear.
- **Employment, social security and social-protection law**, with appropriate safeguards for the data subject's fundamental rights.
- **Establishment, exercise or defence of legal claims**, or where courts act in their judicial capacity — the work of lawyers and judges. _Example:_ if I sue a tenant who is not paying rent, I will process their data to defend my client's rights; otherwise I could not defend those interests.
- **Individual health-care purposes.**
- **Preventive or occupational medicine**, assessment of the employee's working capacity, medical diagnosis, provision of health/social care or treatment, or management of health/social-care systems and services. Here the interest is broader/community-wide; you may not know in advance the names of the subjects (e.g. preventive medicine), but the resulting data may then be processed because it serves the community of employees.
- **Data manifestly made public by the data subject.** _Example:_ if I publish sensitive data about myself (e.g. sexual orientation, or a disease) on my own Instagram/Facebook account, I render it public, and consent is no longer required to process it.
- **Substantial public interest** (Art. 9.2, letter g): must be **proportionate**, respect the essence of the right, and provide suitable safeguards; the public interest must be real and grounded in law, with a balancing against each subject's privacy.

### Check national law

Member States **may maintain or introduce further conditions, including limitations**, regarding the processing of **genetic data, biometric data or data concerning health** (last paragraph of Art. 9). So when processing special categories, you must **check the national law** of the country where the processing takes place, as it may impose **stricter** requirements than the GDPR baseline.

---

## 8. The subjects involved in processing

> Another recurring exam point. The subjects regulated by the GDPR are: the **data subject** (the protected person), the **controller**, the **processor**, plus the **recipient** and **third party**. (The supervisory bodies — national DPA / Garante, and the European Data Protection Board — are separate.)

### Controller vs. processor — definitions

- **Controller (Art. 4.7):** the natural or legal person, public authority, agency or other body which, alone or jointly with others, **determines the purposes and means** of processing. In Italian: _titolare del trattamento_.
- **Processor (Art. 4.8):** the natural or legal person, etc., which **processes personal data on behalf of the controller**. In Italian: _responsabile del trattamento_.

The role matters because it determines the **legal obligations** borne and the resulting **responsibilities**. The controller has the **most responsibility** and, before the **data subject**, is ultimately responsible — even for problems caused by the processor. In the **mutual** relationship, however, a processor that causes damage is liable **towards the controller**.

### Recipient and third party (Artt. 4.9–4.10)

- **Recipient:** a natural or legal person, public authority, agency or another body **to which personal data are disclosed** — whether or not a third party. It is neither the data subject, nor the controller, nor the processor; it is the party receiving a communication containing data, and the GDPR governs what it must do on receiving the data.
- **Third party:** a natural or legal person, public authority, agency or body **other than** the data subject, controller, processor, or persons authorised to process under the direct authority of the controller/processor. There are specific cases in which third parties can process personal data.

### Demonstrating compliance (Art. 24)

Taking into account the nature, scope, context and purposes of processing, and the risks to natural persons' rights and freedoms, the controller must implement **appropriate technical and organisational measures** and be able to **demonstrate** that processing complies with the GDPR; the measures must be **reviewed and updated** where necessary. The controller is the subject bearing the **largest amount of compliance responsibility**.

### How to tell controller from processor — the questions (slide 44)

The more "**yes**" answers, the more likely you are a **controller** (more "no" → processor):

- Did you take the **initiative** to start collecting/processing the data?
- Did you decide the **purpose** of the processing?
- Did you decide **which types** of personal data are collected and **from which** individuals?
- Are you **giving instructions** to another entity, rather than following someone else's?
- Are you the party **mainly benefiting** from the result?
- Do you have a **direct relationship** with the data subject?
- Do you **monitor** other entities' execution of the service?

_Example (the lawyer)._ When a lawyer processes the data of the opposing party to defend a client, is the lawyer a controller or a processor? Running through the questions: the lawyer **takes the initiative**, decides the **purpose**, decides **which data** are needed, and does **not** simply follow the client's instructions (e.g. drafting the defence autonomously) — even though the lawyer does **not** mainly benefit and has **no direct relationship** with the opposing data subject. With more "yes" than "no" answers, the lawyer acts as an **autonomous data controller**, not a processor entrusted by the client.

### The unclear border — case law (EDPB Guidelines 7/2020)

- _CJEU C-210/16, 5 June 2018, Wirtschaftsakademie Schleswig-Holstein:_ the **administrator of a Facebook fan page** is a **controller** (not a mere processor following users' instructions).
- _CJEU C-40/17, 29 July 2019, Fashion ID v. Verbraucherzentrale NRW:_ by embedding the Facebook **"Like" button**, Fashion ID made it possible for Facebook Ireland to obtain visitors' personal data as soon as a visitor consulted the site; Fashion ID was a **controller (jointly with Facebook Ireland)**.
- _CJEU C-25/17, Jehovan todistajat:_ a **religious community is a controller jointly with its members** who engage in door-to-door preaching, even without the community having access to the data or having given written instructions. The members had argued they were "just processors"; the Court held that the way they **collected and accessed** the data was an autonomous processing, making them **joint controllers** with the community (which was held responsible for the unlawful collection).

### Joint controllers (Art. 26.1)

Where two or more controllers **jointly determine** the purposes and means, they are **joint controllers**. They must, in a **transparent** manner, determine their respective responsibilities (typically via an **arrangement/agreement**, unless responsibilities are set by law), and should designate a **contact point** for data subjects. **Joint controllership implies joint liability.**

Questions indicating joint controllership — "yes" points to a joint controller:

- Do you have a **common objective and purpose** with other parties regarding the processing?
- Are you using the **same set** of personal data as another controller?
- Have you **designed the process** with another controller?
- Do you have **common information-management rules** with another controller?

### Obligations of the controller (overview)

Comply with the GDPR through appropriate technical/organisational measures (especially **data protection by design and by default**, which is the controller's duty even where a processor acts); keep **records of processing activities**; handle **data breaches** (notify the DPA within **72 hours** of becoming aware, and notify affected individuals without undue delay); **demonstrate compliance** and keep it updated when processing changes; **choose an adequate processor** (one giving sufficient guarantees, appointed via a **Data Processing Agreement / DPA**); **ensure security of processing** matching the specific risk; carry out a **DPIA** (Data Protection Impact Assessment) where required; **appoint a DPO** where mandatory (e.g. large-scale processing) or voluntarily; and **cooperate with the supervisory authority** (in Italy, the _Garante per la protezione dei dati personali_).

### Obligations of the processor (overview)

Keep **records of processing activities** (unless an exemption applies — this also **protects** the processor in case of a breach); **ensure security of processing** to the level of the specific risk (this duty passes from controller to processor on appointment); operate under a **Data Processing Agreement**; **notify the controller** of any data breach (so the controller can in turn notify the subject and the DPA); be involved in the relationship with the **DPO** where one is designated; and generally **assist the controller** in fulfilling its GDPR obligations.

**Processor acting on instructions:** a processor may only process **in accordance with the controller's instructions**, except where it must process under a **legal obligation, vital interest**, etc. If it **disobeys** the controller's instructions, the processor is **re-qualified as a controller** and then bears full responsibility **before the data subject** too.

**Liability chain:** if a processor causes a **breach**, it becomes legally liable. The data subject or supervisory authority will claim **compensation and damages from the controller** (the last responsible before the data subject); the controller pays and then **recovers** the damages from the processor.

### Data Processing Agreement (DPA)

A DPA is a **legally binding** document between controller and processor, **in writing or electronic form**. It governs the specifics of the processing (which data, for which purpose, on which ground, etc.) and the controller–processor relationship, including rights and obligations. **Failure to enter a DPA where required risks fines.** _The detailed rules on processors (Artt. 28–29) and sub-processors (Artt. 28.2 and 28.4) will be examined in the next class._

