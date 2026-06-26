## 1. Recap of GDPR Background

The lecture opens by recalling the GDPR framework already covered: what the GDPR is, when it was implemented, the difference between a **directive** and a **regulation (GDPR)**, the role of the Court of Justice of the European Union, the scope of the GDPR, what constitutes personal data, the difference between ordinary personal data and **sensitive data (special categories)**, what does and does not constitute data processing, and the territorial criteria for application — including the distinction between the **establishment criterion** and the **targeting criterion** (the GDPR applies to the processing of data of subjects based in the EU even when the enterprise is not based in the EU).

---

## 2. Who Is Protected by the GDPR (the Data Subject)

The GDPR protects **natural persons** with regard to the processing of personal data and the rules relating to the free movement of personal data. Names of companies or enterprises are **not** protected, unless those names are connected to specific natural persons representing the company.

_Example: the Politecnico email addresses._ 
If I write an email to `rettore@polito.it`, there is no GDPR issue, because I am not processing the data of a natural person — the address refers to a role/institution. On the contrary, if I write to `stefano.corgnati@polito.it`, I am processing the family name of a natural person (the director), and therefore I must respect the GDPR rules.

### Definition (Article 4.1)

Personal data means **any information relating to an identified or identifiable natural person**. An identifiable natural person is one who can be identified **directly or indirectly**, in particular by reference to an identifier such as a name, an identification number, location data, an online identifier, and so on.

---

## 3. Lawful Grounds for Processing Personal Data

This is presented as the most important issue for the exam. For **any** processing activity it is necessary to identify a valid ground — the **lawful basis** — to justify the collection, use, or any other form of processing. Without a ground, personal data cannot be processed. In practice, if a company processes data on the basis of one listed ground, it usually does not also rely on a different ground for the same activity.

> **Exam warning:** do not confuse the **principles** of the GDPR (and the rights of the data subject) with the **justification grounds**:
> -  **Principle:** all the rules that say "how" the data have to be treated (all have to be respected)
> - **Lawful Grounds:** it says "why" I can treat the data (only 1 needed) 

The **six lawful bases** under **Article 6** are: 
- **contract**
- **legal obligation**
- **legitimate interest**
- **consent**
- **vital interest**
- **task carried out in the public interest / exercise of official authority.**

### 3.1 Contract

Personal data may be processed when this is necessary to comply with a **contractual obligation**. This is very common in company activity: companies process personal data in order to perform contracts.

_Example: online sales._ 
A company selling goods on a website must process the client's data in order to deliver the goods purchased; without processing that data, the contractual obligation cannot be performed.

Contract can also be a lawful basis for handling **pre-contractual requests** from potential clients.

_Example: a home-repair quote._ 
When a potential client asks a company for an estimate to paint a house, no contract has yet been executed. Yet to issue the estimate the company needs at least the subject's name, family name, and email in order to send the quote. Here contract is the lawful basis.

**Strict interpretation.** Contractual necessity must be interpreted **strictly, not broadly**: it must be demonstrated that the processing is necessary to perform/execute the contract or to address the pre-contractual request. Outside that situation, the basis cannot be stretched to other uses.

_Example: data not necessary for the quote._ 
If, for the painting estimate, I collect the client's name and email (necessary) but also the client's **date of birth**, that extra data is not necessary to send the estimate. The contract basis cannot be spread to cover it.

### 3.2 Legal Obligation

Processing is lawful when it is necessary to comply with a **legal obligation** to which the controller is subject (e.g. obligations imposed by law such as accounting, tax, or anti-money-laundering requirements). [The lecture develops legal obligation and legitimate interest alongside the contract example as listed Article 6 grounds.]

### 3.3 Legitimate Interest

Processing may be based on the **legitimate interest** pursued by the controller or by a third party, provided this interest is not overridden by the interests or fundamental rights and freedoms of the data subject — i.e. a **balancing** is required.

### 3.4 Consent

Consent is one of the six lawful bases, treated in detail in the next section.

---

## 4. Consent

### 4.1 Requirements of Valid Consent

Consent must be **freely given, specific, informed, and unambiguous**. The request for consent, when given in the context of a written declaration that also concerns other matters, must be **clearly distinguishable** from the other matters, in an **intelligible and easily accessible form, using clear and plain language** (Article 7.2). Any part of such a declaration that infringes the regulation shall **not be binding**.

### 4.2 Information to Be Provided (Informed Consent / _informativa privacy_)

To obtain informed consent, the data subject must be given at least:

- The **identity of the controller** and the **purposes** of the processing.
- The **type of data** that will be collected and processed.
- The existence of the **right to withdraw** consent.
- Whether the data will be **transferred to third parties / other controllers or processed by a processor** — if I, as controller, will not process the data directly but will appoint a processor, I must state who will process it.
- The fact that this information is part of the **privacy policy**.

