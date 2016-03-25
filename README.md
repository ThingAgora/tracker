
Tracker-IoT
===========

This is a fork of [Tracker](http://wiki.gnome.org/Projects/Tracker) for the Semantic Web of Things.
It includes simplified versions of the [Semantic Sensors Network](https://www.w3.org/2005/Incubator/ssn/ssnx/ssn) and related ontologies.

Most of the added ontologies are extracted from the n3 versions available on the [Linked Open Vocabularies](http://lov.okfn.org/dataset/lov/) site.


Added ontologies
----------------

### [geo](http://lov.okfn.org/dataset/lov/vocabs/geo)

De facto standard ontology for representing latitude, longitude and altitude information.
Relations are added to make the `slo` ontology derive from `geo` for compatibility.

#### Source files

	95-geo.ontology

#### Sample query
	tracker-sparql -q "SELECT ?url ?long ?lat WHERE {
		?x nie:url ?url; geo:location ?loc. ?loc geo:long ?long; geo:lat ?lat
	}"

### [owl](http://lov.okfn.org/dataset/lov/vocabs/owl)

The OWL 2 Web Ontology Language, referenced by most of the Linked Open Vocabularies ontologies.

#### Source files

	50-owl.ontology

### [dul](http://lov.okfn.org/dataset/lov/vocabs/dul)

The DOLCE+DnS Ultralite ontology. Foundational ontology for modeling either physical or social contexts.
Parent for most classes and properties of the ssn ontology.

#### Source files

	52-dul-classes.ontology
	53-dul-properties.ontology

### [ssn](http://lov.okfn.org/dataset/lov/vocabs/ssn)

The W3C [Semantic Sensor Networks Incubator Group](https://www.w3.org/2005/Incubator/ssn/wiki/Incubator_Report#The_Semantic_Sensor_Network_Ontology) Semantic Sensors Network ontology is based around concepts of systems, processes, and observations. It supports the description of the physical and processing structure of sensors.

#### Source files

	54-ssn-classes.ontology
	55-ssn-properties.ontology

#### Sample model

Create a telemeter model in a file named `ultrasonicWifiTelemeter.n3`

	<uswt00hsr04> a ssn:SensingDevice;
		rdfs:label "uswt HCSR04 ultrasonic telemeter module".

	<uswt00esp01> a ssn:Device;
		rdfs:label "uswt ESP-01 WiFi module".

	<uswt00atmega328> a ssn:Device;
		rdfs:label "uswt Arduino Pro Mini controller".

	<uswt00> a ssn:SensingDevice;
		rdfs:label "ultrasonic Wifi Telemeter";
		ssn:hasSubSystem <uswt00hsr04>;
		ssn:hasSubSystem <uswt00esp01>;
		ssn:hasSubSystem <uswt00atmega328>.

Import the file with tracker.

	tracker-import ultrasonicWifiTelemeter.n3

#### Sample query

	tracker-sparql -q "SELECT ?x ?lx ?y ?ly {
		?x a dul:PhysicalObject;
			dul:hasPart ?y;
			rdfs:label ?lx.
		?y rdfs:label ?ly
	}"

### [muo](http://idi.fundacionctic.org/muo/muo-vocab.html) and [ucum](http://idi.fundacionctic.org/muo/ucum-instances.html)

These ontologies referenced in the [W3C Semantic Sensors Network examples](https://www.w3.org/2005/Incubator/ssn/wiki/Incubator_Report#Additional_examples) describe measurement units.

#### Source files

	56-muo.ontology
	57-ucum.ontology
	58-ucum-instances.ontology

#### Sample model

File `ultrasonicWifiTelemeterEnvironment.n3` describes the feature measured by the telemeter, i.e. the length of the space in front of it.

	<uswt00frontspace> a ssn:FeatureOfInterest;
		ssn:hasProperty <http://purl.oclc.org/NET/muo/ucum/physical-quality/length>.
	
	<uswt00> ssn:observes <http://purl.oclc.org/NET/muo/ucum/physical-quality/length>.

File `ultrasonicWifiTelemeterObservation.n3` describes one measure instance, in meters.

	<uswt00val01> a uomvocab:QualityValue;
		uomvocab:measuredIn <http://purl.oclc.org/NET/muo/ucum/unit/length/meter>;
		uomvocab:numericalValue 10.
	
	<uswt00out01> a ssn:SensorOutput;
		ssn:hasValue <uswt00val01>.
	
	<uswt00observ01> a ssn:Observation;
		ssn:observedBy <uswt00>;
		ssn:observedProperty <http://purl.oclc.org/NET/muo/ucum/physical-quality/length>;
		ssn:featureOfInterest <uswt00frontspace>;
		ssn:observationResult <uswt00out01>.

Import the files.

	tracker-import ultrasonicWifiTelemeterEnvironment.n3 ultrasonicWifiTelemeterObservation.n3

#### Sample query

	tracker-sparql -q "SELECT ?sensor ?property ?feature ?value ?unit {
		?s rdf:type ssn:Observation ;
			ssn:observedBy ?sensor ;
			ssn:observedProperty ?property ;
			ssn:featureOfInterest ?feature ;
			ssn:observationResult ?so .
		?so ssn:hasValue ?ov .
		?ov dul:hasRegionDataValue ?value ;
			uomvocab:measuredIn ?unit . 
	}"


References
----------

  * http://lov.okfn.org/dataset/lov/vocabs/geo
  * http://lov.okfn.org/dataset/lov/vocabs/owl
  * http://lov.okfn.org/dataset/lov/vocabs/dul
  * http://lov.okfn.org/dataset/lov/vocabs/ssn
  * http://idi.fundacionctic.org/muo/muo-vocab.html
  * http://idi.fundacionctic.org/muo/ucum-instances.html
