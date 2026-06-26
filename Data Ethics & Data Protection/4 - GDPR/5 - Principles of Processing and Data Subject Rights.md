## 1. Recipient vs. Third Party

### Recipient

A **recipient** is any subject who receives personal data from another subject. The category is broad: the controller can be a recipient, the processor can be a recipient — essentially all subjects receiving data are considered recipients. The defining feature is that a recipient receives data even though it is neither a controller nor a processor.

A recipient can be a person internal to the controller. For instance, an employee of the controller who processes data on the controller's behalf is considered a recipient, because they are internal to the controller's organization and process the data for the controller.

### Third Party

A **third party** is any party different from the controller, processor, data subject, and recipient. It is an external subject that does **not** have any connection to the personal data and is **not** entitled to process it — but to whom data _could_ potentially be transferred.

The key characteristic is that the third party is an **abstract / undetermined** subject. In an information notice (e.g., under Article 13) one can write that "data may be transferred to third parties" under specific conditions, without identifying who those third parties are. The third party exists, in this abstract sense, only at the moment a data subject or a legitimate transfer makes it possible to transfer the data to it.

_Example: processing of employees' personal data._ 
The information notice may state that "personal data can be transferred to third parties," such as, for instance, an external company that handles payroll. The moment data is actually transferred to that company, the third party becomes a **processor**, and at that point it becomes necessary to execute a data processing agreement. So the "third party" is an abstract category that, once activated by a concrete transfer, turns into a defined role (typically a processor).

---

## 2. The Seven Principles of Processing (Article 5 GDPR)

Article 5 GDPR sets out the key principles underlying the processing of personal data. The professor stresses these are **principles**, distinct from **rights** (such as the right to give or withdraw consent). For the exam: be able to list the seven principles, all derived from Article 5.

The seven principles are:

1. **Lawfulness, fairness and transparency** — personal data shall be processed lawfully, fairly and in a transparent manner in relation to the data subject.
2. **Purpose limitation** — data shall be collected for specified, explicit and legitimate purposes and not further processed in a manner incompatible with those purposes. Further processing for archiving in the public interest, scientific or historical research, or statistical purposes is **not** considered incompatible with the initial purposes.
3. **Data minimization** — personal data shall be adequate, relevant and limited to what is necessary in relation to the purposes for which they are processed.
4. **Accuracy** — personal data shall be accurate and, where necessary, kept up to date. Every reasonable step must be taken to ensure that inaccurate data, having regard to the purposes, are erased or rectified without delay.
5. **Storage limitation** — personal data shall be kept in a form which permits identification of data subjects for no longer than is necessary for the purposes of processing. Data may be stored for longer periods insofar as they will be processed solely for archiving in the public interest, scientific or historical research, or statistical purposes.
6. **Integrity and confidentiality** — personal data shall be processed in a manner that ensures appropriate security, including protection against unauthorized or unlawful processing and against accidental loss, destruction or damage, using appropriate technical or organizational measures.
7. **Accountability** — set out in **Article 5(2)**: the controller is responsible for, and must be able to demonstrate compliance with, paragraph 1.

---

## 3. Lawfulness, Fairness and Transparency (Art. 5.1.a)

Personal data must be processed in a **lawful**, **fair** and **transparent** manner.

- **Lawfulness**: processing must rest on one of the legal bases foreseen by the GDPR; there must be a legitimate ground for processing.
- **Fairness**: data must be processed in a way that respects the data subject and does not mislead or harm them.
- **Transparency**: the purposes of data collection must be **known and made clear in advance** to the data subject. The data subject must be able to understand how and why their data is processed.

The core idea linking the three: the purpose of the collection must be declared up front and the processing carried out within the bounds of the law and of good faith.

---

## 4. Purpose Limitation

Data must be collected for **specified, explicit and legitimate purposes**, and cannot later be processed in ways incompatible with those original purposes. The purpose has to be fixed at the point of collection; you cannot collect data for one reason and then repurpose it for an unrelated one.

