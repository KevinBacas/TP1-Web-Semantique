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

### Explication :
Cette requête retourne les noms ("label"), des compilateurs cross-platform dans toutes les langues et dans la limite de 100 entrées.

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

### Explication :
Renvoi la liste de tous les noms associés aux liens externes présents sur Wikipédia des compilateurs sous license "Proprietary Software" ou "Public Domain" dans la limite de 100 entrées.

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

### Explication :
Retourne toutes les personnes nées après 1992 et dans la limite de 1000 entrées. Les personnes nées le plus récemment sont affichées en premier.

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

### Explication :
Retourne tous les produits des entreprises de type "Public_company" qui ont plus de 49 employés travaillant dans l'indutrie du "Software" ou du "Consumer_electronics". Les résultats sont regroupés par le nom de l'entreprise et ils sont ordonnés en fonction du revenu des entreprise. Les entreprises au plus fort revenu apparaissent donc en premier. La requête renverra au maximum 1000 entrées.

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

### Explication :
Est-ce que la propriété length de "Amazon_River" est supérieur à "Nile" ? En d'autres mots : Est-ce que l'Amazon est plus long que le Nil ?

### Résultat :
`false`. Le Nil est donc plus grand que l'Amazon. Ce résultat se justifie en regardant les information sur Internet.

## Requête 6 :

```sparql
PREFIX vCard: <http://www.w3.org/2001/vcard-rdf/3.0#>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>
CONSTRUCT {
  ?X vCard:FN ?name .
  ?X vCard:URL ?url .
  ?X vCard:TITLE ?title .
}
WHERE {
  OPTIONAL { ?X foaf:name ?name . FILTER isLiteral(?name) . }
  OPTIONAL { ?X foaf:homepage ?url . FILTER isURI(?url) . }
  OPTIONAL { ?X foaf:title ?title . FILTER isLiteral(?title) . }
}
LIMIT 10
```

### Explication :
On construit un nouveau triplet avec un nom, une URL et un titre avec les propriétés foaf:name pour le nom, foad:homepage pour l'URL et foad:title pour le titre. Dans la limite de 10 entrées.

### Réponse :

```turtle
@prefix vcard:    <http://www.w3.org/2001/vcard-rdf/3.0#> .
<http://dbpedia.org/resource/Believers_(film)>    vcard:FN   "Believers"@en ;
  vcard:URL    <http://www.believersmovie.com> .
<http://dbpedia.org/resource/Believers_(A._A._Bondy_album)>    vcard:FN    "Believers"@en .
<http://dbpedia.org/resource/Believers_(Babylon_5)>    vcard:FN    "Believers"@en .
<http://dbpedia.org/resource/Believers_(Don_McLean_album)>    vcard:FN    "Believers"@en .
@prefix dbr:    <http://dbpedia.org/resource/> .
dbr:Max_Band    vcard:FN    "Band"@en .
<http://dbpedia.org/resource/Band,_Iran>    vcard:FN    "Band"@en .
<http://dbpedia.org/resource/Academy_Award_(radio)>    vcard:FN    "Academy Award"@en .
<http://dbpedia.org/resource/Archive_(band)>    vcard:FN    "Archive"@en ;
  vcard:URL    <http://archiveofficial.com> .
<http://dbpedia.org/resource/Band,_Mure\u0219>    vcard:FN    "Band"@en .
<http://dbpedia.org/resource/Believers_(\u00A1Mayday!_album)>    vcard:FN    "Believers"@en .
```

## Requête 7 :

```sparql
PREFIX foaf:  <http://xmlns.com/foaf/0.1/>
DESCRIBE ?ford WHERE {
  ?ford foaf:name "Bush"@en .
}
LIMIT 1
```

### Explication :
On décrit les ressources qui ont comme nom "Bush" en version anglaise. Dans la limite d'une seule entrée.

### Résultats :

