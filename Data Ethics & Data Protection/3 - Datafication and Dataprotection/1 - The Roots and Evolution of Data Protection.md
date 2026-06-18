## 1. **From Ethics to Legal Compliance**

**Your system will be judged not only on ethics, but on legal compliance**. There is a strict line between ethics and law. Failing **ethically** risks your company's reputation and finances. Failing **legally** means authorities will pull the plug and halt your system entirely. Legal compliance is the ultimate hard boundary. For this reason, the minimum standard **for any computer scientist working with data is**:

- **Verify legal compliance:** If a solution does not comply with the law, it has no future, regardless of how technically brilliant it may be.
- **Avoid copy and paste:** Never import a solution from another country without mapping the local regulatory ecosystem first. 
	- _Example_: In Politecnico a student proposed replicating a bio-genetic experimentation model observed in China, without realizing that China operates under a significantly more relaxed regulatory regime on that topic compared to Europe. In Europe, human subject protection standards make that model legally impossible. The idea was not inherently bad, it was simply **non-transferable** to a different legal context. **Always be aware of the context in which you work.**

## 2. **The European Framework**

### **The GDPR Is the Most Advanced Instrument, Not the Only One**

A widespread misconception is that the European framework _is_ the GDPR. In reality, the **GDPR is the most advanced iteration** of a longer tradition. 
Many countries worldwide have adopted data protection legislation, but most are based on **earlier 1980s–90s versions** of the European framework — not the GDPR itself, which requires a level of legal and technical compliance that many countries are not yet positioned to meet.

### **Convention 108 (1981) — The Historical Cornerstone**

The foundational instrument is **Convention 108**, adopted by the **Council of Europe in 1981**. Although it was designed for European member states, it attracted ratification from countries in **Latin America** and **Africa** as well, because those countries recognized in it a workable model suited to their specific context. This makes Convention 108 the first genuinely **international data protection instrument**.

### **The EU Model as the Global Reference**

In different forms — whether the 1990s version or the GDPR — the **EU approach is the reference framework for the large majority of countries** globally. Understanding the GDPR means you will recognise analogous structures in Singapore, Nigeria, Morocco, Peru, and many others. In the **United States**, there is no single federal data protection law — only sector-specific and public-sector regulations — but individual states with major digital economies, most importantly **California**, have adopted data protection frameworks **structurally based on the European model**, with certain limitations and exceptions. **Canada** followed a similar trajectory.

## **3. Privacy vs. Data Protection**

### **The European Distinction**

In Europe, **privacy** and **data protection** are **two legally distinct concepts** and must not be conflated:

- **Privacy** refers strictly to one's _private life_ (religious orientation, intimate relationships, personal beliefs) information that is inherently non-public.
- **Data protection** refers to the protection of _any_ information relating to an identifiable person, _whether that information is public or private_. 

_Example_: The lecturer's own name and university profile are **personal data** subject to data protection law, but they are not "private" in the European sense, since they are publicly available.

### **The American Notion of "Privacy"**

In the **United States**, the term **"privacy"** functions as a **broad constitutional umbrella** that encompasses any form of impact on **self-determination**: personal data protection, same-sex marriage, abortion rights, and much more. This is not arbitrary, it has a specific constitutional origin. The US does not have an explicitly codified right to self-determination, so courts historically **stretched the concept of privacy** to fill that gap.

_**Practical rule**: when reading US literature or legislation, be aware that **"privacy" includes data protection and far more**. When Europeans say "privacy," they refer to something narrower and distinct from data protection. Treating the two as synonymous is a **category error** with real analytical consequences._

## **4.  Historical Origins: Technology–Society–Regulation Pattern**

### **The Recurring Path**

There is a fundamental **recurring pattern** in the history of technology law: 
$$\text{change in technology} → \text{impact on society} → \text{social problem} → \text{regulation}$$
Law enters the picture when there is a problem. If everything works well, no legislation is needed. This is not pessimism, it is the structural function of law.

### **What Is a Mainframe?**

In the **1950s–60s**, the first large-scale computers, **mainframes,** emerged. Their architecture is fundamentally **centralized**: all computational power concentrated in one physical unit (occupying entire rooms, requiring highly specialized staff), with "dumb" terminals connected to it for queries and responses. There was **no internet** in the 1950s; all processing was centralized. Mainframes were extremely large, extremely expensive, and required skills that almost nobody possessed at the time.

### **The Mainframe – AI Parallel**

There is an explicit parallel between **1960s mainframes** and **modern general-purpose AI / Large Language Models**, and it is important to look about it for regulatory thinking.

