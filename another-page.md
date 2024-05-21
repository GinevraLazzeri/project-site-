---
layout: default
title: Cocktail Dress 
description: New Triple 1 & 2
---

## Methodology

We started our project by browsing different **ArCo's ontologies**. We chose the “Clothing Description Ontology” as the main subject of our work. 
We used **Wikidata** to discover the existing types of clothing on the platform (ex. vestito da sera, vestito da ballo, abito da cocktail...).

We then asked an LLM (**ChatGPT**) to create a SPARQL query to find “various typologies of dresses on Wikidata”. As a prompt engineering technique, we used *Chain-of-Thought* (i.e. Chain-of-Thought + Zero-shot):

![ChatGPTCoT2](/immagini_markdown/CoT2.png)

```SPARQL
SELECT ?dressType ?dressTypeLabel ?dressTypeDescription
WHERE {
?dressType wdt:P279 wd:Q220721.
SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
OPTIONAL { ?dressType schema:description ?dressTypeDescription. FILTER(LANG(?dressTypeDescription) = "en") }
}
ORDER BY ?dressTypeLabel
```

![ChatGPTCoT1](/immagini_markdown/CoT1.png)

Although the query is already well structured, we have to fix a few errors:
- instead of giving us the item/object **"dress" (Q200539)**, it gave us that of "280th Infantry Division (Wehrmacht)" (Q220721) which has nothing to do with it;
- since the clothes/garments on ArCo have property values and descriptions in Italian, we also want to find the labels in Italian in the Wikidata query. For this reason we need to **change the language in the SERVICE tag**: instead of "[AUTO_LANGUAGE],en" we put "it" (= Italian);
- likewise, we want the language of the variable ?dressTypeDescription to be Italian in the OPTIONAL tag, meaning that we want the dress type description to be in Italian. This is why we need to **change "en" (= english) to "it" (= italian) in the FILTER tag**.

The following is the corrected version of the query to find the elements that are subclasses of “dress” (i.e. the types of dress/garments) with labels in Italian. If present, it also provides their description in Italian (OPTIONAL tag). Everything is ordered alphabetically on the basis of the ?dressTypeLabel:

```SPARQL
SELECT ?dressType ?dressTypeLabel ?dressTypeDescription
WHERE {
?dressType wdt:P279 wd:Q200539.
SERVICE wikibase:label { bd:serviceParam wikibase:language "it". }
OPTIONAL { ?dressType schema:description ?dressTypeDescription. FILTER(LANG(?dressTypeDescription) = "it") }
}
ORDER BY ?dressTypeLabel
```

Our query produces 93 results. Some of them do not have a literal as a label, but it is not relevant as this query only serves as an inspiration for us to find a type of dress from which to start our searches on ArCo. These below are some of the results produced (among which we also find "cocktail dress"). See results [at this link](https://w.wiki/A5kR)  

![Results_wikidata](/immagini_markdown/Results_wikidata.png)

Looking at these results, we chose the category **'cocktail dress'**. We then switched to **ArCo** where we noticed that many of the dresses had the description 'cocktail dress', but on the page it was not possible to find an IRI associated with that dress under the property value 'cocktail dress'.

Following this remark, we decided to work with the _Ontology Clothing Description_ since clothes generally have many characteristics, but these characteristics do not have a corresponding IRI on ArCo.

_ex._ A dress has the description "cocktail dress" but on the page it is not possible to find an IRI associated with that dress that places it under the property value "cocktail dress". 

Therefore, having new triples to create as goal, we suggest the following new triple:

That dress (subject) - belongs to the class (predicate) - cocktail dress (object) 

We then link the subject to an IRI so that by performing a search I can find that dress in that property value class.

We decided to run QUERY asking if there are any properties with “da cocktail” as a label. True or false? The answer is TRUE.

``` SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

ASK { 
?clothing a arco:HistoricOrArtisticProperty ;
rdfs:label ?label 
FILTER(REGEX(?label,  "da cocktail", "i"))
}
```
Since we got an affirmative answer, we ran a QUERY to search all the items with “da cocktail” in the label present in the whole ArCo Knowledge Graph. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=%0D%0APREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fclothing+%3Flabel+%0D%0AWHERE+%7B+%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel+%0D%0AFILTER%28REGEX%28%3Flabel%2C++%22da+cocktail%22%2C+%22i%22%29%29%0D%0A%7D%0D%0ALIMIT+20%0D%0A%0D%0A%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on). 

``` SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?clothing ?label 
WHERE { 
?clothing rdfs:label ?label 
FILTER(REGEX(?label,  "da cocktail", "i"))
}
LIMIT 20
``` 

Since we previously put LIMIT 20, we decided to count how many “da cocktail” items there are on the ArCo KG. We could not get any results since the SPARQL research goes in time out. 

``` SPARQL
SELECT DISTINCT ?clothing ?label COUNT (DISTINCT ?clothing) AS ?n

WHERE { 
?clothing rdfs:label ?label 
FILTER(REGEX(?label,  "da cocktail", "i"))
}
```

The query does not work. It goes *time out*.

We then decided to check the number of “da cocktail” dress class by class (looking for “clothing” in the ArCo Ontology), aiming to narrow the field and get clarity. 

We ran this QUERY to get the number of “da cocktail” dresses present in the class HistoricOrArtisticProperty. The result is 52. See result [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+COUNT%28DISTINCT+%3Fclothing%29+AS+%3Fn%0D%0AWHERE+%7B+%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel+%3B+%0D%0A++++++++++++++++a+arco%3AHistoricOrArtisticProperty+.%0D%0A%0D%0AFILTER%28REGEX%28%3Flabel%2C++%22da+cocktail%22%2C+%22i%22%29%29%0D%0A%7D&format=text%2Fhtml&timeout=0&signal_void=on).

``` SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT COUNT(DISTINCT ?clothing) AS ?n
WHERE { 
?clothing rdfs:label ?label ; 
                a arco:HistoricOrArtisticProperty .

FILTER(REGEX(?label,  "da cocktail", "i"))
}
``` 

We ran this QUERY to get the number of “da cocktail” dresses present in the class DemoEthnoAnthropologicalHeritage. The result is 0.

``` SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT COUNT(DISTINCT ?clothing) AS ?n
WHERE { 
?clothing rdfs:label ?label ; 
                a arco:DemoEthnoAnthropologicalHeritage .

FILTER(REGEX(?label,  "da cocktail", "i"))
}
```

We ran this QUERY to get the number of “da cocktail” dresses present in the class ArchaeologicalProperty. The result is 0.

``` SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT COUNT(DISTINCT ?clothing) AS ?n
WHERE { 
?clothing rdfs:label ?label ; 
                a arco:ArchaeologicalProperty .

FILTER(REGEX(?label,  "da cocktail", "i"))
}
```

We noticed that all “da cocktail” dresses are in the HistoricOrArtisticProperty class. To double check this and get the labels, we ran this QUERY. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fclothing+%3Flabel%0D%0AWHERE+%7B%0D%0A%3Fclothing+a+arco%3AHistoricOrArtisticProperty+%3B%0D%0Ardfs%3Alabel+%3Flabel+%0D%0AFILTER%28REGEX%28%3Flabel%2C+%22da+cocktail%22%2C+%22i%22%29%29%0D%0A%7D%0D%0ALIMIT+200%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on)

``` SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?clothing ?label
WHERE {
?clothing a arco:HistoricOrArtisticProperty ;
rdfs:label ?label 
FILTER(REGEX(?label, "da cocktail", "i"))
}
LIMIT 200
``` 

