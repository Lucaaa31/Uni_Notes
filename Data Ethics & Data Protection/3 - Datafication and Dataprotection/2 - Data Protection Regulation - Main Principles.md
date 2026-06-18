## 1. Historical Context and the Origin of Data Protection Law

Data protection emerged in the late 1950s and 60s in response to the "Mainframe Era" and the digitalization of government administration. During this period, governments centralized vast amounts of citizen data to support taxation and the administration of the rising Welfare State. However, during the Cold War, this mass centralization raised profound fears among citizens regarding social monitoring and potential discrimination based on political orientation. Rather than halting data collection entirely—which would have dismantled welfare services—a social trade-off was reached, balancing the government's need for efficiency with the citizens' demand for safeguards against misuse.

This trade-off makes data protection highly unique among personality rights. While rights like the right to one's image are regulated minimally and rely on general tort law to seek compensation after a violation occurs, data protection was "co-created" by lawyers and computer scientists. This collaboration introduced a process-based, procedural logic to the law. Instead of a simple blanket protection, the law regulates every stage of the data lifecycle: collection, processing, transfer, and destruction; to mitigate risks before any damage actually happens.

## 2. Core Roles and Actors

To operationalize this procedural approach, the GDPR legislation identifies actors based strictly on their relationship with the data and the power they hold over it.

- **Data Subject** is the natural person to whom the data refers and whose personality is reflected in it.

- **Data Controller** is the entity that determines both the purposes ("why") and the means ("how") of the processing, bearing the primary legal liability.

- **Data Processor** is a third party that processes data strictly on behalf of, and in the interest of, the controller (e.g. a marketing agency hired by a bank).

- **Joint Controllers** exist when two or more entities share an interest in the data, such as a hotel and [Booking.com](http://booking.com/) both utilizing customer information for their own distinct business interests.

## 3. The Shift to the Private Sector and the Evolution of Consent

In the 1980s and 90s, the distribution of computing power via Personal Computers shifted the focus from the public sector to the private sector. Companies began harnessing consumer data for direct marketing, customizing advertising to individual patterns. A famous illustration of this predictive power is the Target case, where a supermarket identified a young girl’s pregnancy before her family knew based entirely on shifts in her purchasing patterns.

This commercialization created a new social trade-off: users now exchange their data for "a slice of the cake," meaning economic benefits like discounts or free services like email and social media.

Because private companies do not have a public legal mandate to collect data like the government does, consent became the primary legal basis and negotiation tool for data processing in the private sector. Other legal bases include processing necessary for a Contract (e.g., providing an address to Amazon to receive a package) and Legitimate Interest, where a company's internal needs (like sending a car maintenance warning) can prevail if they do not significantly impact the user's privacy.

## 4. International Data Flows and Geopolitics

To ensure that these domestic protections are not circumvented by simply moving servers abroad, data moving outside the EU remains under the protection of EU law due to the "territorial principle".

The EU utilizes **Adequacy Decisions** to recognize countries with safe data standards (like the UK or Japan). For countries without adequacy status, entities must rely on Standard Contractual Clauses (SCCs), which act as "copy-paste" contractual safeguards.

Geopolitically, while the EU restricts data flows to protect individual privacy, regimes like China or Russia restrict trans-border data flows to maintain social control over their citizens, illustrating how similar regulations can serve entirely opposite goals.

## 5. Fundamental Rights and the Balancing Test

In the 1980s, the conceptual framework deepened, and data protection was legally recognized as a fundamental right.

This classification is crucial because it means data protection generally prevails over pure economic interests.

This was solidified in the 2014 Google Spain case, which established the "Right to be Forgotten" ruling that an individual's fundamental rights require a search engine to remove outdated, irrelevant professional links, despite the search engine's economic interests.

However, this right is not absolute and is subject to a balancing test against other competing interests. 
For instance, in the Manni Case, a manager of a bankrupted company requested his history be forgotten from a public register, but the court ruled that market transparency takes precedence over individual privacy in that context. Similarly, in Mosley vs UK, the court found that public interest regarding participation in Nazi-themed events superseded the participant's claim to privacy, demonstrating that public interest can override data protection.

## 6. Defining Personal Data, Privacy, and Non-Personal Data

The legal scope of **Personal Data** is extraordinarily broad, _covering any information relating to an identified or identifiable natural person_.

It is vital to separate Data Protection from Privacy.

**Privacy** concerns intimate secrets or personal life (e.g., "what I did last night"), whereas **Personal Data** encompasses even publicly available information like professional profiles or names on websites.

Just because data is public does not mean it is free to use; this distinction is currently creating massive legal challenges for AI companies scraping the internet to train Large Language Models (LLMs) without a valid legal basis.

Modern technology constantly expands what constitutes personal data. Mobility data can be cross-referenced to infer religious habits or health treatments, and Wi-Fi distortion can now identify the unique shape of a human body without using cameras. Conversely, Europe also regulates non-personal data, such as telemetry from industrial machines in a factory, though the regulatory goals there focus on industrial optimization rather than individual human rights.

## 7. The Fragility of Anonymous Data vs. Pseudonymous Data

While true **anonymous data** (such as aggregate door-counter statistics) falls completely outside the GDPR, the professor warned against the _technical fragility of anonymized data_.

Because anonymization processes can often be reverse-engineered, legal status depends on a test of time, effort, and cost. If data can be re-identified with relatively low cost and effort —such as when researchers successfully re-identified supposedly anonymous health records released by the US Department of Health — it remains legally classified as personal data.

**Pseudonymous data**, where identifying details are replaced by a code, is explicitly classified as personal data because the key to bridge the data back to the person still exists somewhere. However, a recent CJEU ruling created an important nuance for sectors like medical research: pseudonymous data can be treated as anonymous by the receiver if they have an airtight legal and technical barrier preventing them from ever accessing the decryption key held by the sender.

## 8. Sensitive Data and the Broad Scope of Processing

Certain types of data are designated as **Sensitive** **Data** (Special Category) and demand a much higher level of legal protection. This category includes information regarding race, political opinions, religious beliefs, and trade union memberships because these metrics have historically been weaponized to discriminate against populations. It also encompasses genetic and biometric data (like facial recognition).

These are deemed highly critical because they are lifelong, unchangeable identifiers that pose severe risks for total social control, particularly in non-democratic regimes.

Furthermore, the legal definition of "data processing" is interpreted quite expansively by the courts. 
_Example_: Jehovah’s Witnesses Case, the court ruled that manual, paper notes taken about people's reactions during door-to-door preaching constituted "data processing". Consequently, the religious community itself was designated as the "data controller" because the processing served its general interest, even if the central community never directly accessed the physical notes.

## 9. The Risk-Based Approach and Evolution of Management

Ultimately, the central philosophy underpinning modern digital regulation—including the GDPR and the recent AI Act—is the R**isk-Based Approach**. The core objective is to prevent damages before they happen through meticulous risk assessment, rather than relying on reactive compensation after the fact.

Historically, this risk management has evolved alongside technology. In the early Mainframe Era, the primary tool was **Licensing**, which strictly required prior authorization to create a database. As computing power scaled exponentially, prior authorization became unfeasible, shifting the regulatory framework toward **Notification** models and internal structural compliance. Today, risk is mitigated largely through **Purpose Limitation**—ensuring organizations only collect what is strictly necessary for a declared goal—and by guaranteeing subject **Transparency**. By granting data subjects **Access Rights**, they are empowered to monitor, rectify, and delete their own data, forming a decentralized layer of risk management in the modern digital society.