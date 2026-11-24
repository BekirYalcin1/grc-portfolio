# Information Security Risk Assessment: Kestrel Shift Ltd

> Kestrel Shift Ltd is a fictional company created for this work sample. Any resemblance to a real organisation is coincidental.

| | |
|---|---|
| Assessment date | 24 November 2026 |
| Prepared by | Bekir Yalcin |
| Method | Asset-based assessment aligned with ISO/IEC 27005:2022 |
| Control reference | ISO/IEC 27001:2022 Annex A |
| Next review | November 2027, or after a significant change or incident |

**Files**

- [`risk-register.csv`](risk-register.csv): the full register, readable in the browser
- [`risk-register.xlsx`](risk-register.xlsx): the working file, with scoring formulas, criteria and a summary sheet

## 1. The company

Kestrel Shift is a B2B SaaS company based in Cork with 50 staff. It sells staff rostering software to hotels and restaurants in Ireland and the UK.

- About 400 customer organisations and roughly 60,000 end users (the customers' employees)
- Processes personal data on behalf of its customers: names, contact details, working hours, availability and pay rates. Kestrel Shift is a data processor under GDPR for this data and a controller for its own staff and sales data
- Hosted on AWS in a single EU region. Eight engineers, one of whom looks after infrastructure
- No dedicated security role. The CTO is responsible for security alongside everything else
- Larger customers have started sending security questionnaires and asking about ISO 27001. That is the reason for this assessment

## 2. Scope

In scope: the production platform, the systems used to build and run it, staff devices and accounts, suppliers that handle customer data, and the processes around joiners, leavers, change and incidents.

Out of scope: the customers' own systems, and financial risks that are not caused by an information security event.

## 3. Method

1. Listed the information assets and the processes that depend on them
2. For each, identified realistic threats and the weaknesses that would let them succeed
3. Recorded the controls already in place
4. Scored likelihood and impact from 1 to 5 with those controls in place (inherent score = likelihood x impact)
5. Compared each score with the acceptance criteria and chose a treatment option
6. Selected controls from ISO 27001:2022 Annex A, named an owner and a target date
7. Scored again assuming the planned controls are working (residual score)

Information came from interviews with the CTO, Head of Engineering, DevOps Lead and People & Operations Manager, and from a review of cloud, identity and repository settings.

### Likelihood

| Score | Level | Meaning |
|---|---|---|
| 1 | Rare | Not expected in the next 3 years |
| 2 | Unlikely | Could happen once in 3 years |
| 3 | Possible | Could happen once a year |
| 4 | Likely | Expected several times a year |
| 5 | Almost certain | Expected monthly or already happening |

### Impact

| Score | Level | Meaning |
|---|---|---|
| 1 | Negligible | No customer effect; under EUR 1,000; fixed within a day |
| 2 | Minor | A few customers affected; under EUR 10,000; no personal data involved |
| 3 | Moderate | Service degraded for hours or limited personal data exposed; under EUR 50,000 |
| 4 | Major | Outage over 4 hours or reportable personal data breach; under EUR 250,000; customers lost |
| 5 | Severe | Large-scale breach or multi-day outage; over EUR 250,000; threatens the business |

### Acceptance criteria

| Rating | Score | Required action |
|---|---|---|
| Low | 1-5 | Accept. Review at the next assessment |
| Medium | 6-10 | Treat, or accept with written approval from the risk owner |
| High | 12-16 | Must be treated. Plan approved within 30 days, controls in place within 90 days where feasible |
| Critical | 20-25 | Treat immediately. Report to the CEO the same day |

Treatment options follow ISO 27005: **modify** (add or improve controls), **retain** (accept), **avoid** (stop the activity) and **share** (insurance or contract).

## 4. Results

17 risks were identified.

| Rating | Before treatment | After treatment |
|---|---|---|
| Critical | 0 | 0 |
| High | 13 | 0 |
| Medium | 3 | 12 |
| Low | 1 | 5 |
| Average score | 12.4 | 6.0 |

| ID | Risk | Inherent | Option | Residual | Owner | Target |
|---|---|---|---|---|---|---|
| R01 | Phishing leads to a compromised staff account | 16 High | Modify | 8 Medium | CTO | 15 Jan 2027 |
| R12 | Credentials committed to source code | 16 High | Modify | 8 Medium | Head of Engineering | 15 Jan 2027 |
| R14 | Attack goes unnoticed (no central logging or alerts) | 16 High | Modify | 8 Medium | CTO | 19 Feb 2027 |
| R02 | Customer files exposed by cloud storage misconfiguration | 15 High | Modify | 5 Low | DevOps Lead | 31 Jan 2027 |
| R05 | Known vulnerability exploited in production | 15 High | Modify | 8 Medium | Head of Engineering | 31 Jan 2027 |
| R08 | Backups cannot be restored | 15 High | Modify | 6 Medium | DevOps Lead | 31 Jan 2027 |
| R03 | Ransomware on staff laptops and shared drive | 12 High | Modify / Share | 6 Medium | CTO | 15 Feb 2027 |
| R04 | Leavers keep access to systems | 12 High | Modify | 6 Medium | People & Operations Manager | 15 Feb 2027 |
| R07 | Breach at a sub-processor | 12 High | Modify | 8 Medium | Data Protection Lead | 19 Feb 2027 |
| R10 | GDPR request or breach handled late | 12 High | Modify | 6 Medium | Data Protection Lead | 19 Feb 2027 |
| R13 | Unreviewed change released to production | 12 High | Modify | 6 Medium | Head of Engineering | 31 Jan 2027 |
| R15 | Infrastructure depends on one person | 12 High | Modify | 6 Medium | CTO | 19 Feb 2027 |
| R16 | Real customer data in test environments | 12 High | Avoid | 3 Low | Head of Engineering | 31 Jan 2027 |
| R06 | Hosting outage | 10 Medium | Modify | 6 Medium | DevOps Lead | 31 Mar 2027 |
| R09 | Insider misuse of production data | 10 Medium | Modify | 5 Low | Head of Engineering | 28 Feb 2027 |
| R11 | Lost or stolen laptop | 9 Medium | Modify | 3 Low | CTO | 31 Mar 2027 |
| R17 | Physical intrusion at the shared office | 4 Low | Retain | 4 Low | CEO | Review Nov 2027 |

Threat, vulnerability, existing controls, planned controls and Annex A references for each risk are in the register.

## 5. What the results say

**Most of the high risks share three root causes.**

1. **Identity is not managed centrally.** MFA is optional, there is no single sign-on, leavers are removed by hand and every engineer has standing access to production (R01, R04, R09). One project, an identity provider with enforced MFA and time-limited production access, lowers all three.
2. **Nothing is checked automatically.** Cloud configuration, dependencies, secrets and code changes all rely on someone remembering (R02, R05, R12, R13). Adding these checks to the CI pipeline costs little and removes four high risks.
3. **Recovery and detection have never been tested.** Backups exist but have not been restored, logs exist but nobody looks at them, and recovery knowledge sits with one person (R08, R14, R15, R06). The company would probably find out about a breach from a customer.

**Recommended order of work**

| Period | Work | Risks addressed |
|---|---|---|
| First 30 days | Enforce MFA; turn on secret scanning and rotate exposed keys; block public access on all storage; stop copying production data to staging | R01, R12, R02, R16 |
| 30 to 60 days | Patch deadlines and CI scanning; branch protection; first backup restore test; backups copied to a separate account | R05, R13, R08 |
| 60 to 90 days | Single sign-on and leaver process; central logging and alerts; incident response plan; supplier register; GDPR procedures; runbooks | R04, R14, R07, R10, R15, R03 |
| After 90 days | Multi-AZ database and recovery test; device management; time-limited production access | R06, R11, R09 |

## 6. Residual risk

After treatment no risk is rated high. Twelve remain medium. These need written acceptance from their owners once the controls are in place and shown to work, because a planned control does not reduce risk until it operates.

Seven residual risks keep an impact score of 4 or 5 even with low likelihood (R02, R09 at impact 5; R01, R05, R07, R12, R14 at impact 4). The controls make these events less likely but would not make them less damaging. They are the ones to watch at each review.

R17 is retained without treatment. The score is within the acceptance level and the cost of extra physical controls in a shared building is out of proportion to the loss.

## 7. Assumptions and limits

- Scores are qualitative and reflect the judgement of the people interviewed. They are useful for ranking, not as a financial forecast
- Impact bands in euro assume annual revenue of about EUR 4 million
- Residual scores assume the planned controls are implemented as described and are operating. They should be confirmed by internal audit
- The assessment covers the situation on the assessment date. New products, suppliers or incidents call for an update
