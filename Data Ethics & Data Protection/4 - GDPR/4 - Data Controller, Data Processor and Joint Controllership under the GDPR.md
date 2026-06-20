## 1. The Data Processor

### Definition and basic rule

A processor can **only** process data following the **instructions given by the controller**. The processor has **no autonomous power** to determine the means, ways or characteristics of the processing — this is forbidden. The processor must always act according to the controller's instructions.

The key consequence of breaking this rule: if the processor does **not follow the controller's instructions**, the processor is **re-qualified as a controller**. From that point, if the (former) processor is responsible for a breach, it will be **liable as a controller**, notwithstanding the supervision the controller had over it. In any case, failure to follow instructions always triggers the processor's responsibility towards the controller.

The relevant provisions are **Articles 28 and 29 GDPR**.

### Sufficient guarantees (Art. 28.1)

Article 28 regulates the processor as the subject appointed by the controller. **Article 28.1** states that, where processing is to be carried out on behalf of the controller, the controller shall use **only processors providing sufficient guarantees** to implement appropriate technical and organizational measures, so that processing meets the requirements of the Regulation and ensures the protection of the rights of the data subject.

Practical meaning: a controller can appoint a processor **only if** the processor offers sufficient guarantees that it is able to implement all the measures required by the GDPR. Otherwise the responsibility for having appointed an unsuitable processor falls on the **controller**. The controller must therefore verify, be aware of, and have knowledge about the competencies of the processor and its ability to carry out the activities entrusted to it.

### Processing only on instructions (Art. 29)

**Article 29** establishes that the processor — and any person acting under the authority of the controller or of the processor who has access to personal data — **shall not process those data except on instructions from the controller**.

---

## 2. The Appointment of the Processor (Art. 28.3)

The processor is **appointed by the controller**. Under **Article 28.3**, processing by a processor shall be governed by a **contract or other legal act** binding on the processor with regard to the controller.

This contract must set out:

- the **subject matter** and **duration** of the processing;
- the **nature and purpose** of the processing;
- the **type of personal data** and **categories of data subjects**;
- the **obligations and rights of the controller**.

### Mandatory content of the agreement (the list in Art. 28.3)

The contract must in particular stipulate that the processor:

- **Documented instructions** — processes the personal data only on documented instructions from the controller, including with regard to **transfers to a third country or international organization**. If the processor transfers data outside the EU in order to process data communicated by the controller, it is **obliged to inform the controller** of that legal requirement before processing.
- **Confidentiality** — ensures that persons authorized to process the data have committed themselves to confidentiality or are under an appropriate statutory obligation of confidentiality. If the processor needs to appoint other subjects, those subjects must be bound by a **contractual or statutory secrecy obligation**.
- **Sub-processors** — respects the conditions for engaging another processor.
- **Assistance with data subject rights** — taking into account the nature of the processing, assists the controller, by appropriate technical and organizational measures, in fulfilling the controller's obligation to **respond to requests for the exercise of data subject rights**. Where the controller does not directly process the data (e.g. a data subject asks for modification or update of the data, and these activities are carried out by the processor), the processor must have full knowledge of the processing and cooperate on the specific point with the controller.
- **Assistance with compliance (Art. 32–36)** — assists the controller in ensuring compliance with the obligations under Articles 32 to 36, including the **Data Protection Impact Assessment (DPIA)**. The processor is bound to cooperate so the controller can carry out a correct DPIA. _(Prof. Mantellero had already explained the DPIA to the class.)_
- **Deletion/return of data** — at the controller's choice, deletes or returns all personal data to the controller **after the end of the provision of services** and deletes existing copies, unless storage is required by law. When the appointment terminates for any reason, the processor must delete everything connected to the appointment; no data may be retained unless specific legislation allows it.
- **Audits** — makes available to the controller all information necessary to demonstrate compliance, and contributes to audits and inspections conducted by the controller or by another auditor mandated by the controller. If an audit is carried out by the controller or by the authorities, the processor must cooperate and provide any clarification, demonstrating that the requirements of the GDPR are respected.

