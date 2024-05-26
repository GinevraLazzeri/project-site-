---
layout: default
title: Paillettes & Short Dress 
description: New Triple 3 & 4
---

## Methodology, Results & Analysis

In the second part of our project, we wanted to see if “paillettes” were a relevant element to work on regarding cocktail dresses. To do so, we retrieved the information from **Llama** (Meta Llama 2 7B > temperature 0.2) using the *Generated Knowledge Prompting technique*.
![Llama_Gen1](/immagini_markdown/Llama_Gen1.png)
![Llama_Gen2](/immagini_markdown/Llama_Gen2.png)
![Llama_Gen3](/immagini_markdown/Llama_Gen3.png)

We modified the prompt by removing the part "they are not necessarily the main characteristics" not to influence the LLM response:
![Llama_Gen4](/immagini_markdown/Llama_Gen4.png)

According to Llama, paillettes can be the main characteristic of cocktail dresses, so we wanted to link the “paillettes" property value to the dress. 
Moreover, looking at the same cocktail dress (subject of triple 1 [https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201385625](https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201385625)), we noticed that it is not only sequined (i.e. with paillettes), but also short, and the “corto” property value is also not linked to the dress. It only has the object “paillettes color bronzo”, which is very specific, and the reference to “abito corto” is completely missing (only the IRI of the dress is present). For this reason, our goal was to link the “paillettes” and “abito corto” property values to the dress.

We ran this QUERY to search the **property value “paillettes”**.  See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fclothing+%3Flabel+%0D%0AWHERE+%7B+%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel+%0D%0AFILTER%28%3Flabel+%3D+%22paillettes%22%29%0D%0A%7D%0D%0ALIMIT+20%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on) 

```SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?clothing ?label 
WHERE { 
?clothing rdfs:label ?label 
FILTER(?label = "paillettes")
}
LIMIT 20
```

Once we found the property value “paillettes”, we browsed the Denotative Description Ontology and found the “has Material or Technique” predicate, which looked suitable for our work as this property has numerous “paillettes” associated with it. 
We then ran this QUERY to make sure that the predicate “Has Material or Technique” is associated with the property value “paillettes”. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0APREFIX+a-dd%3A%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Fdenotative-description%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fc+%3Flabel+%0D%0AWHERE+%7B+%0D%0A%3Fclothing+a-dd%3AhasMaterialOrTechnique+%3Fc+.%0D%0A%3Fc+rdfs%3Alabel+%3Flabel+%0D%0AFILTER%28%3Flabel+%3D+%22paillettes%22%29%0D%0A%7D%0D%0ALIMIT+20%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on) 

```SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX a-dd:<https://w3id.org/arco/ontology/denotative-description/>

SELECT DISTINCT ?c ?label 
WHERE { 
?clothing a-dd:hasMaterialOrTechnique ?c .
?c rdfs:label ?label 
FILTER(?label = "paillettes")
}
LIMIT 20
```
As a result, we decided to use this predicate to create the third triple: we associated the same sequined dress we used before to the predicate "hasMaterialOrTechnique" and to the property value “paillettes”.

**NEW TRIPLE 3**
*   [https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201385625](https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201385625) →  Subject (Abito)
*   a-dd:hasMaterialOrTechnique → Property / Predicate
*   [https://w3id.org/arco/resource/TechnicalCharacteristic/paillettes](https://w3id.org/arco/resource/TechnicalCharacteristic/paillettes) → Property value / Object (Paillettes)

<img src="https://www.sigecweb.beniculturali.it/images/fullsize/ICCD1070166/ICCD15928135_FTMUBOL46.jpg" alt="abito da cocktail" width="250"/>






At this point, we passed on to the feature “corto”. 
We ran this QUERY to search all the items that had “corto” in their label. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fclothing+%3Flabel+%0D%0AWHERE+%7B+%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel+%0D%0AFILTER%28REGEX%28%3Flabel%2C+%22corto%22%2C+%22i%22%29%29%0D%0A%7D%0D%0ALIMIT+200%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on)

```SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?clothing ?label 
WHERE { 
?clothing rdfs:label ?label 
FILTER(REGEX(?label, "corto", "i"))
}
LIMIT 200
```

