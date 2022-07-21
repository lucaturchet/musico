# Competencey questions for the Musician's Context Ontology

The given SPARQL are _examples_ that may be reinterpreted and reused for applications. You can run these queries on the example_dataset.ttl, which represents knowledge related to the context of different musicians using smart musical instruments.

Prefixes:

```
PREFIX ex: <http://www.example.audio/> 
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
PREFIX tl: <http://purl.org/NET/c4dm/timeline.owl#> 
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX gn: <http://www.geonames.org/ontology#>
PREFIX schema: <https://schema.org/docs/schemaorg.owl>
```


1. What is the name, last name, played instrument and role of all visually-impaired female musicians living in London who are amateur and play rock?
```
SELECT  ?firstName ?lastName ?instrument ?role  
	WHERE {
    	?Musician 						a/rdfs:subClassOf*  					musico:HumanMusician ;
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

4.	What are the emotions of beginner smart guitar players while learning the study number 5 of Fernando Sor's "20 studies for guitar" during lessons with Prof. Cristina Ladynekt at KMH Royal College of Music in Stockholm?
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
		?Lesson							rdf:type 								musico:MusicLesson ;
										schema:location                         ex:KMHRoyalCollegeOfMusic .
		?Teacher						rdf:type								musico:HumanMusicTeacher ;
										foaf:firstName							"Cristina" ;
										foaf:lastName							"Ladinekt" ;
										musico:in_participation					?TeacherParticipation .
		?TeacherParticipation			musico:involved_event					?LessonSession ;
										musico:taught_musical_work              ex:study5Sor20Studies .
}
```

5.	Which are the three virtual teachers playing piano with which smart guitar players interact most frequently while self-learning at home?
```
SELECT ?virtualMusician (COUNT(?virtualMusician) as ?num_virtual_musicians)
	WHERE {
		?Musician 						musico:plays_instrument					?instrument ;
										musico:in_participation					?SelfLearnerParticipation .
		?SelfLearnerParticipation		musico:involved_event					?SelfLearningSession .
		?SelfLearningSession			rdf:type 								musico:MusicalSession ;
										schema:isPartOf 						?SelfLearningActivity .
		?SelfLearningActivity			rdf:type 								musico:SelfLearning ;
										schema:location							?home .
		?home							rdf:type								musico:Home .
		?instrument						rdf:type 								smi:SmartGuitar ;
										smi:smi_application						?application .
		?application					rdf:type								smi:SMIApplication ;
										musico:has_virtual_musician				?virtualMusician .
		?virtualMusician				rdf:type								musico:VirtualMusicTeacher ; 	
										musico:plays_instrument					?virtualInstrument .
		?virtualInstrument				rdf:type								smi:Piano . 	
}

GROUP BY ?virtualMusician
ORDER BY DESC(?num_virtual_musicians)
LIMIT 3

```

6.	When is the last time the musician Sharan Giptas has rehearsed with the other members of his band "The Unicorns and Camels"?
```
SELECT ?DateTime
	WHERE {
		ex:SharanGiptas		     		musico:in_participation					?MusicianParticipation .
		?MusicianParticipation 			musico:involved_event					?MusicalSession .
  		?MusicalSession					schema:isPartOf							?Rehearsal .
		?Rehearsal						rdf:type								musico:Rehearsal ;
										musico:has_group                        ex:TheUnicornsAndCamels ;
										event:time								?Time .
		?Time							rdf:type								tl:Interval ;
										tl:at									?DateTime .
}
ORDER BY DESC(?Time)
LIMIT 1
```

7.	How long poly-instrumentalist musicians from Tokyo play on average each day for recreational music making?
```
SELECT ?Musician (AVG(?Duration) AS ?avg)
	WHERE {
		?Musician 						musico:multi-instrumentalism_level		"multi-instrumentalist" ;
										foaf:based_near							ex:Tokyo ;
										musico:in_participation					?Participation .
	    ?Participation					rdf:type                            	musico:MusicianParticipation ;
										musico:involved_event                   ?Session .
	    ?Session 						rdf:type                                musico:MusicalSession ;
										schema:isPartOf							?RecreatinalMusicMaking .
		?RecreatinalMusicMaking			rdf:type								musico:RecreationalMusicMaking ;
										event:time 								?Time .
		?Time							rdf:type								tl:Interval ;
										tl:duration								?Duration .
}
GROUP BY ?Musician
```

8.	How many synchronous, displaced performances have the musicians from London performed in 2020?
```
SELECT (COUNT(DISTINCT ?Performance) as ?count)
	WHERE {
		?Musician 						a/rdfs:subClassOf*  					musico:HumanMusician ;
    									foaf:based_near							ex:London ;
										musico:in_participation					?MusicianParticipation .
		?MusicianParticipation 			musico:involved_event					?MusicalSession ;
										musico:is_situated                      false ;
										musico:is_synchronous					true .
		?MusicalSession					schema:isPartOf							?Performance .
		?Performance					rdf:type 								musico:Performance ;
										event : time 							[
																				a tl: Instant ;
																				tl: at "2020"^^xsd:gYear ;
																				] .
}
```