The GDPR carves out an exception: further processing for **archiving in the public interest, scientific or historical research, or statistical purposes** is not deemed incompatible with the initial purposes.

_Example: the list of students._ 
A professor who collects a list of students for a given course cannot keep reusing that list for unrelated aims. If the purpose was managing that course, the data is tied to that purpose and cannot be diverted to something else. (This same example returns under accountability, where the professor admits he may still be storing old student lists and would have to delete them to be compliant.)

---

## 5. Data Minimization

Personal data must be **adequate, relevant and limited to what is necessary** in relation to the purposes for which it is processed. Only the data strictly needed for the declared purpose should be collected — nothing more.

_Example: delivery address of clients._ 
A business should collect and store the home/delivery address of its clients **only** in cases where it actually has to deliver goods to their home. If there is no delivery, collecting the address is not justified. This same example reappears under accountability: to prove compliance with minimization, you must be able to show you collected addresses only where delivery was required.

---

## 6. Accuracy

Personal data must be **accurate** and, where necessary, **kept up to date**. Every reasonable step must be taken so that data which is inaccurate, with regard to the purposes for which it is processed, is **erased or rectified without delay**.

---

## 7. Storage Limitation

Personal data must be kept in a form that permits identification of data subjects for **no longer than is necessary** for the purposes of processing. Once the purpose is exhausted, the data should no longer be kept in identifiable form.

The exception mirrors purpose limitation: data **may** be stored for longer periods insofar as it will be processed **solely** for archiving in the public interest, scientific or historical research, or statistical purposes.

---

## 8. Integrity and Confidentiality

Personal data must be processed in a manner ensuring **appropriate security**, including protection against unauthorized or unlawful processing and against accidental loss, destruction or damage, by means of appropriate **technical or organizational measures**.

A key point: even **destruction or deletion** of data can itself be an unlawful act when it occurs during a processing for which the data is needed.

_Example: the lawyer destroying a client's data during a lawsuit._ 
If a lawyer must process a client's data to prepare the defense in an ongoing lawsuit and erases or deletes that data during this period, the lawyer is committing an unlawful activity. Data cannot be erased while it is needed for processing, because unauthorized destruction or damage is itself a security/integrity violation. This cannot be done without the data subject's permission.

### Need-to-know access within an organization

Integrity and confidentiality require that personal data is **not available to everyone** within an organization, but only to those who need to work with it. Access must be tied to the **role and purpose** of the person, and the intensity of security measures should be proportionate to the **potential risk** (a risk-based approach).

_Example: doctor's secretary vs. doctor's trainee._ 
The professor works through who, in a professional office, may access client data:

|Subject|Access to client data?|Why|
|---|---|---|
|Doctor's **secretary**|**No**|The secretary's purpose (e.g., handling contacts/appointments) does not require access to clients' health data; that data should not be stored where the secretary can reach it (unless the secretary were also a doctor cooperating in visits).|
|Lawyer's **trainee**|**Yes**|The trainee cooperates in preparing the defense, so access is justified. The trainee is a **recipient**, appointed in **writing** (a written appointment similar to a data processing agreement, not a full contract), entrusted to access the client's data.|
|Lawyer's **secretary**|**Yes**|In a law firm the secretary files briefs and documents of the lawsuit (in Italy, briefs are filed online/digitally), so the secretary necessarily accesses client data. This too requires a specific written appointment authorizing access to the filing system.|

The takeaway is a **risk-based, case-by-case evaluation**: for a doctor the principle of integrity and confidentiality is extremely important; for a lawyer it is important; for a shop selling shoes the risk is far lower, so the required measures differ accordingly.

---

## 9. Accountability (Art. 5.2)

Article 5(2): the controller **shall be responsible for, and be able to demonstrate compliance with, paragraph 1**.

Accountability establishes **two distinct responsibilities** for the controller:

1. The obligation to **be compliant** with all the principles already examined (and with the GDPR generally).
2. The obligation to **be able to prove** that compliance.

It is not enough to claim "I respect all six principles." The controller must produce **evidence** of compliance.

