# UnitUnity
experiments in unifying Unit Of Measure resources across QUDT, UCUM and UN/CEFACT Rec20

rec20.jsonld is a snapshot on 1 Feb 2022 of https://service.unece.org/trade/uncefact/vocabulary/rec20.jsonld
rec20.ttl was obtained by converting rec20.jsonld to N-Quads using https://json-ld.org/playground/

QUDTunit.ttl is a snapshot on 1 Feb 2022 of http://qudt.org/2.1/vocab/unit

rec20Test2.rq is a SPARQL CONSTRUCT query ( intended to operate on rec20.ttl ) to construct mappings from UN/CEFACT rec20 unit resources to the corresponding UN/CEFACT Rec20 code string of 2-3 characters, resulting in triples such as `rec20:kelvin  qudt:uneceCommonCode  "KEL" .`

rec20ConstructedTriples.ttl is the resulting set of constructed triples (using Jena ARQ as follows:  
`arq --data rec20.ttl --query rec20Test2.rq > rec20ConstructedTriples.ttl`

qudtTest2.rq is a SPARQL CONSTRUCT query (intended to operate on QUDTunit.ttl ) to construct mappings from QUDT unit resources to the corresponding UN/CEFACT Rec20 code string of 2-3 characters, as well as to the UCUM code string (expressed currently in QUDT as a typed literal, rather than a Web URI), resulting in triples such as: 

`unit:K  qudt:ucumCode         "K"^^qudt:UCUMcs ;
        qudt:uneceCommonCode  "KEL" .`

qudtConstructedTriples.ttl is the resulting set of constructed triples (using Jena ARQ as follows:
`arq --data QUDTunit.ttl --query qudtTest2.rq > qudtConstructedTriples.ttl`

unifyTest2.rq is a SPARQL CONSTRUCT query (intended to operate on qudtConstructedTriples.ttl and rec20ConstructedTriples.ttl ) to construct mappings from QUDT unit resources to the corresponding UN/CEFACT unit resources, currently expressed as an `owl:sameAs` relationship.

unifyResults.ttl is the resulting set of constructed triples (using Jena ARQ as follows):
`arq --data qudtConstructedTriples.ttl --data rec20ConstructedTriples.ttl --query unifyTest2.rq > unifyResults.ttl`
It includes triples such as:

`rec20:kelvin  owl:sameAs  unit:K .`
`unit:K  owl:sameAs  rec20:kelvin .`

UNECE_Rec20_Bad_IRIs.txt lists errors about invalid URI structures currently appearing within https://service.unece.org/trade/uncefact/vocabulary/rec20.jsonld due to invalid characters.

There are gaps in the cross-references from QUDT to UCUM / UN/CEFACT Rec20 - and from UN/CEFACT to QUDT or UCUM.
From the triples in qudtConstructedTriples.ttl it should be possible to also construct triples that map QUDT or UN/CEFACT unit resources to the actual UCUM unit resource URIs (rather than just typed literals), but this will probably require some string manipulation