```turtle
@prefix dbo:    <http://dbpedia.org/ontology/> .
@prefix dbr:    <http://dbpedia.org/resource/> .
dbr:Bush    dbo:wikiPageDisambiguates    <http://dbpedia.org/resource/Bush,_Illinois> .
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix yago:    <http://dbpedia.org/class/yago/> .
<http://dbpedia.org/resource/Bush,_Illinois>    rdf:type    yago:Tract108673395 .
@prefix ns4:    <http://schema.org/> .
<http://dbpedia.org/resource/Bush,_Illinois>    rdf:type    ns4:Place ,
        <http://dbpedia.org/class/yago/PopulatedPlacesInWilliamsonCounty,Illinois> .
@prefix owl:    <http://www.w3.org/2002/07/owl#> .
<http://dbpedia.org/resource/Bush,_Illinois>    rdf:type    owl:Thing ,
        dbo:PopulatedPlace ,
        yago:VillagesInIllinois ,
        yago:Village108672738 ,
        dbo:Location ,
        yago:YagoGeoEntity .
@prefix geo:    <http://www.w3.org/2003/01/geo/wgs84_pos#> .
<http://dbpedia.org/resource/Bush,_Illinois>    rdf:type    geo:SpatialThing ,
        yago:YagoPermanentlyLocatedEntity ,
        dbo:Place ,
        dbo:Settlement ,
        yago:Location100027167 ,
        yago:Object100002684 ,
        yago:YagoLegalActorGeo ,
        yago:Site108651247 ,
        yago:Settlement108672562 ,
        yago:Region108630985 ,
        yago:PhysicalEntity100001930 ,
        yago:GeographicalArea108574314 .
@prefix wikidata:    <http://www.wikidata.org/entity/> .
<http://dbpedia.org/resource/Bush,_Illinois>    rdf:type    wikidata:Q486972 .
@prefix ns8:    <http://wikidata.org/entity/> .
<http://dbpedia.org/resource/Bush,_Illinois>    owl:sameAs    ns8:Q2469309 ,
        <http://es.dbpedia.org/resource/Bush_(Illinois)> ,
        <http://yago-knowledge.org/resource/Bush,_Illinois> .
@prefix ns9:    <http://wikidata.dbpedia.org/resource/> .
<http://dbpedia.org/resource/Bush,_Illinois>    owl:sameAs    ns9:Q2469309 ,
        <http://sws.geonames.org/4234905/> ,
        <http://pt.dbpedia.org/resource/Bush_(Illinois)> ,
        <http://nl.dbpedia.org/resource/Bush_(Illinois)> ,
        <http://rdf.freebase.com/ns/m.0sjhm> .
@prefix xsd:    <http://www.w3.org/2001/XMLSchema#> .
@prefix georss:    <http://www.georss.org/georss/> .
<http://dbpedia.org/resource/Bush,_Illinois>    georss:point    "37.841388888888886 -89.13222222222223"^^xsd:string .
@prefix rdfs:    <http://www.w3.org/2000/01/rdf-schema#> .
<http://dbpedia.org/resource/Bush,_Illinois>    rdfs:label    "\u0628\u0648\u0634 (\u0625\u0644\u064A\u0646\u0648\u064A)"@ar ,
        "Bush, Illinois"@en ,
        "Bush (Illinois)"@es ,
        "Bush (Illinois)"@pt ,
        "Bush (Illinois)"@nl ;
    rdfs:comment    "\u0628\u0648\u0634 (\u0625\u0644\u064A\u0646\u0648\u064A) (\u0628\u0627\u0644\u0625\u0646\u062C\u0644\u064A\u0632\u064A\u0629: Bush, Illinois) \u0647\u064A \u0645\u0646\u0637\u0642\u0629 \u0633\u0643\u0646\u064A\u0629 \u062A\u0642\u0639 \u0641\u064A \u0627\u0644\u0648\u0644\u0627\u064A\u0627\u062A \u0627\u0644\u0645\u062A\u062D\u062F\u0629 \u0641\u064A \u0625\u0644\u064A\u0646\u0648\u064A."@ar ,
        "Bush is een plaats (village) in de Amerikaanse staat Illinois, en valt bestuurlijk gezien onder Williamson County."@nl ,
        "Bush is a village in Williamson County, Illinois, United States. As of the 2000 census, the village population was 257."@en .
@prefix foaf:    <http://xmlns.com/foaf/0.1/> .
<http://dbpedia.org/resource/Bush,_Illinois>    foaf:name    "Bush"@en .
@prefix dbp:    <http://dbpedia.org/property/> .
<http://dbpedia.org/resource/Bush,_Illinois>    dbp:name    "Bush"@en ;
    geo:lat    "37.841388702392578125"^^xsd:float ;
    geo:long    "-89.13222503662109375"^^xsd:float ;
    foaf:depiction    <http://commons.wikimedia.org/wiki/Special:FilePath/Illinois_-_outline_map.svg> .
@prefix ns15:    <http://www.w3.org/ns/prov#> .
<http://dbpedia.org/resource/Bush,_Illinois>    ns15:wasDerivedFrom    <http://en.wikipedia.org/wiki/Bush,_Illinois?oldid=628856873> ;
    dbo:abstract    "\u0628\u0648\u0634 (\u0625\u0644\u064A\u0646\u0648\u064A) (\u0628\u0627\u0644\u0625\u0646\u062C\u0644\u064A\u0632\u064A\u0629: Bush, Illinois) \u0647\u064A \u0645\u0646\u0637\u0642\u0629 \u0633\u0643\u0646\u064A\u0629 \u062A\u0642\u0639 \u0641\u064A \u0627\u0644\u0648\u0644\u0627\u064A\u0627\u062A \u0627\u0644\u0645\u062A\u062D\u062F\u0629 \u0641\u064A \u0625\u0644\u064A\u0646\u0648\u064A."@ar ,
        "Bush is a village in Williamson County, Illinois, United States. As of the 2000 census, the village population was 257."@en ,
        "Bush is een plaats (village) in de Amerikaanse staat Illinois, en valt bestuurlijk gezien onder Williamson County."@nl ;
    dbo:areaCode    "618"^^xsd:string ;
    dbo:country    dbr:United_States ;
    dbo:daylightSavingTimeZone    dbr:Central_Time_Zone ;
    dbo:postalCode    "62924"^^xsd:string ;
    dbo:region    <http://dbpedia.org/resource/Williamson_County,_Illinois> ;
    dbo:state    dbr:Illinois ;
    dbo:timeZone    dbr:Central_Time_Zone ;
    dbo:type    dbr:List_of_towns_and_villages_in_Illinois ;
    foaf:isPrimaryTopicOf    <http://en.wikipedia.org/wiki/Bush,_Illinois> .
@prefix virtrdf:    <http://www.openlinksw.com/schemas/virtrdf#> .
<http://dbpedia.org/resource/Bush,_Illinois>    geo:geometry    "POINT(-89.132225036621 37.841388702393)"^^virtrdf:Geometry ;
    dbo:thumbnail    <http://commons.wikimedia.org/wiki/Special:FilePath/Illinois_-_outline_map.svg?width=300> .
@prefix dct:    <http://purl.org/dc/terms/> .
<http://dbpedia.org/resource/Bush,_Illinois>    dct:subject    <http://dbpedia.org/resource/Category:Villages_in_Williamson_County,_Illinois> .
@prefix dbc:    <http://dbpedia.org/resource/Category:> .
<http://dbpedia.org/resource/Bush,_Illinois>    dct:subject    dbc:Villages_in_Illinois ;
    dbo:wikiPageID    112070 ;
    dbo:wikiPageRevisionID    628856873 ;
    dbp:areaCode    618 ;
    dbp:areaImperial    0.46000000000000001998 ;
    dbp:areaLandImperial    0.46000000000000001998 ;
    dbp:areaWaterImperial    0.010000000000000000208 ;
    dbp:category    dbr:List_of_towns_and_villages_in_Illinois ;
    dbp:commons    "Bush, Illinois"@en ;
    dbp:country    "United States"@en ;
    dbp:districtType    "Township"@en ;
    dbp:latD    37 ;
    dbp:latM    50 ;
    dbp:latNs    "N"@en ;
    dbp:latS    29 ;
    dbp:leaderType    "Village president"@en ;
    dbp:longD    89 ;
    dbp:longEw    "W"@en ;
    dbp:longM    7 ;
    dbp:longS    56 ;
    dbp:map    "Illinois - outline map.svg"@en ;
    dbp:mapCaption    "Location of Bush within Illinois"@en ;
    dbp:mapLocator    "Illinois2"@en ;
    dbp:populationDate    2000 ;
    dbp:populationDensityImperial    558 ;
    dbp:postalCode    62924 ;
    dbp:region    <http://dbpedia.org/resource/Williamson_County,_Illinois> ;
    dbp:regionType    "County"@en ;
    dbp:state    "Illinois"@en ;
    dbp:timezone    dbr:Central_Time_Zone ;
    dbp:timezoneDst    dbr:Central_Time_Zone ;
    dbp:utcOffset    -6 ;
    dbp:utcOffsetDst    -5 ;
    dbp:mapBackground    "Illinois - background map.png"@en ;
    dbp:hasPhotoCollection    <http://wifo5-03.informatik.uni-mannheim.de/flickrwrappr/photos/Bush,_Illinois> .
<http://dbpedia.org/resource/Bush,_IL>    dbo:wikiPageRedirects    <http://dbpedia.org/resource/Bush,_Illinois> .
<http://en.wikipedia.org/wiki/Bush,_Illinois>    foaf:primaryTopic    <http://dbpedia.org/resource/Bush,_Illinois> .
```
