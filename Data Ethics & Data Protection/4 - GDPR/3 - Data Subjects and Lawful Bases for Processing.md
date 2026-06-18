

## 1. Introduction and Administrative Remarks

**Core Concepts Review:** The foundational framework of the GDPR includes its implementation background, the differences between directives and regulations, the role of the Court of Justice of the European Union, the territorial scope (establishment vs. targeting criteria), and the classification of personal data (normal vs. sensitive/special categories).

## 2. Who is Protected by the GDPR?

The GDPR protects **natural persons** regarding the processing of personal data and rules relating to the free movement of such data.

* **Exclusion of Legal Entities:** The names of companies or enterprises are not protected by the GDPR. However, protection applies if those names are directly connected to specific natural persons representing the company.

_Example_: If an email is sent to a generic corporate address like `rettore@polito.it`, it does not pose a problem under the GDPR because it does not process data concerning a specific natural person. Conversely, sending an email to a specific address like `stefano.corgnati@polito.it` involves processing the family name of a natural person (the Director), meaning GDPR rules must be strictly respected.

### Definition of a Data Subject (Article 4.1)

According to **Article 4.1 of the GDPR**, personal data means any information relating to an identified or identifiable natural person. An identifiable natural person is one who can be identified, directly or indirectly, in particular by reference to an identifier such as:

* Name
* Identification number
* Location data
* Online identifiers

---

## 3. Lawful Bases for Processing Personal Data (Article 6)

In the business environment, companies execute various processing operations for different purposes. For every single processing activity, it is mandatory to identify a valid legal ground, known as the **lawful basis**, to justify the collection, use, or processing of personal data. Without one of these grounds, data processing is strictly prohibited.

**Crucial Exam Warning:** Do not confuse the **Principles of the GDPR** (e.g., privacy by design) or the **Rights of the Data Subject** with the **Justification Grounds (Lawful Bases)**. Confusing these concepts results in failing the exam.
 

**Article 6 of the GDPR** provides exactly **six lawful bases** for processing personal data:
1. Contract 
2. Legal obligation 
3. Legitimate interest 
4. Consent 
5. Vital interest 
6. Task carried out in the public interest / exercise of official authority 



---

## 4. Lawful Basis: Contractual Necessity

Processing is lawful when it is strictly necessary to comply with contractual obligations or to address pre-contractual requests from potential clients.

* **Strict Interpretation:** Contractual necessity must be interpreted strictly. It must be clearly demonstrated that the main object of the contract cannot be performed without processing that specific data. It cannot be used to collect excess data that is irrelevant to the core service.

_Example 1_: An e-commerce company selling products online must process the client's home address. Without collecting and processing this data, the company cannot physically deliver the goods or perform the contract.

_Example 2_: A potential client contacts a home repair company for a price quote to paint their house. Although an executed contract does not yet exist, the company can lawfully process the person's name and email under the pre-contractual ground to issue and send the estimate. However, the company cannot request unnecessary data like the client's date of birth, as it is completely irrelevant to sending a quote.

_Example 3_: In a pre-contractual stage, a bank opening an account or an insurance company drafting a policy can lawfully check the client's financial position or health status. The bank needs this information to verify the legality of activities and financial capability , while the health insurance company needs to evaluate previous diseases to calculate protection levels. This allows the processing of sensitive data under contractual necessity because the core object cannot be evaluated without it.

_Example 4_: A customer orders a laptop from an electronics store because it is out of stock. The customer provides a home delivery address. This address processing is entirely lawful under contractual necessity because it is required to fulfill the delivery. (Note: If the laptop had been available directly in the shop, requesting the address would be unnecessary and unlawful under this basis ). If the seller later uses that delivery address to send promotional materials, the processing becomes illegal under the contract basis, because the address was collected exclusively for product delivery.

---

## 5. Lawful Basis: Legal Obligation

Processing is lawful when it is required to comply with a legal mandate imposed by EU or national law. In these scenarios, processing is permitted even if the data subject does not provide consent or if a contract has not yet been executed.

Valid examples of legal obligations include:

* **Financial and Real Estate Compliance:** Financial institutions and real estate agencies conducting mandatory anti-money laundering (AML) verifications or consulting official lists of registered debtors.
* **Employment Reporting:** The absolute duty of employers to report employee salary data and social security numbers to tax and social security authorities.
* **Suspicious Transactions:** Financial organizations reporting suspicious transactions directly to competent regulatory authorities under established anti-money laundering frameworks.

---

## 6. Lawful Basis: Legitimate Interest

Data processing can be carried out to pursue the legitimate interests of a company, an enterprise, or a third party, provided that these interests do not override the fundamental rights, freedoms, and interests of the data subject.

* **Key Characteristics:** It must refer to a real, present interest expected in the near future. Speculative, overly general, or vague interests are strictly excluded.

* **Case-by-Case Assessment:** Companies cannot simply claim a legitimate interest; they must prove it by conducting a mandatory, documentable case-by-case evaluation.

### The Three-Step Legitimate Interest Assessment (LIA)

To validate this legal basis, companies utilize a standardized template split into three sequential tests:

1. **Purpose Test:** Assesses the real intent behind the processing. It answers questions regarding why the company wants to process the data, what benefits are expected, whether third parties or the wider public benefit, and compliance with industry guidelines or codes of practice.
2. **Necessity Test:** Evaluates whether the processing is strictly necessary and proportional to achieve the identified purpose. It checks if the same goal can be achieved via less intrusive means or by processing significantly less data.
3. **Balancing Test:** Considers the nature of the personal data (e.g., whether it involves special categories, criminal offenses, children, or vulnerable groups) and the reasonable expectations of the individual based on their relationship with the controller. It measures the severity, likelihood, and impact of potential data control loss, ensuring individual rights are not overridden.

### Recognized Examples of Legitimate Interest

While the GDPR does not provide an exhaustive list, common examples recognized by the regulation and the Article 29 Working Party include:

* Fraud prevention.
* Ensuring staff health and safety (e.g., monitoring employees via corporate badges for security and management purposes).
* IT, network, and physical security.
* Direct marketing and advertising, specifically when advertising represents the core commercial job of the company.
* Unsolicited non-commercial messages (e.g., political campaigns or fundraising).
* Exercise of freedom of expression or information (e.g., media, arts, and journalism using names and basic data without defamation).
* Scientific, historical, or statistical research.

_Example 1 (Video Surveillance)_: Banks, jewelry shops, or petrol stations located in areas prone to property crimes can lawfully utilize video surveillance systems without obtaining consent. The legitimate interest consists of preventing dangerous situations and offenses. However, a strict balance remains mandatory: if a camera records video of a customer's highly private life completely unrelated to safety, it infringes on privacy rights, and the footage must be destroyed immediately.

_Example 2 (Copyright Infringement & Balancing Failure)_: Phonogram producers in Italy and Spain requested telecom operators (such as TIM) to hand over the IP addresses of users who allegedly infringed copyrights on songs. While the producers had a clear legitimate interest in identifying copyright infringers, European courts ruled that in the mandatory balancing of rights, the users' fundamental right to private life and data privacy outweighed the protection of commercial exploitation rights. Consequently, telecom operators were justified in refusing to disclose the IP addresses.

---

## 7. Lawful Basis: Consent

Consent is highly visible, but there is a profound **common misconception** that it is the default or primary basis for all data processing.

### Consent and Privacy by Design

Overusing consent or asking for it when another lawful basis applies (such as contract or legitimate interest) is a **severe compliance mistake**. European Data Protection Authorities view this as a failure to comply with the principle of **Privacy by Design**. It indicates that the data controller does not understand the real legal grounds of their own processing operations.

Example: In Italy, when requesting a quote from utility suppliers (gas, electricity, or insurance), an email address is entirely sufficient to receive the estimate. A telephone number is not contractually necessary. If the company requests a phone number to make follow-up sales calls, they can only do so if the user explicitly ticks a separate box to give separate, valid consent for marketing calls.

### Requirements for Valid Consent (Article 4.11 & Article 7)

According to **Article 4.11**, consent must be a clear affirmative action indicating a data subject's specific wishes. To be legally valid, it must meet four criteria:

* **Freely Given:** The individual must have a genuine choice. It is invalid if there is an imbalance of power or dependency between the controller and the subject. It also forbids **bundling**: consent cannot be made a mandatory condition for executing a contract or receiving a service if the processing is not necessary for that service.
* **Specific:** Consent cannot be granted as a general, blanket authorization for any purpose. It requires strict **granularity**, meaning individuals must be free to choose separate check-boxes for different activities (e.g., separate consents for core service, marketing, transferring data to third parties, or behavioral profiling).
* **Informed:** The request must use clear, plain language adapted to the audience, avoiding lengthy or complex legal jargon. The information must be easily and continuously accessible via the company's privacy policy.
* **Unambiguous:** It requires an active statement or clear affirmative action, such as manually ticking an **unticked box**. **Pre-ticked boxes, silence, or total passivity/inactivity can never be interpreted as valid consent**.

### Mandatory Information for Informed Consent

To satisfy the GDPR **Principle of Transparency**, the controller must explicitly provide the following details before consent is given:
* Identity of the data controller.
* Specific purposes of the processing.
* Types of data collected and processed.
* The explicit existence of the right to withdraw consent at any time.
* Whether the data will be shared with, transferred to, or processed by designated third-party processors.
### No Detriment and the Cookie Wall / Paywall Controversy

For consent to be free, choosing not to consent must result in **no detriment** or damage to the user. A company cannot punish a user who refuses non-essential tracking by offering less favorable pricing or restricted experiences.

* **Cookie Wall:** A mechanism blocking website access unless users accept all cookies. The European Data Protection Board (EDPB) expressly prohibits this practice because it destroys free choice.
* **Paywall Alternative ("Consent or Pay"):** A mechanism forcing users to either accept profiling cookies or buy a paid subscription to view content.

Example (The Italian Real Estate & Renovation Case): A local real estate agency requests a client's phone number strictly to contact them if they are running late for a property viewing appointment. However, the agency intends to sell these phone numbers to home renovation companies for marketing. This practice is completely illegal unless the agency obtains a separate, specific, granular, and freely given consent to transfer data to third parties. Furthermore, if the agent writes this phone number on a loose piece of paper and leaves it exposed on a shared desk where ten other unauthorized employees operate, it constitutes a severe security failure and a processing detriment to the data subject.

Example (The Italian Newspaper Paywall Conflict): In October 2022, major Italian newspapers and magazines (including *La Repubblica* and *La Stampa*) implemented a "Consent or Pay" wall, forcing non-subscribers to either accept tracking/profiling cookies or purchase an annual subscription to read articles. The Italian Data Protection Authority (*Garante Privacy*) initiated proceedings, evaluating this setup as a structural violation where consent is not freely given. Conversely, data protection authorities in Austria and France have provisionally deemed cookie paywalls acceptable, arguing that newspapers incur high publishing costs and can require payment or data monetization, though users have launched counter-proceedings against these decisions.

### Withdrawal of Consent (Article 7.3)

Data subjects maintain an absolute right to withdraw their consent at any time.

* **Equivalence of Effort:** Withdrawing consent must be **as easy as it was to give it**. If consent was given with a single click on a website, the company cannot force the user to call a telephone call center or write formal letters to undo it; a corresponding one-click digital option must exist.
* **Temporal Effect:** Withdrawal does not affect the lawfulness of processing carried out before the withdrawal. Processing is lawful up to the moment of withdrawal; it becomes immediately unlawful if processing continues after that point.
* **Consequence:** Upon withdrawal, data must be deleted immediately unless another valid lawful basis (e.g., compliance with a legal obligation like anti-money laundering laws) overrides it.

### Corporate Accountability and Record-Keeping

Under GDPR compliance and accountability, companies must securely store and maintain comprehensive reports and evidence regarding consent logs. 
They must be able to legally prove:

* Which specific individual gave consent.
* Exactly when and how the consent was obtained.
* The precise scope of what the individual agreed to.
* Records of when a user executed a request for withdrawal or deletion.

---

## 1. Justification Grounds for Processing Personal and Sensitive Data

The General Data Protection Regulation (GDPR) establishes strict frameworks regarding when personal and special categories of personal data (sensitive data) can be processed.

