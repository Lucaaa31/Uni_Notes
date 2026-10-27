# Filing a patent
1. **Inventor:** has the idea and writes an **invention disclosure** → goes to the firm's IP department (or a university's **TTO**). The inventor is always named on the patent.
2. **Patent attorney:** runs a first prior-art search and drafts description, drawings, abstract and above all the **claims** (broad enough to protect, narrow enough to be valid).
3. **Filing:** submitted to an office, fees paid → application number + **filing date**. The **first filing** fixes the date: the clock starts here.

### Where to file?
A patent is a **territorial** right → the applicant must decide where protection is worth paying for. Drivers: markets, budget (fees × countries), time before committing, language and rules.
- **Rights are national:** one patent per country
- **Disclosure is global:** a publication anywhere enters the worldwide prior art and destroys novelty everywhere

**WIPO** (UN agency, Geneva) administers the Paris Convention and the PCT but **does not grant patents**. Offices grant patents; the **IP5** are EPO, USPTO, JPO, CNIPA, KIPO.

---

# The patent document
The front page fields are identified by numeric codes:

![[01 - Patents - From Filing to Post-grant Challenges-1793120281772.webp|420]]

| Code | Field | What to remember |
|---|---|---|
| (11) | Publication number | office + serial number + **kind code** (e.g. EP 3757021 B1) |
| (21) | Application number | stays the same from filing to grant: the stable key linking all publications of one application (PATSTAT `appln_id`) |
| (22) | Filing date | the 20-year term is counted from here |
| (43) | Publication of application | ~18 months after filing |
| (45) | Grant date | |
| (51) | Int. Cl. | IPC codes assigned by the examiner |
| (54) | Title | in EN/DE/FR, often deliberately generic |
| (84) | Designated states | EP only. **Designation ≠ protection**: after grant the owner chooses where to validate |
| (71)/(73) | Applicant / Proprietor | who files / who owns after grant |
| (72) | Inventors | right to be named, cannot be transferred |
| (74) | Representative | the attorney firm |
| (56) | References cited | backward citations |

### Kind codes (EPO)
- **A1:** application with search report
- **A2:** application without search report
- **A3:** search report published later
- **B1:** granted patent

One invention produces several documents: A1 and B1 are two different records → link them through the application number, **never count both**.

### Technological classification: IPC vs CPC
- **IPC** (International Patent Classification): managed by WIPO, used by 100+ offices
- **CPC** (Cooperative Patent Classification): extension of the IPC with finer subdivisions, managed jointly by EPO and USPTO (used in PATSTAT)

Codes are read from left to right: section › class › subclass › main group › subgroup.

![[01 - Patents - From Filing to Post-grant Challenges-1793120330503.webp|650]]

### What each field is useful for (in data analysis)
- **Owners:** the "name game", one owner under many spellings (Siemens AG, SIEMENS AKTIENGESELLSCHAFT…) → harmonise names (PATSTAT `han_name`)
- **Inventors:** addresses show where R&D is actually done (often not where the owner is registered); co-inventors form collaboration networks
- **Representatives:** rarely used, but reveal who manages a firm's IP
- **Citations:** **backward** = what the invention draws on; **forward** (received later) = classic indicator of a patent's influence

## Text: abstract, description, claims
- **Abstract:** ~150 words, a search aid. Most used field in NLP, but **not legal** (for legal tasks use the claims)
- **Description:** explains the invention so that an expert could reproduce it (prior art and drawbacks, problem, solution, ways of carrying it out)
- **Claims:** the legal heart, they define the scope of protection
	- **Independent claims:** the broadest definition
	- **Dependent claims:** narrower versions that refer back

## Search report
Drawn up by the examiner (~6–12 months) and published with the application at 18 months. Lists the relevant prior-art documents with a category:
- **X:** particularly relevant on its own → novelty or inventive step at stake
- **Y:** relevant when combined with another document
- **A:** technological background, not threatening

These references become the **backward citations** in patent databases.

---

# From application to grant
![[01 - Patents - From Filing to Post-grant Challenges-1793120379234.webp|700]]

- **Application vs patent:** filed but not yet granted = **pending**; granted = final
- **Applicant** = who files; **assignee** = who owns the rights at a given point in time
- The owner can **license from the day of filing**
- The right ends with refusal, withdrawal, revocation, unpaid renewal fees, or expiry

### Should applications be published before grant?
- **Yes:** inventors signal early to licensees and investors; rivals avoid duplicating R&D; knowledge spreads faster
- **No:** small inventors may be copied or "invented around" by larger rivals; **notice pollution** (abandoned, low-quality applications still block others as prior art)

---

# Patent families
## Three routes to protection
Rights are always national in the end; what changes is how many offices you deal with, and when.
1. **National route:** one application per office, separate procedures, national law and language. Costs add up → suits applicants who need few markets.
2. **European route (EPC, 1973):** one application at the EPO (EN, FR or DE), one search and one examination for up to 39 member states → one European patent that becomes a **bundle of national patents**.
3. **International route (PCT, 1970):** one application through WIPO for 150+ countries; decide where to go at month 30.

