# Comp estimate coverage buckets

Copied from Notion, "Comp estimate coverage buckets with examples"
(https://app.notion.com/p/juiceboxinc/Comp-estimate-coverage-buckets-with-examples-3fa0f8240c0a8113a162cb363d87d413).
Sized on 2026-10-15. Notion is the source of truth; refresh this copy when it changes.

All 825,948,801 profiles in the index. A profile goes into the first bucket it fails, checked from 7 down to 1. Each covered profile is backed by one source, and its estimate uses only that source. Examples are real profiles from a random 1% sample, shown as title · company · location.

| Bucket | Definition | levels.fyi | Minerva | Total (% of index) | Examples |
| --- | --- | --- | --- | --- | --- |
| 1 Ready | Clean profile with enough data in their country. levels.fyi: 2+ job family/level groups in their country, and the title maps to a role. Minerva: US profile whose exact title has a company average. | ~15,211,576 | 7,779,334 | 22,990,910 (2.8%) | Software engineer · Meta · Bellevue, WA (Minerva); Finance manager · Boeing · Fenton, MO (Minerva); Crew member · McDonald's · Mount Juliet, TN (Minerva); Site reliability engineer · AT&T · Hyderabad, India (levels.fyi) |
| 2A Thin: one levels.fyi group | Clean profile; levels.fyi has only one group in their country | 4,549,833 | – | 4,549,833 | Senior iOS engineer · Polycam · Denver, CO; Customs process administrator · Maersk · Turin, Italy |
| 2B Thin: Minerva, other titles | Clean US profile; Minerva has company averages, but not for their title | – | 9,333,184 | 9,333,184 | Corrections officer · State of Maine · Penobscot, ME; Business development manager · Amergis Healthcare Staffing · San Francisco, CA |
| 2C Thin: title doesn't map | Clean profile; levels.fyi has 2+ groups, but none fit the title (estimated) | ~13,168,230 | – | ~13,168,230 | Editorial and publishing assistant · United Nations · New York, NY; Relationship manager III · The Standard · Stuart, FL |
| 2 Thin data (total) | 2A + 2B + 2C | 17,718,063 | 9,333,184 | 27,051,247 (3.3%) | |
| 3 No data in their country | Clean profile; the source has no rows in their country (Minerva is US-only) | 8,774,912 | 1,444,739 | 10,219,651 (1.2%) | Senior project manager · Royal Caribbean · Turku, Finland (levels.fyi); Software engineer · Certinia · Manchester, UK (Minerva); Senior solutions architect · AWS · Saudi Arabia (Minerva) |
| 4 Fixable profile | Zero months of experience, a one-word title, or a non-English title | 27,373,392 | 14,362,870 | 41,736,262 (5.1%) | Human resources manager · Federal Reserve Bank of SF (0 months); Supervisor · U.S. Dept. of Homeland Security (one word); Assistant · Apple (one word); Diseñador gráfico · Elle Magazine · Mexico City (non-English) |
| 5 Hard role | Founder, owner, self-employed, freelancer, student or retired; intern, part-time, contract or temporary; C-level or president | 4,450,669 | 2,570,573 | 7,021,242 (0.9%) | Owner · Elements · East Meadow, NY; Student · School of Visual Arts · New York, NY; Retired · Ford Motor Company · Huron, OH. False positives: Associate dean of students · UW-Madison; Military helicopter crew chief · US Air Force |
| 6A Minerva global average only | Company is in Minerva with global averages only (not in levels.fyi). Would be unlocked by Minerva's promised company averages. | – | – | 41,673,579 (5.0%) | Truck driver · Unimark Truck Transport · Largo, FL; General dentist · Albrecht Family Dentistry · West Bend, WI |
| 6B In neither source | Company is in neither levels.fyi nor Minerva (86% non-US) | – | – | 366,092,972 (44.3%) | Ingénieur études et développement · Proxiad · Roubaix, France; Center manager · ACE Cash Express · Houston, TX; Responsable de RR HH · Plettac Electronics · Buenos Aires |
| 7 No current role | No current company or no current title | – | – | 309,162,938 (37.4%) | (no title, no company) · Sydney, Australia; Nanny · (no company) · Warwick, RI |
| Total | | 73,528,612 | 35,490,700 | 825,948,801 | |

**Source routing:** A company in only one source uses that source. Minerva counts only company-average rows, not global averages. For a company in both: a US profile whose exact title has a Minerva company average → Minerva; otherwise levels.fyi if it has data in the profile's country; otherwise a US profile → Minerva; otherwise levels.fyi (bucket 3). That routes 10.5M of the 37.2M overlap profiles to Minerva. Today's engine sends all 37.2M to levels.fyi.

**Notes:** "~" figures split 28.4M profiles with the pilot's ~54% title-check pass rate. Bucket 1 and 2C examples are judged by hand, because the title check didn't run on the sample. Bucket 5's keyword rules catch some ordinary roles. Open question: should big-tech profiles with rich levels.fyi data (e.g. Meta, 600 groups) prefer levels.fyi over a Minerva exact title?
