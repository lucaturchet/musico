# Competencey questions for the Musician's Context Ontology

The given SPARQL are _examples_ that may be reinterpreted and reused for applications. You can run these queries on the example_dataset.ttl, which represents knowledge related to the context of different musicians using smart musical instruments.

Prefixes:

```
PREFIX ex: <http://www.example.audio/ex> 
PREFIX musico: <http://purl.org/ontology/musico#> 
PREFIX smi: <http://purl.org/ontology/iomust/smi#> 
PREFIX iot: <http://purl.org/ontology/iomust/internet_of_things/> 
PREFIX iomust: <http://purl.org/ontology/iomust/internet_of_things/iomust> 
PREFIX mfoem: <http://purl.obolibrary.org/obo/MFOEM.owl>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX studio: <http://purl.org/ontology/studio/> 
PREFIX mo: <http://purl.org/ontology/mo/> 
PREFIX foaf: <http://xmlns.com/foaf/0.1/> 
PREFIX event: <http://purl.org/NET/c4dm/event.owl#>
PREFIX time: <https://www.w3.org/2006/time>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX gn: <http://www.geonames.org/ontology#>
PREFIX schema: <https://schema.org/docs/schemaorg.owl>
```





0. Tutti i musicisti di Verona e il loro ruolo

SELECT ?name
WHERE {
  ?name a/rdfs:subClassOf*  musico:human_musician .
}



1.	Which musical genres amateur musicians play more frequently when they feel happy?
```
SELECT ?genre (COUNT(?genre) as ?num_genres_happy)
	WHERE {
		?hm					rdf:type					musico:human_musician .
		?hm					musico:professional_level	"Amateur" .
		?hm					musico:plays_genre			?genre .
		?hm					musico:feels_emotion		?ep .
		?ep					rdf:type					mfoem:MFOEM_000042 .
		?ep					rdfs:label					"happiness" .
		?genre				rdf:type					mo:Genre . 	
	}

ORDER BY DESC(?num_genres_happy)
LIMIT 1
```

2.	What is the name and last name of female pop pianists who live in Singapore and are visually impaired? 
```
SELECT ?a
	WHERE {
	
}
```

3.	What is the average mood of beginner pianists while learning the study number 22 of Bach's Well Tempered Clavier during lessons with Prof. Liang Brunzor?
```
SELECT ?a
	WHERE {
	
}
```

4.	How many times the singer Cristina Ladynekt has performed with her smart microphone in the Midnight jazz club of New Orleans in 2021?
```
SELECT ?a
	WHERE {
	
}
```

5.	How long poly-instrumentalist musicians from Brazil play on average each day for recreational music making?
```
SELECT ?a
	WHERE {
	
}
```

6.	What are the virtual musicians with which smart electric guitar players interact most frequently while self-learning?
```
SELECT ?a
	WHERE {
	
}
```

7.	When the smart electric bass player Arturo Sakamoto has rehearsed with the other members of his band The Unicorns?
```
SELECT ?a
	WHERE {
	
}
```

8.	How many times the smart synthesizers produced by Mooggy have been used in remote live concerts?
```
SELECT ?a
	WHERE {
	
}
```

9.	What software tools intermediate composers use for supporting their compositional practice?
```
SELECT ?a
	WHERE {
	
}
```

10.	In which venues of the city of Madrid amateur rock musicians perform more frequently?
```
SELECT ?a
	WHERE {
	
}
```