_Example: the stored student lists (revisited)._ The professor notes that, talking in class, he realizes he may still be storing old lists of past students. To be accountable he must both delete them (be compliant) **and** be able to prove he did not improperly retain them.

_Example: minimization proof._ To prove compliance with minimization, the controller must show, for instance, that delivery addresses were collected only where home delivery was actually required — not as a default.

The GDPR contains **no fixed list** of the actions a controller must take to demonstrate compliance. Proof is shown by demonstrating that all appropriate **technical and organizational measures** were adopted (e.g., collecting data per the minimization principle).

### Why the GDPR has no annex listing the measures

Under the old **Directive 95/46/EC**, there was an **attachment/annex** spelling out the technical and organizational measures to adopt (e.g., for paper data: lock the cabinet storing the list, train employees, etc.). The GDPR **deliberately omits** such an annex, requiring only that the controller adopt all necessary technical and organizational measures **without prescribing which ones**.

The reason is the principle of **privacy by design** (and privacy by default). If the legislator prescribed a fixed list of measures, it would contradict privacy by design, which requires building a data-protection system **tailored to the specific organization** and its actual processing.

_Example: doctor vs. shoe store vs. lawyer._ A doctor cannot adopt the same system as a shop selling shoes, because a doctor processes sensitive (health) data and needs different measures. A lawyer adopts yet different measures from a politician/other controller, and so on. If every controller applied an identical, predefined set of measures, privacy by design would not be implemented at all — the system would be abstract rather than concrete and matched to the real processing. (Concrete tailoring also matters because there is a real difference between, e.g., ordinary personal data and sensitive data, which must be taken into account.)

---

## 10. How a Doctor Implements Technical Measures (Controller–Processor in Practice)

A recurring student question: a doctor has no technical competency, so how can the doctor implement the technical measures required to protect sensitive client data?

The answer: the doctor does **not** build the system personally. The doctor must **appoint a technician** (an IT professional/company) to create the system. This is exactly why the students take this course — to understand their role when building systems that may contain personal and sensitive data.

### Role of the technician

In this scenario the **doctor is the controller**; the **technician is the processor** (the technician is not the controller, since that is the doctor). To appoint the technician as a processor, it is **not enough** to say a few words of appointment — the doctor must sign/execute a **data processing agreement**.

### The data processing agreement (DPA)

The DPA is the **contract** by which the controller appoints the processor, authorizing the processor to process the (sensitive) data. It must be signed/executed between doctor (controller) and technician (processor).

What matters for the doctor's accountability is not only signing the contract, but **verifying the processor's guarantees**:

- The controller must be assured that the processor has the **capabilities and knowledge** to carry out the activity and to build a system offering **sufficient guarantees** for the protection of the data.
- If the controller appoints a company lacking those capabilities and problems arise, the **controller is responsible** for having chosen a subject that did not provide sufficient guarantees.

_Example: the professor's own law firm._ 
The professor appoints an IT company to manage operational/marketing assistance, but only after being satisfied that the company is genuinely able to provide the service and protect the data — otherwise he, as controller, would bear responsibility toward his clients.

### Why the processor also benefits from a written DPA

The relationship between controller and processor is a **legal relationship grounded in the data processing agreement**. A written DPA matters for the **processor** too:

_Example: verbal vs. written appointment._ 
If the controller trusts the processor only verbally, the processor cannot later prove **why** it was processing data received from a specific subject. With a signed DPA, the processor can **demonstrate the authorization** and the reason for processing. So the written agreement protects the processor as well as the controller.

---

## 11. Data Subject Rights — Overview

The GDPR grants several rights to data subjects so they can **stay in control of their own personal data** and of how, for how long, and in what way it is processed. These rights are all connected to the manner, time, and duration of processing.

To let data subjects exercise control, the controller must first **inform** them about the processing: what data is processed, the **legal basis**, the **duration**, the purposes, and so on. Then, upon the data subject's request, the controller must:

- **correct** data that is inaccurate or unlawfully obtained;
- entirely or partly **stop using** the data;
- **delete** data (right to be forgotten);
- provide the data in a **machine-readable copy** (portability).

