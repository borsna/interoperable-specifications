# Rules for interoperable specifications

The rules for interoperable specifications are divided into five parts:

1. Rules for interoperable specifications - PROF-INSPEC
2. Rules for data vocabularies - RDFS-INSPEC
3. Rules for terminologies - SKOS-INSPEC
4. Rules for application profiles - SHACL-INSPEC
5. Rules for diagrams - SVG-INSPEC

## Rules for interoperable specifications - PROF-INSPEC

For a specification to be considered a interoperable specification the following must apply:

><a name="INSPEC1"></a> **Rule INSPEC-1:** The "interoperable specification resource" and its parts MUST have URIs and be described with PROF, the interoperable specification resource must be typed as `prof:Profile`

><a name="INSPEC2"></a> **Rule INSPEC-2:** An interoperable specification part MAY be a data vocabulary, a terminology, an application profile or a diagram. Other parts may exist but have no prescribed meaning by the interoperable specification profile.

><a name="INSPEC3"></a> **Rule INSPEC-3:** Each data vocabulary MUST be possible to interpret as RDFS-INSPEC.

><a name="INSPEC4"></a> **Rule INSPEC-4:** Each terminology MUST be possible to interpret as SKOS-INSPEC.

><a name="INSPEC5"></a> **Rule INSPEC-5:** The interoperable specification MUST list all data vocabularies and terminologies it reuses explicitly and where they are introduced via the `prof:isInheritedFrom`

><a name="INSPEC6"></a> **Rule INSPEC-6:** Each application profile MUST be possible to interpret as SHACL-INSPEC

><a name="INSPEC7"></a> **Rule INSPEC-7:** An interoperable specification MUST contain at least one data vocabulary or application profile

## Rules for data vocabularies - RDFS-INSPEC

RDFS-INSPEC builds on top of the RDF Schema specification by providing the following additional restrictions:

><a name="DV1"></a> **Rule DV-1:** A data vocabulary MUST be expressed in a single RDF Dataset. (*)

><a name="DV2"></a> **Rule DV-2:** There MUST be a single "data vocabulary resource" typed as `owl:Ontology` in the RDF Dataset

><a name="DV3"></a> **Rule DV-3:** All included classes and properties of the data vocabulary MUST point to the "data vocabulary resource" via the `rdfs:isDefinedBy` property

><a name="DV4"></a> **Rule DV-4:** All included classes and properties as well as the data vocabulary resource MUST have URIs

><a name="DV5"></a> **Rule DV-5:** Classes and properties from other vocabularies MAY BE included in the RDF Dataset, e.g. when being pointed to via `rdfs:subClassOf` and `rdfs:subProperty`, but MUST NOT point to the same "data vocabulary resource" via the `rdfs:isDefinedBy` property

><a name="DV6"></a> **Rule DV-6:** The "data vocabulary resource" must be indicated via the `dcterms:requires` property from the "interoperable specification resource" (introduced in Rule INSPEC-1)


## Rules for terminologies - SKOS-INSPEC

SKOS-INSPEC builds on top of the SKOS specification by providing the following additional restrictions:

><a name="TE1"></a> **Rule TE-1:** A terminology MUST be expressed in a single RDF Dataset. (*)

><a name="TE2"></a> **Rule TE-2:** There MUST be a single "terminology resource" with a URI typed as `skos:ConceptScheme` in the RDF Dataset

><a name="TE3"></a> **Rule TE-3:** All concepts and collections in the terminology MUST point to the "terminology resource" via the `skos:inScheme` property

><a name="TE4"></a> **Rule TE-4:** All concepts, collections as well as the terminology resource MUST have URIs

><a name="TE5"></a> **Rule TE-5:** The "terminology resource" must be indicated via the `dcterms:requires` property from the "interoperable specification resource" (introduced in Rule INSPEC-1)

## Rules for application profiles - SHACL-INSPEC

SHACL-INSPEC builds on top of the SHACL specification by providing additional restrictions. Since SHACL is a rich language the following rules does not unfortunately cover all situations. For a more complete treatment see the [SHACL-INSPEC separate document](ap.md) for patterns on how to use the profile in various situations.

><a name="AP1"></a> **Rule AP-1:** An application profile MUST be expressed in a single RDF Dataset (**)

><a name="AP2"></a> **Rule AP-2:** There MUST be a single application profile resource with a URI in the RDF Dataset AND it must be the same as the "interoperable specification resource" (introduced in Rule INSPEC-1)

><a name="AP3"></a> **Rule AP-3:** All property shapes with a severity of `sh:VIOLATION` and with at least one constraint (`sh:and` for specialization does not count) are considered **reusable** and they MUST have URIs and MUST also be pointed to from the application profile resource via the `dcterms:hasPart` property

><a name="AP4"></a> **Rule AP-4:** All node shapes with a severity of `sh:VIOLATION` are considered **reusable** and MUST have URIs and MUST also be pointed to from the application profile resource via the `dcterms:hasPart` property

><a name="AP4"></a> **Rule AP-4:** Reusable node shapes are considered **main** if they have a target declaration, otherwise they are considered **alternative**

><a name="AP6"></a> **Rule AP-6:** All reusable node shapes as well all property shapes pointed to from those MUST provide a label and MAY also provide a definition and a usage note

><a name="AP7"></a> **Rule AP-7:** Shapes MAY inherit and further restrict other reusable shapes via the `sh:and` construct

><a name="AP8"></a> **Rule AP-8:** Shapes MAY indicate that they are based on other reusable shapes via the dcterms:isVersionOf property

><a name="AP9"></a> **Rule AP-9:** Shapes that are inherited or based on MAY reside in other RDF Datasets as long as the dataset is pointed to via owl:import AND there is a `prof:isProfileOf` relation between the application profile resources

><a name="AP10"></a> **Rule AP-10:** All classes and properties referred to via shapes should be explicitly indicated via the `dcterms:requires` property from the "interoperable specification resource" (introduced in Rule INSPEC-1)


(*) Both RDFS and SKOS introduce building blocks (classes and properties) for for defining things (other classes, properties, concepts, collections) in an open world manner. However, both vocabularies and terminologies needs to work in the context of semantic specifications where it is stated explicitly what is included, hence it corresponds to a closed world perspective. Hence, in both RDFS-INSPEC and SKOS-INSPEC there is a restriction to assume that everything needed is provided in the indicated RDF Datasets.

(**) SHACL supports imports declared via owl:import, the rules are written from the perspective that these are respected and all RDF Datasets (potentially recursively) are imported first into a single RDF Dataset.