However, the results were too general and referred to items that were not relevant to our research. 
We then ran another QUERY to search specifically the **property value “corto”**. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fclothing+%3Flabel+%0D%0AWHERE+%7B+%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel+%0D%0AFILTER%28%3Flabel+%3D+%22corto%22%29%0D%0A%7D%0D%0ALIMIT+200%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on) 

```SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?clothing ?label 
WHERE { 
?clothing rdfs:label ?label 
FILTER(?label = "corto")
}
LIMIT 200
```

We explored the associated resources, and among them we found only one which is “abito corto”. For this reason, we decided to work on the property value “abito corto”, and not just “corto” because it is too general (it is present in every ontology) and therefore less effective for our research.

We browsed the Denotative Description Ontology to look for a suitable property and we chose “has Cultural Property Type”, that gave us various results, among which the IRI of the property value “abito corto”. Moreover, browsing the “abito corto” property value, we saw that the only property associated was “has Cultural Property Type”, solidifying our choice. 

This is the QUERY that should be used to find our “abito corto” if our triple was activated. 
See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+a-dd%3A%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Fdenotative-description%2F%3E%0D%0APREFIX+type%3A%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fresource%2FCulturalPropertyType%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+*%0D%0AWHERE+%7B+%0D%0A%3Fclothing+a-dd%3AhasCulturalPropertyType+type%3A2825951733ac23b16160cd06dc1a7e6f+%3B%0D%0A%09rdfs%3Alabel+%3FclothingLabel+.%0D%0A%7D%0D%0ALIMIT+200%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on) 

```SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX a-dd:<https://w3id.org/arco/ontology/denotative-description/>
PREFIX type:<https://w3id.org/arco/resource/CulturalPropertyType/>

SELECT DISTINCT *
WHERE { 
?clothing a-dd:hasCulturalPropertyType type:2825951733ac23b16160cd06dc1a7e6f ;
	rdfs:label ?clothingLabel .
}
LIMIT 200
```
This is the new triple we created in order to associate the property value “abito corto” to the sequined dress using the predicate "hasCulturalPropertyType":

**NEW TRIPLE 4**
*   [https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201385625](https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201385625) → Subject (abito)
*   a-dd:hasCulturalPropertyType → Property
*   [https://w3id.org/arco/resource/CulturalPropertyType/2825951733ac23b16160cd06dc1a7e6f](https://w3id.org/arco/resource/CulturalPropertyType/2825951733ac23b16160cd06dc1a7e6f) → Property value (abito corto)

<img src="https://www.sigecweb.beniculturali.it/images/fullsize/ICCD1070166/ICCD15928135_FTMUBOL46.jpg" alt="abito da cocktail" width="250"/>

Following the results we obtained in this section, we thought it could be useful to expand our research: the following query could be used to find the list of the dresses that have the “paillettes” property value and the dresses that have the “abito corto” property value. In this list we would find our dress twice, if the two triples were activated. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0APREFIX+a-dd%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Fdenotative-description%2F%3E%0D%0APREFIX+type%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fresource%2FCulturalPropertyType%2F%3E%0D%0APREFIX+characteristic%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fresource%2FTechnicalCharacteristic%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fclothing+%3Flabel%0D%0A%0D%0AWHERE+%7B%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel%0D%0A%0D%0A%7B%3Fclothing+a-dd%3AhasCulturalPropertyType+type%3A2825951733ac23b16160cd06dc1a7e6f+%7D%0D%0AUNION%0D%0A%7B%3Fclothing+a-dd%3AhasMaterialOrTechnique+characteristic%3Apaillettes+%7D%0D%0A%7D%0D%0A%0D%0ALIMIT+200%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).

```SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX a-dd: <https://w3id.org/arco/ontology/denotative-description/>
PREFIX type: <https://w3id.org/arco/resource/CulturalPropertyType/>
PREFIX characteristic: <https://w3id.org/arco/resource/TechnicalCharacteristic/>

SELECT DISTINCT ?clothing ?label

WHERE {
?clothing rdfs:label ?label

{?clothing a-dd:hasCulturalPropertyType type:2825951733ac23b16160cd06dc1a7e6f }
UNION
{?clothing a-dd:hasMaterialOrTechnique characteristic:paillettes }
}

LIMIT 200
```





[back](./)
