# TP1 Web Semantique
Rapport du TP1 de Web Sémantique par Kévin BACAS.

## Requête 1:

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbr: <http://dbpedia.org/resource/>
SELECT DISTINCT ?name
WHERE {
  ?r dbo:genre dbr:Compiler .
  ?r dbo:operatingSystem dbr:Cross-platform .
  ?r rdfs:label ?name
}
LIMIT 10
```

### Résultat :

| name
| ----------------------------
| "GNU Compiler Collection"@en
| "تجميعة مصرفات جنو"@ar
| "GNU Compiler Collection"@de
| "GNU Compiler Collection"@es
| "GNU Compiler Collection"@fr
| "GNU Compiler Collection"@it
| "GNUコンパイラコレクション"@ja
| "GNU Compiler Collection"@nl
| "GNU Compiler Collection"@pl
| "GNU Compiler Collection"@pt

### Explication :
Cette requête retourne les noms ("label"), des compilateurs cross-platform dans toutes les langues et dans la limite de 100 entrées.

## Requête 2:

```sparql
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbp: <http://dbpedia.org/property/>
SELECT ?name ?externalLink
WHERE {
  ?r dbo:genre dbr:Compiler .
  { ?r dbo:license dbr:Proprietary_software }
  UNION
  { ?r dbo:license dbr:Public_domain } .
  ?r dbp:name ?name .
  ?r dbo:wikiPageExternalLink ?externalLink .
}
LIMIT 10
```

### Résultat :

name                                        | externalLink
------------------------------------------- | ------------------------------------------------------------------------------------------
"Silverfrost FTN95: Fortran for Windows"@en | http://www.silverfrost.com
"OpenLisp"@en                               | http://www.softwarepreservation.org/projects/LISP/islisp/
"OpenLisp"@en                               | http://www.iso.org/iso/iso_catalogue/catalogue_ics/catalogue_detail_ics.htm?csnumber=22987
"OpenLisp"@en                               | http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=44338
"OpenLisp"@en                               | http://www.eligis.com/
"OpenLisp"@en                               | http://www.eligis.com
"Lattice C"@en                              | http://web.archive.org/web/20090423134335/http:/www.lattice.com/otherdos.htm
"Lattice C"@en                              | http://www.sas.com/products/sasc/
"Le Lisp"@en                                | http://www.eligis.com/lelisp
"Le Lisp"@en                                | http://www.softwarepreservation.org/projects/LISP/le_lisp/

### Explication :
Renvoi la liste de tous les noms associés aux liens externes présents sur Wikipédia des compilateurs sous license "Proprietary Software" ou "Public Domain" dans la limite de 100 entrées.

## Requête 3 :

```sparql
PREFIX dbp: <http://dbpedia.org/property/>
SELECT DISTINCT ?name
WHERE {
  ?r dbp:dateOfDeath ?dob .
  ?r dbp:name ?name .
  FILTER(?dob > 1992)
}
ORDER BY DESC (?dob)
LIMIT 10
```

### Résultats :

| name
| -----------------------------------------
| "George II, Prince of Anhalt-Dessau"@en
| "Henry VII of England"@en
| "John I, Count Palatine of Simmern"@en
| "Eric II"@en
| "Eric II of Mecklenburg"@en
| "Henry the Younger of Stolberg"@en
| "René II, Duke of Lorraine"@en
| "Ursula of Brandenburg"@en
| "Waldemar VI, Prince of Anhalt-Kothen"@en
| "De Ros, Edmund de 10th Baron de Ros"@en

### Explication :
Retourne toutes les personnes nées après 1992 et dans la limite de 1000 entrées. Les personnes nées le plus récemment sont affichées en premier.

## Requête 4 :

```sparql
PREFIX dbr: <http://dbpedia.org/resource/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX dbp: <http://dbpedia.org/property/>
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
LIMIT 10
```

### Résultats :

| products
| --------------------------------
| dbr:Vehicle_audio
| dbr:Television
| dbr:Vehicle_audio
| dbr:DVD_recorder
| dbr:DVD_recorder
| dbr:DVD_player
| dbr:Automotive_navigation_system
| dbr:DVD_recorder
| dbr:Television
| dbr:DVD_player

### Explication :
Retourne tous les produits des entreprises de type "Public_company" qui ont plus de 49 employés travaillant dans l'indutrie du "Software" ou du "Consumer_electronics". Les résultats sont regroupés par le nom de l'entreprise et ils sont ordonnés en fonction du revenu des entreprise. Les entreprises au plus fort revenu apparaissent donc en premier. La requête renverra au maximum 1000 entrées.

## Requête 5 :

```sparql
PREFIX prop: <http://dbpedia.org/property/>
PREFIX dbr: <http://dbpedia.org/resource/>
ASK
{
  dbr:Amazon_River prop:length ?amazon .
  dbr:Nile prop:length ?nile .
  FILTER(?amazon > ?nile) .
}
```

### Résultat :
`false`. Le Nil est donc plus grand que l'Amazon. Ce résultat se justifie en regardant les information sur Internet.

### Explication :
Est-ce que la propriété length de "Amazon_River" est supérieur à "Nile" ? En d'autres mots : Est-ce que l'Amazon est plus long que le Nil ?