---

## 3. Sub-Processors (Art. 28.2 and 28.4)

Sometimes the processor cannot carry out all the processing activities by itself, so it is common for a processor to appoint one or more **sub-processors**. This is regulated by **Articles 28.2 and 28.4**.

### Authorization (Art. 28.2)

The processor **shall not engage another processor without prior specific or general written authorization** of the controller. Without this permission, appointing a sub-processor is not possible. The authorization may be:

- **Specific** — e.g. the processor needs to appoint "Mr. Mario Rossi" as a sub-processor for specific processing activities, and the controller gives a specific authorization for that person and those activities.
- **General** — the contract contains a generic provision allowing the processor to appoint the sub-processors it deems necessary and appropriate to carry out its processing activities.

Even where a general authorization exists, the processor may appoint **only sub-processors having the same characteristics**, i.e. able to respect the GDPR.

### Same obligations and full liability (Art. 28.4)

Under **Article 28.4**, where a processor engages a sub-processor to carry out specific activities on behalf of the controller, the **same data protection obligations** set out in the contract must be imposed on the sub-processor, by way of a contract or other legal act — in particular, providing sufficient guarantees to implement appropriate technical and organizational measures meeting the requirements of the Regulation.

Consequence in case of breach: where the sub-processor fails to fulfil its data protection obligations, the **initial processor remains fully liable** to the controller for the performance of the sub-processor's obligations.

_Example: His Web case (Garante per la Privacy, 2022)._ His Web was a web-application provider that entered into a contract with a **group of hospitals** to create an application for collecting and managing **whistleblowing reports** concerning employees. To perform the contract, His Web engaged another company, **C-Web**, to **host** the application. His Web did **not** ask for the prior written authorization from the group of hospitals (the controller), nor did it formalize any relationship with C-Web beyond the hosting contract. The hospitals claimed a breach of the Regulation and of the agreement. His Web responded that the object of the contract was only the provision of an **infrastructure**, that neither it nor C-Web ever processed identifying data, since the whistleblower reports were **encrypted** with a key available exclusively to the hospitals, and that it therefore did not need authorization nor to impose the obligations on its sub-processor. The Italian authority held this argument was **not correct**, because personal data of the persons involved in the whistleblowing reports were in any case present on the server with His Web. His Web was fined **€40,000** for the violation of **Article 28.2 GDPR**.

---

## 4. Joint Controllership (Art. 26)

There are situations in which there can be **more than one controller** — this is **joint controllership**, regulated by **Article 26 GDPR**. It arises where the parties **jointly determine the purposes and means** of processing.

### Joint controllership agreement

In cases of joint controllership it is usually necessary to draw up an **agreement** that clearly specifies the **responsibilities, the division of duties and the tasks** of each controller, and to **designate a contact point** for the data subject.

### Joint liability (key point)

Regardless of the agreement, joint controllership always implies **joint liability**. Vis-à-vis the data subject, **each controller is fully liable** for any breach. A damaged data subject does **not** have to check the internal agreement to decide which controller is responsible — both are responsible, and the data subject can bring a claim against **either one** of them.

Internal settlement follows afterwards: the controller who paid damages to the data subject can, according to the joint controllership agreement, claim reimbursement from the controller that was actually responsible for the breach.

_Example: the lawyer processing the opposing party's data._ When a lawyer's client does not get paid, the client communicates the **financial data of the opposing party** to the lawyer. The client (as controller) can communicate that data on the basis of its **legitimate interest**. The lawyer then needs to process the opposing party's financial data to draft the brief and prepare the defenses in the lawsuit, **without** asking the opposing party's consent. The justification ground the **lawyer** relies on is **not** the client's legitimate interest and **not** the client's instructions (the client does not know how to draft a brief or prepare defenses) — it is the **exercise of authority / processing in the context of legal proceedings**. Because the lawyer processes the data autonomously and not under the client's instructions, the lawyer is considered a **separate controller**, producing a case of **joint controllership**.

