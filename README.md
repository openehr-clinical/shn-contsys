SHN Contsys clinical modelling
==============================

Resources related to the development of clinical information model components aligned to the ISO 1340 System of Concepts to support continuity of care (Contsys).

The aim is to develop a set of archetypes and templates which best represent an approach to implementation of the Contsys concepts within an EHR. 

The archetypes have been developed against the openEHR reference model.

This work is being carried out as part of the EU SemanticHealthNet project.

###Available resources

* docs
  * mindmaps (design documents from which archetypes and template are built)
  * other design and reference documents
  
* models
  * archetypes (clinical information components)
  * templates (datasets built from clinical information components)
  * technical (technical artefacts including schema and example instances
	
	
A. Health Thread / Health Issue / Healthcare Matter
================================================

##Overview

One of the more difficult constructs seen in electronic health records is the 'problem list', 'health thread' or 'health concern'. A key function of such constructs is to maintain a list (perhaps with nested lists), that pulls together potentially disparate health record items to summarise or re-phrase some aspect of the patient's current health status. 

In longitudinal records the health thread often has a specific status as the 'Problem list', being maintained as a holistic summary of the patient's health status but both the HL7 Health Concern and CEN Health Thread rescognise other more transient uses of the construct, perhaps within the context of a Care plan or, referral or discharge summary. It might be argued that the traditional secondary care reporting paradigm of "Primary diagnosis, Co-morbidity, Other problems" is a style of Health Thread, optimised for episodic care.

The piece of work, part of the EU SemanticHealthnet project, explores if/how the Contsys definitions relating to Health Thread can be represented as openEHR archetypes and templates.

 
## Relevant Contsys definitions

### Healthcare matter

[Contsys - Healthcare matter](https://contsys.org/concept/healthcare_matter) 

**Term:** healthcare matter

**Synonym:** care matter

**Definition:** representation of a matter related to the health of a subject of care and/or the provision of healthcare to that subject of care, as identified by one or more healthcare actors

**NOTE 1** Healthcare matter is a very broad and flexible concept that includes anything related to the health or the healthcare of a subject of care. This means that health conditions, healthcare activities, health problems, the result of healthcare activities, etc. all are possible to be identified as healthcare matters. Thereby healthcare matter might have several specializations and further associations.

**NOTE 2** According to this definition, a healthcare matter can represent a disease, an illness or another kind of health condition. In addition a healthcare matter may represent, for example, a request for a procedure (therapeutic or preventive) by the subject of care or another healthcare actor.

**NOTE 3** Concepts described and/or identified in a clinical terminology may represent types of healthcare matter.

**NOTE 4** Other specializations of this concept than those included in this standard, may be created when needed.

**EXAMPLES** A loss of weight, an immunization, a heart attack, a drug addiction, a case of meningitis in the school, a water fluoridation, a health certificate, an injury, dermatitis, an X-ray investigation, an arthroscopy, an administration of an oral antibiotic, a post-operative infection.


####openEHR alignment notes

Aligns to : ENTRY archetypes

Healthcare matter is an extremely broad concept but certainly includes the scope of the ENTRY class in openEHR. Health threads may include references to very different kinds of health information, including administrative data, medications, observations,procedures and diagnoses.

In openEHR, HealthCare matters included within a Health Thread will be represented by any ENTRY archetypes, though the majority are likelty to be Problem/Diagnosis and Procedure archetypes. 


###Health thread
[Contsys - Health thread](https://contsys.org/concept/health_thread )

**Term:** health thread

**Definition:** defined association between healthcare matters as determined by one or more healthcare actors

**NOTE 1** A health thread reconciles a range of healthcare matters reflecting the variety of scopes of healthcare actors, particularly of healthcare providers.

**NOTE 2** A health thread inherently associates the healthcare processes as well as the healthcare activity period elements referring to those healthcare matters.

**NOTE 3** A health thread may be established by a team (e.g. a coordination committee).

**NOTE 4** A health thread can be built step-by-step, by allowing each healthcare professional to add their perspective into a common health thread.

**NOTE 7** Under the responsibility of a designated healthcare actor, a health thread linking several healthcare matters can describe an episodes of care bundle, for instance, a partial or comprehensive synthesis of healthcare actor related episodes of care.

**NOTE 8** A collective decision (before, during or after the healthcare interventions) may define a health thread and so the idea of the 'episode' accepted by all the healthcare professionals involved.

**NOTE 9** Two health conditions may sometimes only be recognized as belonging to the same health thread late in the process of care. Conversely, two health conditions thought initially to belong to the same health thread may need to be separated later.

**NOTE 10** Since a health thread links any number of healthcare matters; it also may link health threads linking other health issues. Hence, a health thread may be considered an aggregation of health issues and/or health threads.

**EXAMPLES**

* A low back pain, known for many years by the subject of care's GP, treated several times by the physiotherapist who labelled it a scoliosis and currently a candidate for a specific orthopaedic intervention.

* A case labelled social problem by the GP after being treated by the psychiatrist for minor depression and the rheumatologist for osteoarthritis.

* Type 2 diabetes treated by a GP, a nurse, an endocrinologist

####openEHR alignment notes

**Aligns to:** new EVALUATION 'Health Thread' archetype

There is currently no direct equivalent of a Health thread construct in openEHR. The requirement is for an EVALUATION archetype which acts as the root of a potentially complex tree-structure containing Health issues as branches and Healthcare matters as leaf nodes. The Health Thread defines the clinical owner, start and end dates and the type of thread.

The nested relationships between the parent Health Thread and its child components requires the use of a LINK attribute on the Health Thread archetype root node.

e.g.

`Health Thread
   Health Issue
	   Healthcare Matter
	   Healthcare Matter
   Health Issue
	   Healthcare Matter`

###Health issue
[Contsys - Health issue](https://contsys.org/concept/health_issue)

**Term:** health issue

**Definition:** representation of an issue related to the health of a subject of care as identified by one or more healthcare actors

**NOTE** According to this definition, a health issue can correspond to a health problem, a disease, an illness or another kind of health condition.

**EXAMPLES** A loss of weight, a heart attack, a drug addiction, an injury, dermatitis.

####openEHR alignment notes

Aligns to : new EVALUATION Health Issue archetype

The Contsys definition has been interpreted such that Health Issue is equivalent to HL7 Health concern i.e. it represents a meta-wrapper around one or more existing clinical record entries, grouping or nesting them in ways which help elucidate progression of an illness, relationship to a clinical process or summarise a patients overall health status. Each health issue must have a name, often coded, may have optional start and end dates and allows status to be carried as active, inactive or closed, in alignment with LL Weed's original POMR paradigm.

Each Health Issue is realted to its parent Health Thread via a LINK pointing to its root archetype node. The Health Issue itself uses LINKS to assert relationships between itself and its child ENTRIES. Thess can be other Health Issues but more often will be HealthCare matters.


Artefacts
=========

###Mindmaps

Health Thread and Health Issue archetypes: [Xmind format](./docs/mindmaps/Health Thread.xmind) [png format](./docs/mindmaps/Health Thread.png)

Problem List Example: : [Xmind format](./docs/mindmaps/Problem list example.xmind) [png format](./docs/mindmaps/Problem list example.png)


###Archetypes

[Health Thread](./models/entry/evaluation/openEHR-EHR-EVALUATION.health_thread_contsys.v1.adl)  
[Health Issue](./models/entry/evaluation/openEHR-EHR-EVALUATION.health_issue_contsys.v1.adl)

**Example Healthcare matters**  
[Problem / diagnosis](./models/entry/evaluation/openEHR-EHR-EVALUATION.problem_diagnosis.v1.adl)  
[Procedure](./models/entry/action/openEHR-EHR-ACTION.procedure.v1.adl)




  
