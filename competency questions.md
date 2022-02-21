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





1. What is the name, last name, played instrument and role of all visually-impaired female musicians living in London who are amateur and play rock?
```
SELECT  ?firstName ?lastName ?instrument ?role  
	WHERE {
    	?musician 						a/rdfs:subClassOf*  					musico:HumanMusician ;
    									foaf:based_near							ex:London ;
    	 								foaf:firstName							?firstName ;
    	 								foaf:lastName							?lastName ;
    									foaf:gender								"female" ;
    									musico:professional_level 				"Amateur" ;
										musico:has_disability                   true ;
										musico:disability_type                  "visually-impaired" ;
    									musico:plays_genre						ex:Rock ;
    									musico:plays_instrument					?instrument ;
    									rdf:type								?role .
}
```

2.	Which musical genres professional musicians play more frequently when they feel happy?
```
SELECT ?Genre (COUNT(?Genre) as ?num_genres_happy)
	WHERE {
		?Musician						a/rdfs:subClassOf*						musico:HumanMusician ;
										musico:professional_level				"Professional" ;
    	 								foaf:firstName							?firstName ;
    	 								foaf:lastName							?lastName ;
										musico:in_participation					?MusicianParticipation .
		?MusicianParticipation 			musico:involved_event 					?MusicalSession ;
										musico:felt_emotion 					mfoem:happiness ;
										musico:played_musical_work				?MusicalWork .
		?MusicalWork					mo:genre								?Genre .			
}
GROUP BY ?Genre
ORDER BY DESC(?num_genres_happy)
LIMIT 1
```

3.	In how many sessions the musician Cristina Ladynekt has performed with her smart microphone during a tour in 2021?
```
SELECT  (COUNT(?MusicalSession) as ?num_sessions)
	WHERE {
		ex:CristinaLadynekt				musico:in_participation					?MusicianParticipation .	
  		?MusicianParticipation			musico:played_instrument				ex:karaokeSmartMicrophone ;
										musico:involved_event					?MusicalSession .
  		?MusicalSession					schema:isPartOf							?Performance .
		?Performance					schema:isPartOf							?Tour .
  		?Tour							event:time								[ 
                                                                                a tl:Instant ;
                                                                                tl:at "2021"^^xsd:gYear ;
                                                                                ] .
}
```


4.	What are the emotions of beginner smart guitar players while learning the study number 5 of Fernando Sor's "20 studies for guitar" during lessons with Prof. Cristina Ladynekt at KMH Royal College of Music in Stockholm ?
```
SELECT ?emotion
	WHERE {
		?Learner						rdf:type								musico:HumanMusicLearner ;
										musico:expertise_level					"Beginner" ;
										musico:plays_instrument                 ex:SmartGuitar ;
										musico:in_participation					?LearnerParticipation .
	    ?LearnerParticipation			musico:involved_event					?LessonSession ;
										musico:felt_emotion 					?emotion .
		?LessonSession					rdf:type 								musico:MusicalSession ;
										schema:isPartOf 						?Lesson .
		?Lesson							rdf:type 								musico:Lesson ;
										schema:location                         ex:KMHRoyalCollegeOfMusic .
		?Teacher						rdf:type								musico:HumanMusicTeacher ;
										foaf:firstName							"Cristina" ;
										foaf:lastName							"Ladinekt" ;
										musico:in_participation					?TeacherParticipation .
		?TeacherParticipation			musico:involved_event					?LessonSession ;
										musico:taught_musical_work              ex:study5Sor20Studies .
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