### Processing Based on Vital and Public Interests
In addition to standard legal grounds such as contracts, legitimate interest, and consent, personal data processing is allowed under specific circumstances under Article 6(1) of the GDPR:
* **Vital Interests (Article 6(1)(d)):** Processing is lawful when it is necessary to protect the vital interests of the data subject or another natural person.
  _Example: An ambulance service needs to handle the health data of an individual suffering from an acute medical condition or disease. It is legally permitted to process all necessary data, including health data, to safeguard that person's vital interests._
* **Public Interest or Official Authority (Article 6(1)(e)):** Processing is lawful when it is necessary for the performance of a task carried out in the public interest or in the exercise of official authority vested in the controller, as provided by Article 6(1)(d)/(e) .
  _Example: This ground covers the essential public safety and investigative activities routinely carried out by the police force._

When processing is justified under these specific grounds of protecting vital interests or performing public duties, the standard restrictive provisions set out in Article 9 for sensitive data do not apply in the same restrictive manner. Public authorities or authorized entities can process the data without seeking the specific explicit permission usually mandatory for sensitive information.

### The General Prohibition of Sensitive Data (Article 9(1))
The foundational rule of the GDPR regarding special categories of personal data—commonly referred to as **sensitive data**—is that its processing is **always prohibited**. According to Article 9(1), this prohibition applies strictly to data revealing or concerning:
* Racial or ethnic origin 
* Political opinions 
* Religious or philosophical beliefs 
* Trade union membership 
* Genetic data and biometric data processed for the purpose of uniquely identifying a natural person 
* Data concerning health 
* A natural person's sex life or sexual orientation 

### Exceptions to the Prohibition (Article 9(2))
The general prohibition on processing sensitive data is only lifted if two cumulative conditions are met: the controller must have a standard lawful basis for processing personal data, **and** one of the specific exempting conditions listed under Article 9(2) must be fully satisfied. The primary justification grounds include:

* **Explicit Consent (Article 9(2)(a)):** The data subject must give explicit consent to the processing of those sensitive personal data for one or more specified purposes. This consent must strictly follow standard GDPR criteria: it must be freely given, specific, and unambiguous. 
  While regular personal data consent can sometimes be inferred from specific behavior, sensitive data requires **written evidence or a signed statement** (e.g., a physical piece of paper or a specific digital signature) in virtually all practical scenarios.
* **Employment, Social Security, and Social Protection Law (Article 9(2)(b)):** Processing is allowed if it is necessary to carry out obligations or exercise specific rights of the controller or the data subject in the field of employment, social security, and social protection law, insofar as it is authorized by national law or collective agreements providing safeguards. This is conceived entirely in the exclusive interest of the employees rather than the employer.
  _Example: An employer needs to prepare the monthly company payroll. To accomplish this, the employer is legally permitted to communicate relevant sensitive employee data to an external professional consultant appointed specifically to process the payroll._
* **Protection of Vital Interests with Incapacity (Article 9(2)(c)):** Sensitive data can be processed if it is necessary to protect vital interests or safety, and the data subject is **physically or legally incapable of giving consent**. This exception is strictly centered on protecting the data subject, never the controller.
* **Data Manifestly Made Public (Article 9(2)(e)):** Processing is permitted if it relates to personal data that has been clearly and intentionally made public by the data subject.
  _Example: An individual chooses to publish specific details regarding their health condition, a disease they are battling, or their sexual orientation directly onto their public personal social media profiles, such as Instagram or Facebook. Because the individual intentionally rendered this information public, third parties may process it without needing to request explicit consent._
* **Establishment, Exercise, or Defense of Legal Claims (Article 9(2)(f)):** Data can be processed when necessary for the establishment, exercise, or defense of legal claims or whenever courts are acting in their judicial capacity.
  _Example: A landlord initiates a legal action before a judge against a tenant who has failed to pay their apartment rental fees. The landlord is fully authorized to process and use the tenant’s relevant data for this action. The judge, in turn, is authorized to process that data to evaluate the dispute, as individuals must be allowed to defend their legal rights and interests._
