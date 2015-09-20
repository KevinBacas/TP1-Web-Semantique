# TP1 Web Semantique

## Requête 1:
```sparql
SELECT DISTINCT ?name
WHERE {
  ?r dbo:genre dbr:Compiler .
  ?r dbo:operatingSystem dbr:Cross-platform .
  ?r rdfs:label ?name
}
LIMIT 100
```

### Explication :
Cette requête retourne les noms ("label"), des compilateurs cross-platform dans toutes les langues et dans la limite de 100 entrées.

## Requête 2:
```sparql
SELECT ?name ?externalLink
WHERE {
  ?r dbo:genre dbr:Compiler .
  { ?r dbo:license <http://dbpedia.org/resource/Proprietary_software> }
  UNION
  { ?r dbo:license dbr:Public_domain } .
  ?r dbp:name ?name .
  ?r dbo:wikiPageExternalLink ?externalLink .
}
LIMIT 100
```
### Explication :
Renvoi la liste de tous les noms associés aux liens externes présents sur Wikipédia des compilateurs sous license "Proprietary Software" ou "Public Domain" dans la limite de 100 entrées.

## Requête 3 :
```sparql
SELECT DISTINCT ?name
WHERE {
  ?r dbp:dateOfDeath ?dob .
  ?r dbp:name ?name .
  FILTER(?dob > 1992)
}
ORDER BY DESC (?dob)
LIMIT 1000
```

### Explication :
Retourne toutes les personnes nées après 1992 et dans la limite de 1000 entrées. Les personnes nées le plus récemment sont affichées en premier.


## Requête 4 :
```sparql
SELECT ?products
WHERE {
  ?r dbo:numberOfEmployees ?nbEmployees .
  ?r dbo:product ?products .
  { ?r dbo:industry dbr:Software }
  UNION
  { ?r dbo:industry dbr:Consumer_electronics } .
  ?r dbp:name ?name .
  ?r dbo:operatingIncome ?income .
  ?r dbo:type dbr:Public_company .
  FILTER(?nbEmployees > 49)
}
GROUP BY (?name)
ORDER BY DESC (?income)
LIMIT 1000
```

### Explication :
Retourne tous les produits des entreprises de type "Public_company" qui ont plus de 49 employés travaillant dans l'indutrie du "Software" ou du "Consumer_electronics". Les résultats sont regroupés par nom de l'entreprise et ordonnés en fonction du revenu des entreprise. Les entreprises au plus fort revenu apparaissent en premier. La requête renverra au maximum 1000 entrées.