| **Dimension**           | **Mainframe (1960s)**     | **General-Purpose AI (today)** |
| ----------------------- | ------------------------- | ------------------------------ |
| **Infrastructure**      | Entire dedicated rooms    | Massive data centres           |
| **Cost**                | Extremely high            | Extremely high                 |
| **Skills required**     | Rare, highly specialised  | Rare, highly specialised       |
| **Power concentration** | Centralized in few actors | Centralized in few actors      |

What we regulate, at the end, is **power**. Power was asymmetrically distributed in the mainframe era, and it is asymmetrically distributed again today with AI. Solutions developed to address that earlier power concentration are worth revisiting, which is precisely why studying this history is not merely academic.

## **5. Why Did Governments Invest in Mainframes? Two Drivers**

### **Tax Collection**

The US government was among the first to invest heavily in mainframes, and the primary driver was **tax collection** in order to ensure that all citizens and entities paid their taxes.

_Example_: the **Roman Empire** was economically dominant in part because of its superior capacity to collect taxes. With the fall of the Empire, tax collection collapsed, and this contributed directly to economic disruption. I**nformation about wealth enables redistribution and investment**, which in turn generates economic growth.

### **The Welfare State**

The 1960s and early 70s were the **formative period of the welfare state,** the political model in which the state actively provides to citizens universal services such as education, healthcare, social support, and economic redistribution. The Italian university system is an example because students pay €2,000–3,000 per year in Italy, whereas in the UK it costs £50,000 per year and in the US it can be even higher. The difference reflects the presence or absence of a welfare state logic.

To operate a welfare state, the government must know **who has money and who needs money.** The state needs information about taxes, property, family composition, healthcare needs, and social assistance requirements. Before mainframes, all of this information existed **on paper, fragmented across thousands of archives** (school records, hospital records, municipal records) distributed across cities and institutions. Reconstructing a single individual's complete profile required manually consulting all of them, an enormous, often impossible effort!

The mainframe was a revolution. You centralize all available information, process it overnight (then one day was the processing time) and obtain **a single unified record per person**. One day of processing time was extraordinary compared to the weeks or months it would take to compile the same information manually from paper archives.

## **6. The Cold War Dimension: Data as Instrument of Control**

The fundamental insight that drives all subsequent data protection law is that data can be used for good or for bad. The exact same personal information that enables helpful welfare services (such as healthcare records, income data, and family composition) can easily be **turned against citizens**. This creates a dangerous dual-use tension where centralized data becomes a powerful tool for surveillance, political control, and discrimination.

This dual-use danger was not just a theoretical risk; it was an observable historical reality during the 1960s and 1970s. This era was the height of the **Cold War**, featuring two opposing blocs (Soviet-communist and Western-capitalist) that were deeply suspicious of each other. Both sides actively monitored internal dissent and ideological deviation.

- _Example:_  **House of Leaves museum in Tirana**, Albania, is a concrete illustration of this extreme data surveillance. Disguised externally as a private medical clinic, this small building was actually the headquarters for the Albanian communist secret services. It was equipped with the digital surveillance tools of the era to systematically control the population. The surveillance was so intense that the ratio of informants to monitored citizens was effectively one-to-one, meaning almost everyone was being watched by someone else.

The weaponization of personal data was not exclusive to authoritarian regimes. In the United States (a self-described democracy) the era of **McCarthyism** meant that simply being suspected of being a communist was dangerous. Personal information held by state institutions was used as an instrument of **political repression and persecution**. This proves that the dangers of data concentration existed on both sides of the Iron Curtain.

## 7. **The Social Trade-Off: Birth of Data Protection Law**

Citizens in the 1970s faced a genuine structural **dilemma**. The instinctive response to surveillance risk — _"stop collecting our data"_ — was not actually viable. Stopping data collection would have meant **dismantling the welfare state**, eliminating the very services citizens relied on. The government, equally, could not accept a blanket halt to data collection, it needed information to fund public services and collect taxes. A simple "stop" was therefore a **non-starter for both sides**.

### **The Trade-Off**

The resolution was a **negotiated trade-off**: 
> _"You want my data, and I understand why — it funds services that benefit me. But I will give you my data only in exchange for **safeguards** that reduce the risk of misuse."_ 

This is the **foundational logic of data protection law** — it is not anti-data-collection. It is a set of **conditions** under which **data collection becomes socially acceptable**.

### **The First Generation of Data Protection Laws**

The outcome of this social demand was the **first wave of data protection legislation**, emerging in the **1970s**, which introduced provisions designed to **prevent potential misuse of personal data** through rights, safeguards, and obligations.

The historical approach to data protection is not merely academic decoration, **every component of modern data protection law** (including the GDPR) **reflects an element that emerged from this historical evolution**. Understanding the history is the only way to understand why the law is structured the way it is today.