* **Substantial Public Interest (Article 9(2)(g)):** Processing is allowed when necessary for reasons of substantial public interest laid down in law. This processing must be strictly proportionate, respect the essence of data protection rights, and maintain specific measures to safeguard the data subject's fundamental rights. It requires a careful balancing act between the collective public interest and individual privacy rights.
* **Preventive and Occupational Medicine (Article 9(2)(h)):** Processing is allowed for the purposes of preventive and occupational medicine, assessing an employee's working capacity, medical diagnosis, or managing healthcare systems. This addresses a broader community interest than standard employment law.
  _Example: Public health systems or corporate medical officers process health data to implement wide-scale preventive medicine programs. Even though the precise identities of all subjects may not be known initially, processing the subsequent individual medical data is permissible because it serves the collective safety and health interests of the entire workforce and community._

Before any sensitive data processing begins, the controller must determine, document, and prove exactly which justification ground applies. Failing to document this renders the processing completely unlawful.

### The Role of National Legislation
Article 9 concludes by stating that EU Member States may maintain or introduce further conditions, including specific limitations, regarding the processing of genetic data, biometric data, or data concerning health. 

Therefore, compliance requires looking beyond the general framework of the GDPR. A data controller must always double-check the specific **national law of the country where the processing occurs**, as local member state legislations frequently adopt much stricter requirements for these highly sensitive classes of information.

---

## 9. Subjects Involved in Data Processing

The GDPR identifies specific actors involved in any data processing operation. It is critical during legal evaluations not to confuse these operational roles with foundational GDPR principles or the rights of data subjects.

### The Core Definitions (Article 4)
* **Data Subject:** The natural living person whose personal data is being processed, and who is the primary subject of protection under the GDPR framework.
* **Data Controller (*Titolare del trattamento*):** The natural or legal person, public authority, agency, or other body which, alone or jointly with others, **determines the purposes and means** of the processing of personal data.
* **Data Processor (*Responsabile del trattamento*):** A natural or legal person, public authority, agency, or other body which **processes personal data on behalf of the controller**.
* **Recipient:** A natural or legal person, public authority, agency, or another body to which personal data is disclosed, regardless of whether they are a third party or not. It is distinct from the data subject, controller, or processor. 
* **Third Party:** Any natural or legal person, public authority, agency, or body *other* than the data subject, the controller, the processor, and the persons who are under the direct authority of the controller. They represent external entities who may only process data under very specific, legally defined exceptions.

### Determining Roles: Controller vs. Processor
Distinguishing between a controller and a processor can be complex in real-world business environments. To determine the true legal status of an entity, organizations must evaluate a set of targeted diagnostic criteria:

* **Initiative:** Did you take the initiative to start collecting or processing the data in any other way? (Yes $\rightarrow$ Controller)
* **Purpose:** Did you decide the underlying purpose of the processing? (Yes $\rightarrow$ Controller)
* **Data Types:** Did you decide which types of personal data are to be collected and from which types of individuals? (Yes $\rightarrow$ Controller) 
* **Instructions:** Are you giving instructions to another entity processing personal data rather than following instructions from someone else? (Yes $\rightarrow$ Controller)
* **Benefit:** Are you the party mainly benefiting from the results of the processing? (Yes $\rightarrow$ Controller)
* **Relationship:** Do you have a direct relationship with the data subject? (Yes $\rightarrow$ Controller)
* **Monitoring:** Do you monitor other entities' execution of the service? (Yes $\rightarrow$ Controller)

Answering "yes" to these parameters indicates that the entity functions as a controller, while "no" answers suggest a processor role.

_Example: A practicing lawyer processes personal and highly sensitive details belonging to both their own client and the opposing party during a lawsuit. While the lawyer acts to benefit the client and gathers information based on the client's original dispute, the lawyer decides autonomously how to structure the defense, which data points are legally required for court briefs, and does not follow operational data instructions from the client. By answering affirmatively to the initiatives of purpose, collection, and autonomous decision-making, the lawyer acts legally as an independent, **autonomous data controller**, not a processor hired by the client._