At this point, we search for the items with the **property value** “da cocktail”. The first one is associated with only one resource, i.e. “abito da cocktail”. The second one is associated with “bastoncino da cocktail”, which is not relevant for us. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%0D%0A%3FclothingProperty+%3Flabel+%0D%0AWHERE+%7B+%0D%0A%3FclothingProperty+rdfs%3Alabel+%3Flabel+%0D%0AFILTER%28%3Flabel+%3D+%22da+cocktail%22%29%0D%0A%7D%0D%0ALIMIT+100%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).

``` SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT 
?clothingProperty ?label 
WHERE { 
?clothingProperty rdfs:label ?label 
FILTER(?label = "da cocktail")
}
LIMIT 100
```  

We ran this QUERY to both double-check and retrieve the only dress associated with the property value “da cocktail” with an IRI. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0APREFIX+a-cd%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Fcontext-description%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fclothing+%3Flabel+%3FauthorName%0D%0AWHERE+%7B+%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel+%3B%0D%0A+++++++++++++++a-cd%3AhasAuthor+%3Fauthor+.%0D%0A%3Fauthor++rdfs%3Alabel+%3FauthorName%0D%0A++++++++++++++++%0D%0AFILTER%28REGEX%28%3Flabel%2C+%22cocktail%22%2C+%22i%22%29%29%0D%0AFILTER%28REGEX%28%3FauthorName%2C+%22sartoria+sorelle+sobrero%22%2C+%22i%22%29%29%0D%0A%7D%0D%0ALIMIT+20%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).

``` SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>

SELECT DISTINCT ?clothing ?label ?authorName
WHERE { 
?clothing rdfs:label ?label ;
               a-cd:hasAuthor ?author .
?author  rdfs:label ?authorName
                
FILTER(REGEX(?label, "cocktail", "i"))
FILTER(REGEX(?authorName, "sartoria sorelle sobrero", "i"))
}
LIMIT 20
```

This QUERY verifies that the property value "da cocktail" with property "arc:hasCulturalPropertyCategory" is only linked to the same Resource as before by "sartoria sorelle sobrero". This QUERY is useful because if we create new triples associated with the same property value, we can verify the link between them has been successfully created. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0APREFIX+c%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fresource%2FCulturalPropertyCategory%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%0D%0A%3FclothingProperty+%3Flabel+%0D%0AWHERE+%7B+%0D%0A%3FclothingProperty+rdfs%3Alabel+%3Flabel+%3B%0D%0Aarco%3AhasCulturalPropertyCategory+c%3Ada-cocktail+.%0D%0A%7D%0D%0ALIMIT+100%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).

``` SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX c: <https://w3id.org/arco/resource/CulturalPropertyCategory/>

SELECT DISTINCT 
?clothingProperty ?label 
WHERE { 
?clothingProperty rdfs:label ?label ;
arco:hasCulturalPropertyCategory c:da-cocktail .
}
LIMIT 100
```

These are the new triples we created: we chose a cocktail dress and a skirt that did not have the property value “da cocktail” associated with them and we link them to the property value “da cocktail” we have found.
We reused the predicate arco:hasCulturalPropertyCategory because it was already associated with the property value “da cocktail” in the “sartoria sorelle sobrero” dress.

**NEW TRIPLE 1**
*[https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201385625](https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201385625) → Subject (abito) 
*arco:hasCulturalPropertyCategory → Property / Predicate
*[https://w3id.org/arco/resource/CulturalPropertyCategory/da-cocktail](https://w3id.org/arco/resource/CulturalPropertyCategory/da-cocktail) → Property value / Object ("da cocktail")

**NEW TRIPLE 2**
[https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900750218](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900750218) → Subject (gonna)
arco:hasCulturalPropertyCategory → Property 
[https://w3id.org/arco/resource/CulturalPropertyCategory/da-cocktail](https://w3id.org/arco/resource/CulturalPropertyCategory/da-cocktail) → Property value ("da cocktail")






[back](./)
