---
layout: default
title: Drapery & Bow in a Dress
description: New Triple 5, 6 & 7
---

## Methodology, Results & Analysis

In the last part of our project we decided to focus on the decorative apparatus of a new dress. 
Browsing the Denotative Description ontology, we found the classes “Technical Characteristic” and “Decorative Apparatus" (having super-class "Iconographic or Decorative Apparatus”). In order to choose the most suitable one, we asked **ChatGPT** and **Gemini** to tell us the differences between the two using the *Zero-shot prompting technique*. 

Chat GPT
![ChatGPT_Zero-Shot1](/immagini_markdown/ChatGPT_Zero-Shot1.png)
![ChatGPT_Zero-Shot2](/immagini_markdown/ChatGPT_Zero-Shot2.png)

Gemini

![Gemini_Zero-Shot1](/immagini_markdown/Gemini_Zero-Shot1.png)
![Gemini_Zero-Shot2](/immagini_markdown/Gemini_Zero-Shot2.png)

Based on the LLMs results, we opted for the "Iconographic or Decorative Apparatus” class. 
We then built a QUERY identifying all the cocktail dresses having an IRI linked to an Iconographic or Decorative Apparatus object and asked **ChatGPT**, with a *Zero-shot prompting technique*, to build a QUERY, based on ours, to find all the cocktail dresses that DO NOT have an IRI linked to an Iconographic or Decorative Apparatus.

![ChatGPT_decorative1.png](/immagini_markdown/ChatGPT_decorative1.png)
![ChatGPT_decorative2.png](/immagini_markdown/ChatGPT_decorative2.png)
![ChatGPT_decorative3.png](/immagini_markdown/ChatGPT_decorative3.png)

```SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX a-dd: <https://w3id.org/arco/ontology/denotative-description/>

SELECT DISTINCT *
WHERE { 
  ?clothing rdfs:label ?label ; 
            a arco:HistoricOrArtisticProperty .
  FILTER(REGEX(?label,  "da cocktail", "i"))
  
  OPTIONAL { ?clothing a-dd:hasIconographicOrDecorativeApparatus ?dec . }
  FILTER(!BOUND(?dec))
}
```

See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0APREFIX+a-dd%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Fdenotative-description%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+*%0D%0AWHERE+%7B+%0D%0A++%3Fclothing+rdfs%3Alabel+%3Flabel+%3B+%0D%0A++++++++++++a+arco%3AHistoricOrArtisticProperty+.%0D%0A++FILTER%28REGEX%28%3Flabel%2C++%22da+cocktail%22%2C+%22i%22%29%29%0D%0A++%0D%0A++OPTIONAL+%7B+%3Fclothing+a-dd%3AhasIconographicOrDecorativeApparatus+%3Fdec+.+%7D%0D%0A++FILTER%28%21BOUND%28%3Fdec%29%29%0D%0A%7D%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).

In the QUERY provided by ChatGPT, the BOUND keyword is preceded by “**!**”, that in the FILTER keyword means “NO”. This QUERY therefore eliminates all the results bound to that property value.