Requests can be submitted **orally or in writing** by the data subject.

### Article 12 GDPR — transparency and procedure

Article 12 requires the controller to provide information relating to the processing in a **concise, transparent, intelligible and easily accessible form, using clear and plain language**, in particular for information addressed specifically to a **child**.

- Information should be provided in writing, or by other means including, where appropriate, electronic means.
- When requested by the data subject, information **may be provided orally**, provided the data subject's identity is proven by other means.
- The controller must facilitate the exercise of data subject rights and must provide information on action taken **without undue delay and within one month** of receipt of the request.
- That period may be **extended by two further months** where necessary.
- If the controller does **not** take action on the request, it must inform the data subject **without delay and at the latest within one month** of receipt.

---

## 12. The Right of Access

The right of access lets the data subject obtain confirmation of whether their data is processed and obtain **copies** of that data, including related information.

_Example: the fired employee requesting their file._ 
The classic access case is the **employee's file**. A former employee, often in order to "disturb" the former employer, exercises the right of access and asks for **copies of all documents** containing their personal data. For an employee who worked 20 years, collecting every document containing personal data is **very burdensome** for the employer — yet it remains a valid request that must be honored.

### Form of the copy

If the access request is made by **electronic means**, the data must be provided in a **commonly used electronic form**, unless the data subject requests otherwise. The request must also be **reasonable**:

_Example: paper vs. electronic._ 
If a former employee demands **paper copies** of all documents, but the employer can collect everything electronically, the demand for paper is **not reasonable** — the employee can be given the information in electronic form.

### Protecting the rights of others when providing a copy

When providing a copy, the rights of **other subjects** must be protected.

_Example: an email containing a colleague's name._ 
A former employee requests access and is entitled to receive an email concerning them. But that email also contains the **names of other colleagues** who requested nothing. The controller must protect the other employees' data while still delivering the email to the requester. The solution is **not** to refuse the email, but to **redact / blank out** the names of the other subjects so their rights are protected while the requester still receives the document concerning them.

### Additional information accompanying the copy (Art. 15.1)

When a copy of the personal data is provided, the data subject must also receive additional information — the same kind already examined for information notices — including the **purposes of processing** (e.g., whether data was collected for marketing/advertising or only to fulfill the contract), and notably the **identity of the recipients** of the data.

_Example (Court of Justice case — postal company)._ 
A data subject asked a postal/post-office service company for access to the personal data concerning him, and also wanted to know whether his data had been **disclosed**, and to whom. The company gave only a **general response** and did **not** disclose the specific identity of the recipients.

The **Court of Justice** held that the right of access includes an **obligation on the controller to provide the actual identity of the recipients**, **unless**:


- it is **impossible** to identify the recipients, or
- the controller demonstrates that the request is **manifestly unfounded or excessive**,

in which cases the controller may instead indicate only the **categories** of recipients.

In this case it may have been impossible for the postal company to list every single recipient (the person may have sent a very large volume of mail), so a general response was considered sufficient and the company was not held responsible.

### Further information the data subject must be told about

The data subject must also be informed of:

- the right to request **rectification or erasure**;
- the right to **lodge a complaint** before the supervisory authority;
- the **source** of the data;
- whether data is processed through an **automated decision-making system**.

_Why automated decision-making matters._ 
Processing through an automated system is **riskier** than processing on paper. With paper, dissemination is limited — someone has to physically take and read the sheet. With an automated system, **dissemination, reproduction, and copying are far easier**, so the data subject has the right to know whether their data is processed by automated means.

- **Safeguards for transfers to third parties / outside the EU.** The data subject must be told about safeguards regarding transfers of personal data, especially **outside the European Union**. _Why it matters:_ the legislation of other countries may be weaker — e.g., **US legislation is not as strict** as EU law and does not provide the same safeguards — so the data subject must be informed when their data is transferred, for instance, to the United States.
- **Regulated professions (e.g., medical).** Where the controller communicates data to subjects bound by professional secrecy, such as doctors, the data subject must be informed, because this entails an **additional layer of protection**.