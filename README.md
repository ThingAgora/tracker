
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

References
----------

  * http://lov.okfn.org/dataset/lov/vocabs/geo