_Example: the two doctors._ You go to a first doctor for a specific problem (e.g. a hand or a recurring headache). You give that doctor your sensitive data, and the doctor collects it and asks for consent for sensitive data. But that doctor lacks the competence for your specific case and refers you to a **second, specialized doctor**. The second doctor is an **autonomous controller** of your data — **not** a processor. This is again **joint controllership**, requiring a **joint controllership agreement** specifying the contact point for the data subject, the duties, tasks and responsibilities.

### Example of a Joint Data Processing Agreement (pharmaceutical sector)

The professor showed a template (uploaded on _Portale della Didattica_) to illustrate the typical content. Students do **not** have to memorize the agreement; reading it helps understand the topic. At the exam the professor will ask about the **role and regulation of joint controllership**, and explaining the typical content of the agreement improves the answer.

The structure shown:

- **Premises ("Whereas")** — set out the facts on which the relationship is based. Here, the parties process **personal and job-related data of employees** in relation to the legal obligations of **Good Manufacturing Practices (GMP)** in the pharmaceutical field (regulatory guidelines ensuring pharmaceutical products are consistently produced and controlled to quality standards, covering production, distribution and treatment of raw materials). The data are processed for the **traceability** of pharmaceuticals across research, development and production. Both parties are based in the EU and subject to the GDPR. Because the data are accessible to and used by **both** parties, they **jointly determine purposes and means** and are therefore **joint controllers under Art. 26 GDPR**.
- **Art. 1 – Object and applicable law** — the agreement regulates the respective tasks and responsibilities regarding collection, use and storage of the shared personal data in compliance with GMP. The parties undertake to comply with the GDPR and related Italian national laws; the agreement is subject to **Italian law**. Processing **other than** that needed for GMP obligations is conducted **independently** and is not subject to the agreement.
- **Art. 2 – Information to data subjects** — each party must provide data subjects with comprehensive and accurate information under **Art. 13 GDPR** and obtain consent where necessary. Upon hiring, Alfa's employees receive a privacy notice and give consent at the same time; the notice must refer to **both parties as joint controllers**, so the data subject is aware their data is processed by two controllers.
- **Art. 3 – Exercise of rights** — data subjects may contact **either** party to exercise their GDPR rights, using the contact details provided; the parties must promptly inform each other of communications received.
- **Data processing** — each party adopts an adequate internal organization and designates and trains the individuals responsible for processing (each joint controller having its own processors). If a party delegates to an external processor, it guarantees that only entities offering **sufficient assurances** are engaged.
- **Data breaches** — if either party becomes aware of a breach or loss, it must notify the other **within 48 hours**; the parties cooperate to mitigate or remedy the breach and, if necessary, notify the **supervisory authority** and the **data subjects**.

This is the **minimum content** required. Without such an agreement clearly establishing tasks, duties and responsibilities, it is very difficult for the non-breaching controller to demonstrate that the fault lies with the other controller.

_Example: the Jehovah's Witnesses case (Court of Justice of the EU)._ The Jehovah's Witness community collected personal data (names, addresses) during **door-to-door preaching**, from subjects unknown to them, without consent or knowledge — an **unlawful** processing. Both the individual members and the community were involved in activities aimed at preaching, keeping records of preachers, and distributing publications. The problem was establishing the respective roles of the **preachers** and the **community**. The Court of Justice held that a religious community is a **controller jointly** with the members who engage in preaching, because those members process personal data in the context of a door-to-door activity **organized, coordinated and encouraged** by the community — even though the community could not intervene during the actual collection of data, which was done directly by the members. Although the members acted on the community's guidelines, they processed the data **autonomously** and were therefore **controllers** themselves. This was unfavorable for the members: as controllers they were held **fully responsible**, with all the responsibilities of a controller and not of a processor.

---

## 5. The Data Processing Agreement (DPA)

The appointment of a processor by a controller is made through an agreement — the **Data Processing Agreement (DPA)** — a **legally binding document, in writing or electronic form**. According to the **European Data Protection Board (EDPB)** guidelines, a **verbal/oral** agreement appointing a processor is **not valid**: the **written form is required**.