All of this serves the **transparency principle**, one of the main principles of the GDPR.

### 4.3 How Information Must Be Provided

The information must be **intelligible, easily accessible, and in clear and plain language** (Article 7.2), and it is essential to consider the audience.

_Example: the* informativa privacy.* 
In Italian practice, people sign an_ informativa privacy *many times. It should not be written in difficult legal wording but in clear, plain language, and it must remain **easily accessible** — e.g. after an online sale, the signed information must always be findable on the website, so the user can always retrieve what they signed (or, where the information was given to perform a contract, can still access it).

### 4.4 Unambiguous Consent

Consent must be obtained by a **statement or a clear affirmative action**.

**Valid (unambiguous):** requiring the user to **tick an unticked box**; actively choosing technical settings (e.g. for cookies — essential vs. non-essential); or other conduct that clearly indicates acceptance of the specific proposed processing.

**Invalid:** interpreting **silence or inactivity** as consent (never allowed), or using a **pre-ticked box**. These are illegal under the GDPR because they are clear evidence that consent was not unambiguously given.

### 4.5 Withdrawal of Consent (Article 7.3)

The data subject has the **right to withdraw consent at any time**, and withdrawal must be **as easy as giving consent** (it should not require a new effort). The data subject must be informed of this right **before** giving consent.

Withdrawal does **not** affect the lawfulness of processing carried out **before** withdrawal: up to the moment of withdrawal the processing is lawful; it becomes unlawful only if the controller continues processing **after** the withdrawal is exercised.

_Example: the real-estate agency._ 
I gave consent for a real-estate agency to use my number to arrange apartment-viewing appointments, but I no longer agree and do not want them to keep processing my data — I can withdraw the consent.

_Example: withdrawal must use equally easy means._ 
If I gave consent by ticking a box on a website, I must be able to withdraw equally easily (e.g. on a website); forcing me to phone a call center to withdraw is **not** as easy as giving consent, and is not GDPR-compliant.

After withdrawal, data processed on the basis of consent must be **deleted immediately**, because the consent is no longer valid — **unless** another lawful basis applies from that moment on.

_Example: bank and anti-money laundering._ 
A subject gave consent for a bank to use financial data, then withdraws it. If processing has meanwhile become mandatory under anti-money-laundering legislation, the bank no longer processes on the basis of consent but on the basis of **legal obligation**.

### 4.6 Records / Proof of Consent

Controllers and processors should keep **records (evidence)** of consent — both when consent is obtained and when a withdrawal/deletion request is received. Companies must be able to prove that consent was validly obtained, recording: **which individual** gave consent, **when** it was obtained, **how** it was obtained, and **to what exactly** the individual agreed.

---

## 5. Other Justification Grounds: Vital Interest and Public Interest / Official Authority

Beyond contract, legitimate interest, and consent, personal data may be processed when necessary to protect the **vital interest** of the data subject or of another natural person, or when processing is necessary for the **performance of a task carried out in the public interest or in the exercise of official authority** vested in the controller (**Article 6.1(d) and 6.1(e)**).

_Example: the ambulance._ 
An ambulance protecting the vital interest of a person with a disease may process all the data needed, including health data, to protect that vital interest (likewise for protecting safety).

_Example: the police._ 
Activities carried out by the police are an example of processing necessary for the performance of a task in the public interest or exercise of official authority.

Importantly, when processing is needed to protect a **vital interest** or to perform duties in the **public interest**, the special restrictions of **Article 9 on sensitive data do not apply** — a public authority or other subject may, in these cases, process even **sensitive data** without the specific permission otherwise required.

---

## 6. Processing of Special Categories of Data (Sensitive Data)

### 6.1 General Prohibition (Article 9.1)

The processing of **special categories of personal data is, as a rule, prohibited**. Article 9.1 lists data revealing **racial or ethnic origin, political opinions, religious or philosophical beliefs, trade union membership**, as well as **genetic data, biometric data** processed to uniquely identify a natural person, and data concerning a person's **health, sex life, or sexual orientation**.

### 6.2 Lifting the Prohibition (Article 9.2)

The prohibition is lifted only when, **in addition to a lawful basis under Article 6**, one of the conditions in **Article 9.2** is satisfied. The conditions discussed:

