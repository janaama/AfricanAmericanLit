# This page follows the progression of queries used to extract information

* Query 1: fetshing list of writers to input in person table in DBeaver (with one writer "ed bullins" excluded)

```SQl
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>

SELECT DISTINCT ?writer ?writerLabel ?wikidata WHERE {
  # All pages linked from the Wikipedia list page
  dbr:List_of_African-American_writers dbo:wikiPageWikiLink ?writer .

  # Keep only people
  ?writer rdf:type dbo:Person .

  # English label
  ?writer rdfs:label ?writerLabel .
  FILTER (lang(?writerLabel) = "en")

  # EXCLUDE Ed Bullins
  FILTER (?writer != dbr:Ed_Bullins)

  # Optional Wikidata link
  OPTIONAL {
    ?writer owl:sameAs ?wikidata .
    FILTER(STRSTARTS(STR(?wikidata), "http://www.wikidata.org/entity/"))
  }
}
ORDER BY ?writerLabel
```

* Query 2: adding birth dates and death dates in ISO

```SQL
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbp: <http://dbpedia.org/property/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

SELECT DISTINCT
  ?writer
  ?writerLabel
  (STR(?birthISO) AS ?birthDate)
  (STR(?deathISO) AS ?deathDate)
  ?wikidata
WHERE {
  # Writers from the list page
  dbr:List_of_African-American_writers dbo:wikiPageWikiLink ?writer .
  ?writer rdf:type dbo:Person .
  FILTER (?writer != dbr:Ed_Bullins)

  # Label
  ?writer rdfs:label ?writerLabel .
  FILTER (lang(?writerLabel) = "en")

  # Raw birth/death values
  OPTIONAL { ?writer dbo:birthDate ?b1 . }
  OPTIONAL { ?writer dbp:birthDate ?b2 . }
  OPTIONAL { ?writer dbo:deathDate ?d1 . }
  OPTIONAL { ?writer dbp:deathDate ?d2 . }

  # Normalize to ISO dates
  BIND(
    IF(BOUND(?b1),
       xsd:date(?b1),
       IF(BOUND(?b2) && STRLEN(STR(?b2)) >= 4,
          xsd:date(CONCAT(SUBSTR(STR(?b2),1,4), "-01-01")),
          UNDEF
       )
    ) AS ?birthISO
  )

  BIND(
    IF(BOUND(?d1),
       xsd:date(?d1),
       IF(BOUND(?d2) && STRLEN(STR(?d2)) >= 4,
          xsd:date(CONCAT(SUBSTR(STR(?d2),1,4), "-01-01")),
          UNDEF
       )
    ) AS ?deathISO
  )

  # Wikidata ID (for joins later)
  OPTIONAL {
    ?writer owl:sameAs ?wikidata .
    FILTER(STRSTARTS(STR(?wikidata), "http://www.wikidata.org/entity/"))
  }
}
ORDER BY ?writerLabel
```