### Key European Court of Justice (ECJ) Case Law
The boundaries between roles have been continually clarified by the ECJ, utilizing guidelines such as **EDPB Guidelines 07/2020**:
* ***Wirtschaftsakademie* Case:** The ECJ ruled that the administrator of a fan page hosted on a social network qualifies as a data controller. Despite operating within a larger network platform, the administrator's decisions mean they shoulder full Article 24 controller responsibilities.
* ***Fashion ID* Case (2019):** The ECJ declared that a commercial website embedding a social plugin (specifically the Facebook "Like" button) acts as a controller alongside Facebook Ireland, due to making it possible to automatically transmit visitors' personal data to Facebook.
* ***Jehovah's Witnesses* Case:** The ECJ established that a religious community acts as a controller jointly with its individual members who take part in organized door-to-door preaching. Even if individual members argue they are mere processors, or even if the central community leadership never explicitly sees the specific personal data collected or provides written instructions, the community is a joint controller because it organizes, coordinates, and encourages the framework of the activity.

---

## 3. Joint Controllers (Article 26)

When two or more controllers **jointly determine the purposes and means of processing**, they are formally designated as **Joint Controllers**. 

### Identifying a Joint Relationship
Organizations operate under joint controllership if they can answer "Yes" to the following core operational parameters:
1. Do you have a common objective and purpose with other parties regarding the processing?
2. Are you using the exact same set of personal data for this processing as another controller?
3. Did you design the process with another controller?
4. Do you have common information management tools with another controller?

### The Joint Arrangement Requirements
Joint controllers must establish a clear, transparent internal legal arrangement to map out their respective compliance duties under the GDPR. This arrangement must explicitly detail who fulfills the obligation to provide mandatory privacy information notices to data subjects, and how data subject rights are managed. Within this contract, the joint controllers can designate a single contact point for data subjects.

---

## 4. Obligations and Liability Frameworks

### Obligations of the Data Controller
The data controller bears the largest burden of regulatory compliance and organizational liability under GDPR Article 24.
Their primary mandatory duties include:
* **Demonstrating Compliance:** Controllers must implement technical and organizational security measures to ensure and actively prove that all processing aligns with the GDPR.
* **Privacy by Design and Default:** They must ensure that data protection principles are structurally integrated into systems from the absolute start, a responsibility that remains with the controller even if external processors do the practical work.
* **Maintaining Records:** Controllers must maintain comprehensive records detailing exactly how, why, and on what legal justification grounds data is processed.
* **Data Breach Notification:** If a breach occurs, the controller must notify the relevant Data Protection Authority within **72 hours** of discovery, and inform individual subjects without undue delay.
* **Vetting Processors:** Controllers are legally required to employ only data processors that provide sufficient technical and organizational guarantees, structured via a binding **Data Processing Agreement (DPA)**.
* **Impact Assessments and DPOs:** When undertaking high-risk processing or handling massive volumes of data, controllers must perform a Data Protection Impact Assessment (DPIA) and officially appoint a Data Protection Officer (DPO).

### Obligations of the Data Processor
While processors act on strict instructions, they maintain independent obligations under the GDPR:
* **Independent Record Keeping:** Processors must keep detailed records of all processing activities carried out on behalf of a controller.
* **Security Implementation:** They must implement technical security protections matching the structural risks of their assigned tasks.
* **Immediate Breach Alerts:** Processors are legally bound to notify the data controller immediately upon discovering any data breach within their systems.
* **DPO and Vetting Support:** Processors must actively assist the controller in achieving full compliance and cooperate with the controller's DPO.

### The Chain of Civil Liability
The allocation of liability between controllers and processors forms a protective chain for the data subject:

* **The Front-Facing Rule:** Before the data subject and public supervisory authorities, the **Data Controller is always the primary liable party** . Because individuals hand over data to the controller, the controller answers directly for processing failures or processor errors.
* **The Re-qualification Rule:** If a processor violates or completely ignores the explicit instructions given by the controller, the processor is instantly **re-qualified as a Data Controller** under the law. They lose their processor protections and inherit full direct liability and administrative exposure before data subjects.
* **The Right of Recourse:** If a data breach occurs due to a processor's negligence, the controller may be forced to pay immediate damages to the injured individuals or authorities. However, under the DPA and mutual indemnity laws, the controller retains the legal right to pursue secondary claims against the processor to recover those financial losses. Ultimately, the economic risk of a processor-caused breach stops with the processor.