- **(a) Explicit consent.** The data subject gives **explicit consent** to the processing of those sensitive data for one or more specified purposes. The consent must follow all the usual rules (freely given, unambiguous, specific, etc.).
- **(b) Employment / social security / social protection.** Processing necessary to carry out the obligations and exercise specific rights of the controller or data subject in the field of employment, social security, and social protection law, where authorized by national law or a collective agreement with appropriate safeguards. The rationale: these activities are conceived in the **exclusive interest of the employees**, not of the employer. _Example: the employer (controller) may communicate the necessary employee data to an external consultant/professional appointed to prepare the **payroll**._
- **(c) Vital interest where consent is impossible.** Processing necessary to protect the **vital interest** of the data subject or of another person where the data subject is **physically or legally incapable of giving consent** — again, in the interest of the data subject, never of the controller.
- **(e) Data manifestly made public by the data subject.** _Example: publishing one's own sensitive data — e.g. data on sexual orientation, or news about one's own disease/health — on a personal Instagram or Facebook account._ This works as a sort of consent: since the data is rendered public by the subject, consent is not required to process it.
- **(f) Legal claims and courts.** Processing necessary for the **establishment, exercise, or defense of legal claims**, or whenever courts act in their judicial capacity (the work of lawyers and judges). _Example: if I sue a tenant who is not paying rent, I will process that subject's data to bring the case before a judge; likewise the judge processes the data of the parties to the dispute._
- **(g) Substantial public interest.** Processing necessary for reasons of **substantial public interest**, which must be **proportionate**, respect the essence of the right to data protection, and provide suitable and specific safeguards for the fundamental rights and interests of the data subject. The public interest must be **real and existing**, and a **balancing** with the data subject's interests is required.

---

## 7. Obligations of the Controller

The controller must be able to **demonstrate compliance** — i.e. show that the measures and steps taken are effective. Demonstrating compliance also means that, when the situation or the processing changes, the controller must **review, update, modify, and amend** the policy in line with the new measures adopted to remain GDPR-compliant.

### 7.1 Choice of the Data Processor

If a controller works with a **processor**, it has the duty to choose subjects that provide **sufficient guarantees** of respecting GDPR requirements. For this reason a processor is appointed through a **Data Processing Agreement (DPA)**, in which the controller specifies all the duties, responsibilities, and obligations to which the processor is subject. Without this, the controller could not demonstrate that it took all necessary measures — including in the choice of a person able to provide guarantees in the processing of data.

### 7.2 Security of Processing

The controller must implement appropriate **technical and organizational measures** corresponding to the specific **risk** of each case.

### 7.3 Further Controller Duties

- Carry out, under certain circumstances, a **Data Protection Impact Assessment (DPIA)**.
- Appoint a **Data Protection Officer (DPO)** where required — **mandatory** for big companies processing large amounts of data. Other companies that do not process large amounts of data may still appoint a DPO voluntarily, to be safer.
- **Cooperate with the data protection authority** — in Italy the _Garante per la protezione dei dati personali_ (privacy/data-protection guarantor); each EU Member State has its own data-protection authority.

---

## 8. Obligations of the Processor (Articles 28–29)

- **Records of processing activities.** The processor must keep records documenting **how, for which purposes, and on which grounds** it processes personal data (subject to specific exemptions, to be examined later). This is framed both as an **obligation** and as an **instrument that protects the processor**: in case of a data breach, having recorded that all necessary measures were taken can help the processor defend itself.
- **Security of processing.** The processor must implement appropriate **technical and organizational measures** corresponding to the specific risk. This obligation, originally on the controller, is **transmitted to the processor** upon appointment.
- **Data Processing Agreement.** The delegation of processing must be arranged **contractually**, through the DPA executed between controller and processor.
- **Notification of data breaches.** The processor must **notify data breaches to the controller**, because the controller is then obliged to notify the affected data subjects and the data-protection authorities.
- **Relationship with the DPO.** Where a DPO is appointed, the processor is also involved in the designation and in the relationship with the DPO; the processor must **assist and inform the controller** in fulfilling its GDPR obligations, including in case of a breach or any other relevant event.

### 8.1 Acting Only on the Controller's Instructions

The processor can process data **only in accordance with the instructions given by the controller**, except in the cases already seen where processing is necessary for performing an obligation, protecting a vital interest, and so on (where the controller's instructions are not needed).

**Re-qualification as controller.** If the processor does **not respect** the controller's instructions, the processor is **re-qualified as a controller** and thereby becomes responsible **also before the data subject**. This holds even where no problem arises: simply not following the controller's indications turns the processor into a controller before the data subject, assuming all the controller's responsibilities. This is presented as a very important principle to remember.

### 8.2 Chain of Responsibility (Controller vs. Processor)

Before the data subject, the **last responsible party is always the controller**, because the data subject communicated the data to the controller. The processor is **not directly responsible before the data subject** or the supervisory authority (so long as it stays within instructions); its responsibility runs toward the **controller**.

Therefore, if a breach occurs: data subjects or the supervisory authority claim **compensation and damages from the controller**; the controller pays; and the controller then **recovers those damages from the processor** if the breach was caused by the processor. The chain must be clear: the controller answers externally, then turns to the processor internally.

> The rules on the processor are contained in **Articles 28 and 29**. Joint controllers and the processor will be examined further in the next class. The lecture stops at **slide 50**.