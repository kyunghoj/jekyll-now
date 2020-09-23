> The W3C OWL 2 Web Ontology Language (OWL) is a Semantic Web language designed
> to represent rich and complex knowledge about things, group of things, and
> relations between things. 

OWL은 사물, 사물들의 모임, 그리고 사물 간의 관계에 대한 풍부하고 복잡한 지식을
나타내기 위해 설계된 Semantic Web 언어입니다. 

> OWL is a computational logic-based language such that knowledge expressed in
> OWL can be reasoned with by computer programs either to verify the consistency
> of that knowledge or to make implicit knowledge explicit. 

OWL은 계산 논리학 기반의 언어입니다. 따라서 OWL로 표현된 지식을 컴퓨터
프로그램을 통해 그 지식의 일관성을 검증할 수 있고, 또한 암묵적인 지식을
명시적으로 만들 수도 있습니다.

Ontologies 라고 알려진 OWL 문서들은 WWW을 통해 출판될 수 있고 다른 OWL
ontologies를 참고하거나, 참고될 수 있습니다. OWL은 W3C의 Semantic Web 기술
스택의 일부이며, 그 스택은 RDF와 SPARQL을 포함합니다. 

OWL 2 is a language for expressing *ontologies*. The term *ontology* has a
complex history both in and out of computer science, but we use it to mean a
certain kind of computational artifact -- i.e., something akin to a program, an
XML schema, or a web page -- generally presented as a document.

An ontology is a set of precise descriptive statements about some part of the
world (usually referred to as the *domain of interest* or the *subject matter*
of the ontology). Precise descriptions satisfy several purposes: most notably,
they prevent misunderstandings in human communication and they ensure that
software behaves in a uniform, predictable way and works well with other
software. 

Ontology는 세상의 어떤 부분에 대한 정밀한 묘사를 하는 선언들의 집합입니다. 
(그 세상의 '부분'은 보통 domain of interest 또는 subject matter라고 지칭합니다)
정밀한 묘사는 여러 가지의 목적을 만족시킵니다. 인간의 의사소통에서 오해를
방지하며 소프트웨어가 일관성 있고, 예측가능한 방식으로 행동하며, 다른
소프트웨어와 함께 잘 작동하리라는 것을 보장합니다. 

In order to precisely describe a domain of interest, it is helpful to come up
with a set of central terms -- often called vocabulary -- and fix their
meaning. Besides a concise natural language definition, the meaning of a term
can be characterized by stating how this term is interrelated to the other
terms. A *terminology*, providing a vocabulary together with such interrelation
information constitutes an essential part of a typical OWL 2 document. Besides
this terminological knowledge, an ontology might also contain so called
assertional knowledge that deals with concrete objects of the considered domain
rather than general notions.

OWL 2 is not a programming language: OWL 2 is *declarative*, i.e., it describes
a state of affairs in a logical way. Appropriate tools (so-called reasoners)
can then be used to infer further information about that state of affairs. How
these inferences are realized algorithmically is not part of the OWL document
but depends on the specific implementations. Still, the correct answer to any
such question is predetermined by the formal semantics (which comes in two
versions: the *Direct Semantics* and the *RDF-Based Semantics*. 
Only implementations that comply with these semantics will be regarded as OWL 2
conformant. Through its declararive nature, the activity of creating OWL 2
documents is conceptually different from programming. Still, as in both cases
complex formal documents are created, certain notions from software engineering
can be transferred to ontology engineering, such as methodological and
collaborative aspects, modularization, patterns, etc. 

OWL 2 is not a schema language for syntax conformance. Unlike XML, OWL 2 does
not provide elaborate means to prescribe how a document should be structured
syntactically. In particular, there is no way to enforce that a certain piece
of information (like the social security number of a person) has to be
syntactially present. This should be kept in mind as OWL has some features that
a user might misinterpret this way. 

OWL 2 is not a database framework. Admittedly, OWL 2 documents store
information and so do databases. Moreover a certain analogy between assertional
information and database content as well as terminological information and
database schemata can be drawn. However, usually there are crucial differences
in the underlying assumptions (technically: the used semantics). If some fact
is not present in a database, it is usually considered false (the so-called
*closed-world assumption*) whereas in the case of an OWL 2 document it may
simply be missing (but possibly true), following the *open-world assumption*.
Moreover, database schemata often come with the prescriptive constraint
semantics mentioned above. Still, technically, databases provide a viable
backbone in many ontology-oriented systems.

## 3. Modeling Knowledge: Basic Notions

OWL 2 is a knowledge representation language, designed to formulate, exchange, and reason with knowledge about a domain of interest. Some fundamental notions should first be explained to understand how knowledge is represented in OWL 2. These basic notions are:

* Axioms: the basic statements that an OWL ontology expresses
* Entities: elements used to refer to real-world objects
* Expressions: combinations of entities to form complex descriptions from basic ones