**BOUND**: A variable in a SPARQL query is considered "bound" when it has a value assigned to it as a result of the query evaluation. If a variable is bound, it means that it has been successfully matched with a value from the data being queried (Source: Quora [https://www.quora.com/What-do-the-terms-of-binding-bound-mean-in-SPARQLand Wikipedia](https://www.quora.com/What-do-the-terms-of-binding-bound-mean-in-SPARQL) and Wikipedia [https://en.wikibooks.org/wiki/SPARQL/Expressions_and_Functions#:~:text=Try%20it!-,BOUND,%2CthenExpression%2CelseExpression](https://en.wikibooks.org/wiki/SPARQL/Expressions_and_Functions#:~:text=Try%20it!-,BOUND,%2CthenExpression%2CelseExpression)).

To be critical towards ChatGPT’s answer, we subtracted the number of cocktail dresses that have the property value “Has Iconographic or Decorative Apparatus” (i.e. 24) to the number of total cocktail dresses in the Historic or Artistic Property class (i.e. 52). The result is 28.

```SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT COUNT(DISTINCT ?clothing) AS ?n
WHERE { 
?clothing rdfs:label ?label ; 
                a arco:HistoricOrArtisticProperty ;
                a-dd:hasIconographicOrDecorativeApparatus ?dec .

FILTER(REGEX(?label,  "da cocktail", "i"))
}
```
See results (24) [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+COUNT%28DISTINCT+%3Fclothing%29+AS+%3Fn%0D%0AWHERE+%7B+%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel+%3B+%0D%0A++++++++++++++++a+arco%3AHistoricOrArtisticProperty+%3B%0D%0A++++++++++++++++a-dd%3AhasIconographicOrDecorativeApparatus+%3Fdec+.%0D%0A%0D%0AFILTER%28REGEX%28%3Flabel%2C++%22da+cocktail%22%2C+%22i%22%29%29%0D%0A%7D%0D%0A%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).

```SPARQL
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX a-dd: <https://w3id.org/arco/ontology/denotative-description/>

SELECT COUNT(DISTINCT ?clothing) AS ?n
WHERE { 
  ?clothing rdfs:label ?label ; 
            a arco:HistoricOrArtisticProperty .
  FILTER(REGEX(?label,  "da cocktail", "i"))
  
  OPTIONAL { ?clothing a-dd:hasIconographicOrDecorativeApparatus ?dec . }
  FILTER(!BOUND(?dec))
}
```
See results (28) [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0APREFIX+a-dd%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Fdenotative-description%2F%3E%0D%0A%0D%0ASELECT+COUNT%28DISTINCT+%3Fclothing%29+AS+%3Fn%0D%0AWHERE+%7B+%0D%0A++%3Fclothing+rdfs%3Alabel+%3Flabel+%3B+%0D%0A++++++++++++a+arco%3AHistoricOrArtisticProperty+.%0D%0A++FILTER%28REGEX%28%3Flabel%2C++%22da+cocktail%22%2C+%22i%22%29%29%0D%0A++%0D%0A++OPTIONAL+%7B+%3Fclothing+a-dd%3AhasIconographicOrDecorativeApparatus+%3Fdec+.+%7D%0D%0A++FILTER%28%21BOUND%28%3Fdec%29%29%0D%0A%7D%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on) 

28 + 24 = 52 > Chat GPT's QUERY is correct!

Browsing the list of dresses that do not have an IRI associated with an Iconographic or Decorative Apparatus, we found a dress that had a bow and a drapery in the description but no “fiocco” nor “drappeggio” IRI property values associated with it. 

Following the research we carried out in this section, we created a new triple which links the dress to the Decorative Apparatus property value. The literal we used in the object was invented by us (on the basis of the Decorative Apparatus page of other dresses) as it does not actually exist.

**NEW TRIPLE 5**
*   [https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900750088](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900750088) → Subject (abito)
*   a-dd:hasIconographicOrDecorativeApparatus → Predicate
*   [https://w3id.org/arco/resource/IconographicOrDecorativeApparatus/0900750088](https://w3id.org/arco/resource/IconographicOrDecorativeApparatus/0900750088)  → Object (LITERAL of the Decorative Apparatus of this dress: **Apparato decorativo 1 del bene culturale 0900750088** ( → code of the dress ).

<img src="http://www.sigecweb.beniculturali.it/images/fullsize/ICCD1020284/ICCD11168479_SSPSAEPM%20FI%2024672UC.jpg" alt="abito da cocktail" width="250"/> <img src="http://www.sigecweb.beniculturali.it/images/fullsize/ICCD1020284/ICCD11168480_SSPSAEPM%20FI%2024669UC.jpg" alt="abito da cocktail" width="250"/>

We retrieved information from **Llama 3 8B** using the *Few-shot prompting technique* to find the **property value “drappeggio”**, giving as examples the QUERIES to find the property values “da cocktail” and “paillettes”.

![Llama_Few-Shot](/immagini_markdown/Llama_Few-Shot.png)

This verified that the QUERY provided by Llama works by running it. The results are three items with the property value “drappeggio”. We chose randomly the first one since none of them has any corresponding resource. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Fclothing+%3Flabel+%0D%0AWHERE+%7B+%0D%0A%3Fclothing+rdfs%3Alabel+%3Flabel+%0D%0AFILTER%28%3Flabel+%3D+%22drappeggio%22%29%0D%0A%7D%0D%0ALIMIT+20%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).

```SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?clothing ?label 
WHERE { 
?clothing rdfs:label ?label 
FILTER(?label = "drappeggio")
}
LIMIT 20
```

We created our triple linking the previous Decorative Apparatus to the property value “drappeggio” using the predicate hasType. Indeed, looking at the results of other Decorative Apparatus, we noticed that “core:hasType” was the most commonly used predicate and also because it avoided repetition which would occur if we used the predicate “Decorative Apparatus Type” instead. For this reason, we opted for “core:hasType”.

**NEW TRIPLE 6** 
*   [https://w3id.org/arco/resource/IconographicOrDecorativeApparatus/0900750088](https://w3id.org/arco/resource/IconographicOrDecorativeApparatus/0900750088) (See Literal above) → Subject
*   core:hasType → Predicate
*   [https://w3id.org/arco/resource/DecorativeApparatusType/1900115134drappeggio](https://w3id.org/arco/resource/DecorativeApparatusType/1900115134drappeggio)  → Object


We created this next triple following the same reasoning: we linked the same Decorative Apparatus to the property value “fiocco” using the same predicate (hasType). Actually, we corrected the already existing property value “ficco”.

**NEW TRIPLE 7**
*   [https://w3id.org/arco/resource/IconographicOrDecorativeApparatus/0900750088](https://w3id.org/arco/resource/IconographicOrDecorativeApparatus/0900750088)(See Literal above) → Subject
*   core:hasType → Predicate
*   [https://w3id.org/arco/resource/DecorativeApparatusType/fiocco](https://w3id.org/arco/resource/DecorativeApparatusType/fiocco) → Object

The following QUERY is a double-check, it would work if our triple was activated. It now works if we change the word “fiocco” to “ficco” since the latter already exists in our sequined cocktail dress. See results [at this link](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+rdf%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F1999%2F02%2F22-rdf-syntax-ns%23%3E%0D%0APREFIX+rdfs%3A+%3Chttp%3A%2F%2Fwww.w3.org%2F2000%2F01%2Frdf-schema%23%3E%0D%0APREFIX+core%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Fcore%2F%3E%0D%0APREFIX+a-dd%3A%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Fdenotative-description%2F%3E%0D%0APREFIX+dec%3A%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fresource%2FDecorativeApparatusType%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+*%0D%0AWHERE+%7B+%0D%0A%3Fclothing+a-dd%3AhasIconographicOrDecorativeApparatus+%3Fdecorativeapparatus+.%0D%0A%3Fdecorativeapparatus+core%3AhasType+dec%3Aficco%3B%0D%0A%09++++++++++++++++++++++++rdfs%3Alabel+%3FdecLabel+.%0D%0A%7D%0D%0ALIMIT+200%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on) 


```SPARQL
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX core: <https://w3id.org/arco/ontology/core/>
PREFIX a-dd:<https://w3id.org/arco/ontology/denotative-description/>
PREFIX dec:<https://w3id.org/arco/resource/DecorativeApparatusType/>

SELECT DISTINCT *
WHERE { 
?clothing a-dd:hasIconographicOrDecorativeApparatus ?decorativeapparatus .
?decorativeapparatus core:hasType dec:fiocco;
	                        rdfs:label ?decLabel .
}
LIMIT 200
```

At this point, we created the complete Decorative Apparatus page of our dress based on the answers we got from **ChatGPT** and **Mixtral** using the *Few-shot technique* providing two examples. 

The two examples of dresses containing the Decorative Apparatus page: 
1. [abito, da cocktail, femminile - ambito italiano (terzo quarto sec. XX)](https://dati.beniculturali.it/lodview-arco/resource/HistoricOrArtisticProperty/0900750229.html)
   
![App_dec_rosso](/immagini_markdown/App_dec_rosso.png)

2. [sopravveste, da cocktail, femminile di Veneziani Jolanda (terzo quarto sec. XX)](https://dati.beniculturali.it/lodview-arco/resource/HistoricOrArtisticProperty/0900750231.html)
   
![App_dec_bianco](/immagini_markdown/App_dec_bianco.png)

Prompting Chat GPT

![ChatGPT_few-shot1](/immagini_markdown/ChatGPT_few-shot1.png)
![ChatGPT_few-shot2](/immagini_markdown/ChatGPT_few-shot2.png)
![ChatGPT_few-shot3](/immagini_markdown/ChatGPT_few-shot3copia.png)
![ChatGPT_few-shot4](/immagini_markdown/ChatGPT_few-shot4.png)

ChatGPT’s answer was satisfying regarding the Label, Description and Type sections, whereas the other information it gave us was not relevant. The section “core:hasType” was not provided as it is not associated with the Decorative Apparatus of our dress, so we created it from scratch.


We asked Mixtral the same question, but the results were not satisfying, as seen in the images below.

![Mixtral_few-shot1](/immagini_markdown/Mixtral_few-shot1.png)
![Mixtral_few-shot2](/immagini_markdown/Mixtral_few-shot2.png)


This is the final corrected version of the Decorative Apparatus page of our dress :

rdfs:**label** Apparato decorativo 1 del bene culturale 0900750088

core:**description** Abito con gonna leggermente svasata, drappeggio fermato sul centro dietro da fiocco decorativo

l0:**name** Apparato decorativo 1 del bene culturale 0900750088

rdf:**type** a-dd:IconographicOrDecorativeApparatus        → Apparato iconografico e decorativo

core:**hasType** [https://w3id.org/arco/resource/DecorativeApparatusType/fiocco](https://w3id.org/arco/resource/DecorativeApparatusType/fiocco)
                 [https://w3id.org/arco/resource/DecorativeApparatusType/1900115134drappeggio](https://w3id.org/arco/resource/DecorativeApparatusType/1900115134drappeggio)



[back](./)
