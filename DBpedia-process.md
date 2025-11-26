# DBpedia Process

## General List of African American Writers

Executed
Query on SPARQL:
SELECT DISTINCT ?writer ?label
WHERE {
  dbr:List_of_African-American_writers dbo:wikiPageWikiLink ?writer .
  ?writer rdf:type foaf:Person .
  OPTIONAL {
    ?writer rdfs:label ?label .
    FILTER (lang(?label) = 'en')
  }
}
ORDER BY ?label
LIMIT 500

## Specific Writer in DBpedia: Toni Morrison

### 1. When I query DBpedia for Toni Morrison, I get some properties

`dbo:award`(with values such as National Humanities Medal);

`dbo:education` (with values such as Cornell University and Howard University);

and a large number of `dbo:wikipagewikilink` properties that point to categories or related pages (e.g. African-American novelists, Pulitzer Prize for Fiction winners, etc.).

HOWEVER, I do not get a dbo:birthDate property in my results, even though the date of birth is present in Wikipedia. I am not sure if this is a limitation of DBpedia or a mistake on my part in the code.

### 2. SPARQL query

 query in SPARQL:
PREFIX dbr:  <http://dbpedia.org/resource/>
PREFIX dbo:  <http://dbpedia.org/ontology/>
PREFIX dbp:  <http://dbpedia.org/property/>
PREFIX schema: <http://schema.org/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT ?p ?pLabel ?o ?oLabel
WHERE {
  dbr:Toni_Morrison ?p ?o .

  FILTER(
    STRSTARTS(STR(?p), STR(dbo:)) ||
    STRSTARTS(STR(?p), STR(dbp:)) ||
    STRSTARTS(STR(?p), STR(schema:))
  )

  OPTIONAL {
    ?p rdfs:label ?pLabel .
    FILTER (lang(?pLabel) = 'en')
  }

  OPTIONAL {
    ?o rdfs:label ?oLabel .
    FILTER (lang(?oLabel) = 'en')
  }
}
LIMIT 200

?p = property
?o = value

## Most frequent properties for the whole population of African American Writers

Question: Across all African-American writers, which properties are most common?
To better understand which types of structured information DBpedia provides for African-American writers, I used a SPARQL query that counts how often each ontology property (`dbo:`) appears among all the individuals extracted from the list. Purpose: identifying what kinds of biographical elements are most consistently available, and which are missing.

### 1. SPARQL query

PREFIX dbo:  <http://dbpedia.org/ontology/>
PREFIX dbr:  <http://dbpedia.org/resource/>
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?p (COUNT(DISTINCT ?writer) AS ?count)
WHERE {
  dbr:List_of_African-American_writers dbo:wikiPageWikiLink ?writer .
  ?writer rdf:type foaf:Person .
  ?writer ?p ?o .
  FILTER(STRSTARTS(STR(?p), STR(dbo:)))
}
GROUP BY ?p
ORDER BY DESC(?count)
LIMIT 500

### 2. Most frequent properties

1. WikipageWikiLink:
(355): most common property, but not descriptive I believe. Mainly links writers to categories and related Wiki pages (contextual linking and not biographical information)

2. Description:
(355): textual abstracts from Wikipedia. Narrative context.

3. birthPlace:
(320): one of the most consistent properties (Person → Place) in conceptual model.

4. birthDate:
(292): did not appear in my test writer (Toni Morrison), but birth dates are available in the overall population. Works as attribute in "person" box.

5. thumbnail:
(262): image of the individual. Not necessarily relevant here.

6. deathDate and deathPlace:
(169 and 150 respectively): optional attributes in "person" table.

7. birthName:
(160): name changes. not sure if useful in this study.

8. education and almaMater:
(112 and 83 respectively): links between writers to universities and/or educational intitutions. works for my Person - organisation relationship.

9. award:
(72): moderately frequent. add award entity and person-award link table in SQL database.

10. genre:
(58): literary genre.

11. notableWork:
(52): important publication - corresponds to Work entity.

12. spouse, relative and child:
(52, 22, 21 respectively): not central to project.

13. pseudonym:
(18): not essential

14. movement:
(17): Literary or political movements (for example, Harlem Renaissance, Black Arts Movement). Rare in DBpedia but important for a sociological study of the field. This means that movement affiliation must be manually extracted from Wikipedia and encoded in my own database.

### What this means for my project

These frequency results show that: (1) basic biographical data (i.e. birth place and date, occupation, education) is available in DBpedia and maps to the structure of my conceptual model. (2) Awards and notable works appear often enough to justify their inclusion as entities in the database. (3) Movements and organisational memberships are not well represented in DBpedia, but are crucial for the prosographical study (must be manually collected from Wikipedia). (4) family data is not essential.

### Conclusion

DBpedia provided data for the most common biographical elements of African Aerican writers. It aligns with many of the entities and relationships in my CM (Person, Place, Organization, Award, Work). However, important factors such as literary or political movements, group affiliations etc, are missing from DBpedia and have to be extracted (?) from wikipedia.