The professor's practical view: independently of the legal obligation, it is **advisable** to have a DPA whenever a third party (formally or factually a processor) processes data collected by the controller. By signing the DPA, the processor expressly **takes on the responsibilities** set out in it, and the controller can be sure the processor is aware of and accepts the specific conditions for which it was chosen. Not entering into a DPA, where it is required, can expose the controller to **fines**.

The DPA governs the **peculiarities** of the processing: the type of data, the purpose, the legal ground, the relationship between controller and processor, the rights and obligations of both parties. The EDPB wants companies to draft and execute such agreements **reproducing the GDPR provisions** — e.g. setting a specific procedure for the processor to handle an access request, or describing in detail the purposes, nature, storage period, and categories (sensitive or non-sensitive) of data.

### Example of a DPA template (structure shown)

- **Whereas** — the company acts as **data controller** and wishes to subcontract certain services that imply processing of personal data to the data **processor**; reference is made to the GDPR. This agreement is linked to a **principal agreement** under which the processor carries out specific services.
- **Art. 2 – Processing of company personal data** — the processor shall comply with all applicable data protection laws, and not process the company's personal data other than on the company's **documented instructions**; the processor takes all reasonable steps to ensure the reliability of any employee, agent or contractor with access to the data, **limiting access** strictly to those who need to know, and ensuring all such individuals are subject to **confidentiality** undertakings or statutory obligations.
- **Deletion** — on cessation, the processor procures the deletion (or return) of the data and copies, and provides **written certification** that it has complied, within **10 business days** of the cessation date.
- **Audit rights (Section 10)** — the controller's right to **verify** what the processor does with the data. The processor makes available all information necessary to demonstrate compliance and allows and contributes to audits and inspections by the company or a mandated auditor. (Information and audit rights arise to the extent the agreement does not otherwise provide rights meeting data protection law requirements.)
- **Data transfer** — the processor may not transfer or authorize the transfer of data outside the EU / EEA without the **prior written consent** of the company. Where data is transferred from inside the EEA to outside it, the parties must ensure the data is adequately protected, relying — unless otherwise agreed — on **EU-approved Standard Contractual Clauses (SCCs)**.

_Note on data transfers:_ data can be transferred outside the EU only with specific measures that make the processing safe — e.g. agreements between member states, or the use of **Standard Contractual Clauses** in the contract. (The professor will explain SCCs in detail in a later class.)

