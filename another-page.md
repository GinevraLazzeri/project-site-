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

[back](./)
