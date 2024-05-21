---
layout: default
title: Drapery & Bow in a Dress
description: New Triple 5, 6 & 7
---

## Methodology

In the last part of our project we decided to focus on the decorative apparatus of a new dress. 
Browsing the Denotative Description ontology, we found the properties “Technical Characteristic” and “Has Iconographic or Decorative Apparatus”. In order to choose the most suitable one, we asked **ChatGPT** and **Gemini** to tell us the differences between the two using the *Zero-shot prompting technique*. 

Chat GPT
![ChatGPT_Zero-Shot1](/immagini_markdown/ChatGPT_Zero-Shot1.png)
![ChatGPT_Zero-Shot2](/immagini_markdown/ChatGPT_Zero-Shot2.png)

Gemini

![Gemini_Zero-Shot1](/immagini_markdown/Gemini_Zero-Shot1.png)
![Gemini_Zero-Shot2](/immagini_markdown/Gemini_Zero-Shot2.png)

Based on the LLMs results, we opted for Decorative Apparatus Type. 
We then built a QUERY identifying all the cocktail dresses having an IRI linked to an Iconographic or Decorative Apparatus and asked ChatGPT to build a QUERY, based on ours, to find all the cocktail dresses that DO NOT have an IRI linked to an Iconographic or Decorative Apparatus.

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

In the QUERY provided by ChatGPT, the BOUND tag is preceded by “!”, that in the FILTER tag means “NO”. This QUERY therefore eliminates all the results bound to that property value.

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

Browsing the list of dresses that do not have an IRI associated with an Iconographic or Decorative Apparatus, we found a dress that had a bow and a drapery in the description but no “fiocco” nor “drappeggio” IRI properties associated with it. 

Following the research we carried out in this section, we created a new triple which links the dress to the Decorative Apparatus property value. The literal we used in the object was invented by us (on the basis of the Decorative Apparatus page of other dresses) as it does not actually exist.

**NEW TRIPLE 5**
*   [https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900750088](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900750088) → Subject (abito)
*   a-dd:hasIconographicOrDecorativeApparatus → Predicate
*   [https://w3id.org/arco/resource/IconographicOrDecorativeApparatus/0900750088](https://w3id.org/arco/resource/IconographicOrDecorativeApparatus/0900750088)  → Object (LITERAL of the Decorative Apparatus of this dress: **Apparato decorativo 1 del bene culturale 0900750088** ( → code of the dress ).

<img src="http://www.sigecweb.beniculturali.it/images/fullsize/ICCD1020284/ICCD11168479_SSPSAEPM%20FI%2024672UC.jpg" style="float:left;margin: 10 px" alt="abito da cocktail" width="250"/>
<img src="http://www.sigecweb.beniculturali.it/images/fullsize/ICCD1020284/ICCD11168479_SSPSAEPM%20FI%2024672UC.jpg" alt="abito da cocktail" width="250"/>











[back](./)