**Priority:** after the first filing you have **12 months** to file in other countries keeping the original **priority date**: anything published in between does not destroy novelty (Paris Convention).

### EPO validation
![[01 - Patents - From Filing to Post-grant Challenges-1793120427965.webp|450]]

1. **One procedure** (~4 years): filing, search, publication at 18 months, examination
2. **Grant** once, by the EPO, published in the European Patent Bulletin
3. **Validation:** within a short deadline (usually 3 months) the owner picks the states, paying a fee and filing a translation where required
4. Then a **bundle of national patents**: national renewal fees, national courts, country-by-country lapses

In practice validation concentrates in DE (94%), FR (74%), GB (68%), then IT (31%).

### Paris route vs PCT route
![[01 - Patents - From Filing to Post-grant Challenges-1793120476696.webp|700]]

- **Paris route:** separate filings at each office, each claiming the priority. Faster and cheaper, but all countries must be chosen and paid for at **month 12**.
- **PCT route:** one international application (WO) at month 12 → international search report and written opinion on patentability (~16 m) → publication (18 m) → **national/regional phase at month 30** (EPO: 31). More expensive, but a single application and more time to choose countries.

## What is a patent family
**Patent family:** the set of applications filed at different offices that protect the same invention and claim the same priority. **One invention = one family**, however many documents and countries.
- **Priority date:** date of the first filing
- **Extension:** a filing abroad that claims that priority
- **Simple family (DOCDB):** exactly the same priorities → `docdb_family_id` in PATSTAT (the one to use in the PW)
- **Extended family (INPADOC):** linked by at least one shared priority, directly or through other documents → larger

In databases each document is a separate record: count **families**, not documents.

---

# Some figures from the EPO
- EPO is the **5th office** in the world by applications (~194k in 2022), after China, US, Japan and Korea
- **More than half** of EPO applications come from outside Europe (US 24.9%, Asia 26.4%) → the EPO is the gateway to the European market
- 72% large enterprises, 18% SMEs and individual inventors, 10% universities and public research
- Top applicants are dominated by ICT firms (Huawei, LG, Qualcomm, Samsung, Ericsson)

## The Unitary Patent (since 1 June 2023)
After grant the owner can request **unitary effect** within one month, at no fee.

| | European patent | Unitary patent |
|---|---|---|
| Territory | states validated one by one | all participating EU states at once (18; Spain and Croatia out) |
| Translations | required by many states | one only |
| Renewal fees | paid to each national office | one fee to the EPO (cheaper if you need ≥4 countries) |
| Courts | national, possibly different outcomes | **Unified Patent Court**: one decision for all states |
| Revocation | country by country | everywhere at once |
| Flexibility | can drop countries later to save fees | all or nothing |

---

# Challenging a patent after grant
## Opposition at the EPO
Administrative procedure to **revoke or limit** a granted European patent **centrally**, for all states at once, without going to court.
- **Who:** anyone except the owner, even a **straw man** acting for someone who wants to stay anonymous
- **When:** within **9 months** of the publication of the grant
- **Grounds:** not patentable (no novelty, no inventive step…), insufficient disclosure, or the patent goes beyond the application as filed
- **Procedure:** notice + fee → opposition division (3 examiners) → written exchange → oral proceedings → decision → appeal to the Boards of Appeal
- **Outcomes:** opposition rejected, patent revoked, or maintained in **amended (narrower) form**, valid in all states
- ~34 months, €880 fee, €5–10k in total for simple cases
- ~**5%** of EPO patents are opposed (2018: ~40% revoked, ~32% amended, ~29% rejected)

## USPTO: the PTAB (since 2012)
| | EPO opposition | Post Grant Review (PGR) | Inter Partes Review (IPR) |
|---|---|---|---|
| When | within 9 months of grant | within 9 months of grant | after the 9-month PGR window |
| Who | anyone, even anonymously | must name all real parties in interest | must name all real parties in interest |
| Grounds | novelty, inventive step, insufficient disclosure | novelty, obviousness, written description, enablement | only prior art (patents, printed publications) |
| Total cost | €5–10k | $100–250k | $50–150k |

Much rarer in the US: **0.46%** of US patents challenged at the PTAB vs ~5% opposed at the EPO.

## Invalidity vs infringement
| | Invalidity | Infringement |
|---|---|---|
| Question | should this patent exist at all? | is someone using what the claims protect without permission? |
| Who starts it | anyone who feels threatened | the patent owner |
| Where | patent office (opposition, PGR, IPR) or court | court only |
| When | office: time limits; court: any time | any time during the patent's life |

**They travel together:** sued for infringement, the defendant almost always counter-attacks the validity of the patent.
