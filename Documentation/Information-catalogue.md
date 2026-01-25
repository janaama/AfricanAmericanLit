# Subject: African-American writers (19th - 21st c.)

## Objects (with their properties - french to English)

### Personne (writer)

- nom: full name of the writer
- date de naissance: year or full DOB
- lieu de naissance: city, state, or country
- origine/context social: family background (if mentioned or available)
- formation: stuides, universities attended (if), or degrees
- occupation: main profession(s) (e.g., novelist, poet, journalist, professor)
- mouvements littéraires: affiliations such as Harlem Renaissance, Black Arts movement, etc.
- engagements / appartenances: social, political, or activist involvement (civil rights, feminism)
- prix / distinctions: literary awards or recognitions
- publications principales: list or number of key works
- période d'activité: approximate years active
- lien Wiki: URL to their article

### Organisation

- nom: name of the institution(s) (e.g., university, association, publisher, literary magazine)
- type: category (acadmeic, political, publishing, artistic)
- siège: main city or place
- fondation: creation date

### Lieu

- nom: name of the place (city, state, country)
- type: category (birthplace, place of work, publication place)  
- coordonnées géographique

### Oeuvre (work)

- titre:title of the work
- année de publication: year published
- genre: novel, poem, essay, play, autobiography
- référence bibliographique: full citation
- lieu de publication: city or publisher
- thèmes principaux: recurring subjects (identity, race, freedom)
- prix/récompenses: if that work received specific awards

### Mouvement littéraire (movement)

- nom: name of the movement
- période: years active
- principes/thématiques: core ideas (e.g., self-expression, racial justice)
- principaux membres: key writers involved

### Distinction (award) (optional)

- nom: name of the award
- année: year received
- catégorie: literary, poetry, fiction, etc.
- institution: organisation that grants it

## Relations entre objets

main relationships to model

- personne est née dans lieu: a writer was born in a place (1 - 1)
- personne a étudié dans organisation: writer studied at an organisation (e.g., university) (0.N -- 0.N)
- personne est membre de organisation: writer belongs to a professional or activist organisation (0.N -- 0.N)
- personne est affiliée à mouvement: writer takes part in a literary movement (0.N -- 0.N)
- personne a reçu distinction: writer received award (0.N -- 0.N)
- oeuvre a pour auteur personne: a work is written by one or more writers (1 - N)
- oeuvre a lieu de publication lieu: a work was published in a specific city/place (1 -- 1)
- organisation est située dans lieu: an organisation has its headquarters in a place (1 -- 1)
