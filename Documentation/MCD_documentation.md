# Conceptual Data Model – African-American Writers

## Scope

Population: Writers listed on Wikipedia’s “List of African-American writers” (19th–21st c.).  
Purpose: Prosopographical analysis of careers, networks, works, and recognition.

## Entities (Classes) and Attributes

### Writer

- **name**: full name as used in Wikipedia page title.
- **birthDate** (date or year): ISO 8601 if available.
- **birthPlace** (Place, 1..1): city/state/country at birth.
- **gender**: as stated by the source.
- **wikipediaLink** (url): article URL.
- **periodActive** (years): approximate active years.

### Work

- **title**: work title.
- **year** (year): publication year.
- **genre**: novel, poetry, essay, drama, etc.
- **publicationPlace** (Place, 1..1): city/state/country.
- **awards** (Award, 0..n): awards tied to this work (if specific).

### Movement

- **name**, **period**, **notes** (optional).

### Organisation

- **name**, **type** (university, association, publisher, journal, etc.), **locatedIn** (Place, 1..1).

### Award

- **name**, **category** (poetry/fiction/etc.), **grantingOrganisation** (Organisation, 1..1), **year**.

### Place

- **city**, **state**, **country**, **geo**.

## Relationships (Semantics & Cardinalities)

- **Writer —writes→ Work**: 1 writer writes 0..n works; each work has 1 main writer (co-authors can be added as additional Writer–Work links).
- **Writer —member_of→ Movement**: 0..n ↔ 0..n (membership or affiliation).
- **Writer —affiliated_with→ Organisation**: 0..n ↔ 0..n (employment, editorial board, association).
- **Writer —received→ Award**: 0..n ↔ 0..n (awards to persons).
- **Award —granted_by→ Organisation**: 0..n awards granted by 1 organization.
- **Writer —born_in→ Place**: 1 ↔ 1 (birth event location).
- **Work —published_in→ Place**: 1 ↔ 1 (publication city).
- **Organisation —located_in→ Place**: 1 ↔ 1 (HQ or main location).

## Notes on Data Source and Coding

- Primary source: Wikipedia biographies and list pages.
- If an attribute is missing, leave null and keep a source note when possible.