_Example: the geolocalization case (Garante per la Privacy, 2022)._ **JCG** was the **data controller** (in Italian, _titolare del trattamento_, literally "owner of the processing"). It had a relationship with a processor (Verizon) that provided **geolocalization devices**. The controller installed these devices to **track vehicles** delivering goods. The vehicles were not owned by the controller but by **third companies** to which the controller had outsourced services. The **data subject was a driver** employed by the outsourcing company, with **no direct contractual relationship** with JCG, and there was **no agreement** between JCG and the third parties (the driver's employer), nor a proper agreement covering the processor relationship. Because the driver employed by the third party was tracked by the geolocalization device without the necessary contractual framework, the processing was **unlawful**, and the Italian data protection authority issued a fine of **€50,000** to JCG.

> **Terminology note:** in Italian, the controller is _titolare del trattamento_ — "owner of the processing." The professor finds this clearer than the English "controller," because it conveys that the controller holds **all the responsibilities** connected to the processing.

---

## 6. Data Protection by Design and by Default (Art. 25)

These two principles were mentioned in the first class as being **new** with respect to **Directive 95/46/EC** — the GDPR established them **for the first time**, and they may be considered the **most important principles** of the GDPR.

### The problem under the old Directive

Under the Directive (and, in Italy, the _Codice Privacy_ that implemented it), each company/controller tended to adopt documents **without thinking** about the actual processing, the data involved, or the categories of data subjects. For instance, an Italian company used a **template** for the information notice on personal data and another template for sensitive data — the information was **identical**, applied on a "copy-paste" basis, without assessing case by case whether that information was really needed.

_Example: the PC supplier._ Under the Directive, a supplier/seller of PCs giving clients an information notice referring to **sensitive data** was told this was fine ("you are respecting the Directive and the _Codice Privacy_"). Today this is **not** acceptable: if a PC supplier issues clients a notice asking for **consent to process sensitive data**, then — to the supervisory authority, the EU legislator and the EDPB — this shows the supplier **did not understand** privacy legislation, because it applied a template in a situation where it does **not** process sensitive data at all. The old justification ("it's better to adopt more documents to be safer, so I have everything covered") is now **incorrect**: a PC supplier has to follow only the specific requirements relevant to it, which do **not** include consent for sensitive data.

### Privacy by design

**Privacy by design** focuses on **embedding privacy into the design** of systems and processes. The controller must **adapt all internal policies to the specific situation** of its own company, rather than applying generic templates.

_Example: the professor's own templates._ When the GDPR was adopted in **2018**, the professor (as a lawyer) initially expected to simply rewrite the old Directive-95 templates and reuse the **same** templates for all clients. In practice, this was impossible: each client's situation was completely different, so each set of documents had to be **rewritten entirely** for each client. That is privacy by design — adapting the **framework** provided by the GDPR to the **specific situation** of each company.

The scaling consequence: a **small company** does not have the same privacy-protection needs as a **big company**; and a big company **not** processing large amounts of data does not have the same needs as companies processing large amounts of data such as **Meta/Facebook, Google**, etc. Each must adapt the GDPR requirements to its specific situation.

If the controller does not respect this principle, a supervisory authority inspection will find **evidence** that the controller adopted processes **not matched** to its specific situation — proof that it did not understand its tasks and responsibilities.

### Privacy by default

**Privacy by default** aims to ensure that privacy protections are **automatically in place** for end users. "By default" means **from the beginning** — from the moment the GDPR entered into force for existing companies, or from the moment a start-up is created/constituted — **not** only after a breach occurs. It works **alongside** privacy by design, ensuring that systems automatically collect and use **only the personal data needed**.

### Article 25 GDPR

Both principles are contained in **Article 25 GDPR**, titled _"Data protection by design and by default."_

- **Art. 25(1)** — taking into account the **state of the art**, the **cost of implementation**, and the **nature, scope, context and purposes** of processing, as well as the **risks** (of varying likelihood and severity) to the rights and freedoms of natural persons, the controller shall — **both at the time of determining the means** for processing **and at the time of the processing itself** — implement appropriate technical and organizational measures (such as **data minimization**) designed to implement data protection principles effectively and to integrate the necessary safeguards into the processing.
- **Art. 25(2)** — the controller shall implement appropriate technical and organizational measures ensuring that, **by default, only personal data necessary for each specific purpose** are processed. This obligation applies to the **amount** of data collected, the **extent** of processing, the **period of storage**, and the **accessibility**. In particular, such measures shall ensure that, **by default, personal data are not made accessible without individual intervention to an indefinite number of natural persons**.

_Example: Foodinho / Deliveroo case (Garante per la Privacy, June 2021)._ The authority examined **food delivery platforms** employing **gig workers** (riders delivering food orders by bike). The apps used for the riders were held **not legal** because they **failed to provide transparent information** about the **algorithms** used to manage work shifts, and did not explain how the **reputational rating system** embedded in the app worked. By applying a specific algorithm, the app created a form of **discrimination**, excluding riders from job opportunities. The companies were found to have violated the **principle of data minimization** (certain data was not necessary) and **protection by design and by default**, because the systems were built to **collect and store all data** relating to order management and to let authorized operators pass functions from one to another by **sharing this data**. Sharing data between **Foodinho** and **Deliveroo** (e.g. data about a specific rider's delays) is not correct: a system **designed for Foodinho** cannot simply be applied to Deliveroo, because the responsibility of one passes to the other and this produces **discrimination**. This therefore infringes privacy **by design** (the system was designed for one entity, not the other) and, as a consequence, privacy **by default**.
