## 1. Data Subjects' Rights (continued)

The earlier part of the course covered several rights belonging to the data subject: the right to be informed, the right of access, the right to quality of the data stored by the controller or processor, and the right to rectification. This lecture continues with the remaining rights.

### Right to Restriction of Processing (Art. 18 GDPR)

Under the right to restriction, the data subject does not ask for data to be **erased**, but only for processing to be **suspended** while the data is kept. In the restricted state, the controller may only **store** the data; it may not actively process it unless one of the exceptions applies (data subject's consent, establishment/exercise/defence of legal claims, protection of the rights of another person, or important public interest).

Restriction typically applies in three situations: when the **accuracy** of the data is contested (pending verification), when processing is **unlawful** but the subject prefers restriction to erasure, and when the controller no longer needs the data but the subject needs it for **legal claims**. A further case is when a verification is pending regarding the **legal grounds** of the processing.

_Example: the former employee and the pending dispute._ Normally, once the employment relationship ends, the former employer must erase the employee's data and the new employer takes over. But the employee may ask the **former** employer **not to erase** the data, only to **stop using** it. The reason: there is an ongoing **dispute/controversy** between employer and employee, and the relevant information — needed by the employee, e.g. for legal proceedings — is stored only at the employer's premises. So the employee invokes the right to restriction to preserve that evidence.

_Example: how restriction looks in practice._ If you (the controller/employer) keep my data on a **server accessible to all other employees**, you are **not** respecting my right to restriction. Concretely, the controller must **separate** the data — store it in a distinct physical or online location — and **remove** it from any place where other staff or assessors could simply look at it (including anything published online). This is harder to implement in practice than it sounds, because it requires real isolation of the restricted data, not just a statement.

### Right to Data Portability (Art. 20 GDPR)

Data portability lets the data subject move their data from one provider to another. The classic case is **switching phone operators**: you move the data tied to your account from one operator to another.

The point requiring protection is the **link between your phone number/account and your personal data**. Your phone number is personal data because it identifies you, and all the information attached to the account (your contacts, your activity) is personal data too. The previous operator acts as a **processor** that was entitled to hold this data only **for a specific period**; once you switch, it is **not** entitled to keep your contacts or account data.

_Example: TIM → another operator._ If I move from "TIM" to another provider, TIM cannot keep my contacts. If I gave my number to someone, that data was entrusted to the operator only for the duration of the relationship — so the purpose of portability is precisely to **allow the switch** from one operator to another, taking the data with you.

_Example: when the old provider may keep some data._ The provider may retain **specific** data in narrow cases — e.g. **marketing purposes with consent**, or, more importantly, where retention is needed for **legal/litigation reasons**. For instance, if data was used to commit an unlawful act (e.g. illegal downloading of music/software/movies through your account, dissemination of unlawful content), the operator may keep and process the data in order to **demonstrate it bears no responsibility** if legal proceedings begin.

### Right to Object (Art. 21 GDPR)

The right to object lets the data subject object to processing based on **legitimate interest** or **public interest**, and — very importantly — to processing for **direct marketing**. Where the objection concerns direct marketing, the controller must **stop**; there is no balancing test. For other grounds, the controller may continue **only** if it demonstrates **compelling legitimate grounds** that override the subject's interests.

_Example: the daily marketing phone calls._ Every day I get two or three sales calls. Each time I **exercise my right to object**: I tell them I object and will report them to the privacy authority, and they hang up immediately. Note the distinction from the right to be forgotten / withdrawal of consent: here I never **gave** consent in the first place, so they should not be calling at all (and it's unclear how they obtained my number). I am registered in the **opt-out registry** (the "do not call" registry recording that I don't want such calls), yet I still receive calls — so I object.

### Right Not to Be Subject to Automated Decision-Making (Art. 22 GDPR)

The data subject has the right not to be subject to a decision **based solely on automated processing, including profiling**, that produces **legal effects** concerning them or **similarly significantly affects** them. In other words, the subject is entitled to demand that decisions affecting them be made — at least reviewable — by **humans**, and to know whether their data is being processed automatically.

_Why the right exists — the risk of leakage._ Data processed by automatic means carries a higher risk of **data leakage / spreading**, possibly involving **sensitive data**. (Contrast: if a person at a small shop happened to mention some of my data to a friend, that is not a comparable systemic risk.) Automated processing at scale can produce a **leak or spread** of data, so the subject must be able to know whether decisions are automated and to refuse purely automated decisions. Profiling, in particular, is not possible without **consent**.

The right does **not** apply when the automated decision is:

- **necessary for entering into or performing a contract** (e.g. taking out an insurance policy entirely online, where the decision is made automatically to execute the contract);
- **authorised by applicable law**; or
- **based on the data subject's consent**.

Even where an exception applies and the controller is allowed to make an automated decision, the controller must still **implement safeguards** to protect the subject's rights — at minimum the possibility of **human intervention**, so that the entire system is not left purely automated. If the safeguards are not adopted, or consent is absent, or there is a conflict of interest, the processing is not legitimate.

_Example: consent limited to a specific purpose._ If I gave consent for a specific purpose (e.g. receiving emails about **food**), the company may send marketing only within that scope; it cannot use the same consent for unrelated purposes.

This concludes the part on data subjects' rights: failing to respect any of them is a **breach of the GDPR**.

---

## 2. Controller Compliance: The 10 Steps

The controller must act in specific ways to be GDPR-compliant. The professor lists **10 steps** on the slide (students should read them in full); the lecture highlights the most important ones.

### Privacy Policy and Cookie Policy

A website must always have both a **privacy policy** and a **cookie policy**, drafted in compliance with the GDPR. Cookies are a sensitive issue because they can be used to **track users** and store data about their behaviour.

_Example: the three cookie options._ When you visit a website you typically get three choices: **accept all**, **accept only essential/necessary**, or **reject/customise**.

- **Accept all** → you communicate to the page owner a large amount of data: your identity, your IP address, how many pages you visit, which pages, which product prices you looked at, how many times, etc.
- **Reject everything** → navigation may become inconvenient or even impossible (you can't move smoothly from page to page; you may have to keep logging in).
- **Essential cookies only** → these are the cookies strictly needed to use the website correctly; they do **not** require personal data such as your behaviour.

From a data-protection standpoint the recommendation is to **accept only essential cookies**. Note also that if you click "save choices", usually only the essential box stays ticked, so saving means consenting only to essential cookies (don't assume the other boxes are pre-checked in a way that binds you).

### Other steps highlighted

The remaining steps include setting up a **breach notification system**, analysing and putting in place a **company security policy**, ensuring releases/contracts with ICT and other parties protect data, appointing a **Data Protection Officer (DPO)** where required, and **establishing records of processing activities** (see Section 4). Two points are stressed below.

### Ensuring third parties (suppliers, trainees) respect data protection

The controller must ensure that the people and partners it works with also respect data protection.

_Example: the trainee in the law firm._ The professor's office has a trainee. To make the firm act in compliance, the professor **drafts policies**, explains to the trainee what data protection is, and explains key concepts. Every company in the controller role should issue internal policies governing the processing of personal data.

### Informing candidates and deleting their data

Candidates in a recruitment process must be informed that their data is being processed. When you receive a CV and evaluate it against an open position, you are processing the candidate's personal data — which is permitted for the purpose of the selection.

_Example: the rejected candidate's CV._ If you decide **not** to hire the candidate, in principle you must **delete all their data** once the evaluation is finished. A company might want to keep the CV in a database for future openings — but it cannot keep it indefinitely (e.g. retrieving a CV 3 years later for a new role is not allowed by default). This is a practical clash between business needs and the GDPR. The workaround: state in the **privacy policy** that CVs may be retained for **future collaboration**, but only for a **limited, defined period** (the professor uses up to about **5 years** for clients). Without such a clause, the data must be deleted after the selection ends.

### Data-protection clauses in supplier/contractor agreements

Contracts with suppliers must contain clauses on the processing of personal data, because performing the contract often involves exchanging personal data between client and supplier.

_Example: the supplier delivery and "Mr. Myrosi"._ A supplier must deliver parts to me tomorrow, but I won't be in the office. I email the supplier: "you'll find my collaborator, **Mr. Myrosi**." By naming my collaborator I am **communicating personal data** to the supplier, who will then process it (to make the delivery). Because such exchanges occur routinely when performing the contract, the agreement must include a clause covering the processing of this personal data. (Best practice: minimise the personal data sent.)

---

## 3. Data Breach: Obligations

In case of a **data breach**, the controller must be prepared to react. The two core obligations are to **notify the supervisory (data protection) authority** and, where required, to **inform the affected data subjects** (and account for all entities processing the data). The controller must keep a list of the relevant parties so it can promptly inform the supervisory authority, the data subjects, and any other processors involved.

The controller must also **be prepared to handle access requests** (see below), since after a breach subjects will want to know what happened to their data.

---

## 4. Records of Processing Activities (Art. 30 GDPR)

Each controller must maintain a **record of the processing activities** containing all the information listed in **Art. 30(1)** (categories of data, purposes, recipients, retention, etc.). Where the controller uses a **processor**, the processor must **also** keep its own record of processing activities. Records must be kept **in writing**.

### Why records matter — the burden of proof

_Example: defending yourself in a dispute._ If a data subject files a complaint before the authority (e.g. the Garante / "Antitrust"-type authority) claiming you processed their data unlawfully, **you** must be able to demonstrate that the processing was lawful — for instance that it was done to **perform a contract**. It is not enough to simply assert "I processed it lawfully, Mr. so-and-so"; you must **prove it in writing**, via a register or a contract. Without written records it would be practically **impossible** to demonstrate lawful processing — hence the need for centralised, written records.

### When records are mandatory

Drafting records of processing activities is **mandatory** under **Art. 30** where an enterprise or organisation processes data of more than a certain scale / regularly (the lecture references the threshold tied to the size and nature of processing). The professor uses his small office as a discussion case: even an office of **seven people** keeps a record so as not to have to assess case-by-case.

_Example: records differ by sector._ A generic small-company record sheet lists processing activities such as **customers' names, addresses, dates**, etc. Records for a **healthcare provider** or a **financial services company** look **different** from those of, say, a **Polytechnic university** or a **hospital**, because the processing — and the categories of data involved — differ across sectors. The record must reflect the specific data each organisation actually processes.

---

## 5. Non-Compliance, Enforcement, and Sanctions

If the controller does **not** comply with the GDPR (or, if it does not process personal data at all but should comply, etc.), the processing is considered **unlawful**. Unlawful processing typically surfaces through a **complaint** by a data subject — though the controller can defend itself by explaining why, in its view, the processing was lawful.

### How proceedings start

Two routes:

- **By complaint of the data subject** — the subject alleges to the supervisory authority that processing was unlawful and that the controller/processor failed to respect its obligations.
- **By the authority itself (ex officio)** — the data protection authority can start proceedings **directly**, without any complaint, when it independently learns of a possibly unlawful situation.

_Example: the call centre._ In Italy, a **call centre** very often receives attention from the data protection authority on the authority's own initiative, because the authority is unsure about the lawfulness of the call centre's processing, and so it opens investigations without waiting for a complaint.

### Consequences of unlawful processing

Unlawful processing may lead to: **administrative audits**, **administrative fines**, **criminal charges** in some cases, exclusion from **public tenders** or from working with certain clients, and **negative publicity / reputational damage**.

The fines: non-compliance can reach up to **€20 million**, or up to a percentage of turnover (**4%**), whichever applies — and for breaches the figure can be even higher when calculated on turnover.

_Example: the Intesa Sanpaolo case (the bank employee in Bisceglie)._ A few years ago, an employee at a **Intesa Sanpaolo** branch (in the Bisceglie/Bari area) accessed the bank accounts of famous Italian people — politicians, influencers, doctors, singers — and knew everything about their finances. **Last week** the **Garante** (data protection authority) issued a sanction against Intesa Sanpaolo for **failing to build a system capable of preventing** such unauthorised access to all the bank accounts. The fine was **€32 million** — the €20 million cap was insufficient, so the authority applied the **4% of turnover** rule. The professor notes this is the **first time** he has seen a fine this high (previous large fines, e.g. against telephone providers or call centres, were lower), and that beyond the money, the **reputational damage** to the bank is severe.

### Criminal and other consequences

Beyond fines, **criminal charges** may follow under applicable national law, and there can be knock-on effects such as **inability to participate in public tenders** or to work with clients (who would no longer trust the company's compliance), plus the costs and damage of the **investigation** itself.

---

_Note: the lecture ended by referring students to the slides for further detail on the post-breach review, and noted that the topic of data protection had already been partly covered with Prof. Mancaleoni._