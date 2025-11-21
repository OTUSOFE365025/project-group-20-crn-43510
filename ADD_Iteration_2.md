Include in this file the 7 steps for Iteration 2
# **Step 1 — Review Inputs**

| Step                  | Content                                                                                                                                                                                  |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Step 1: Review Inputs | In Iteration 2, we are moving forward with the AIDAP system design.                                                                                                                      |
|                       | The inputs are the same as were utilized in Iteration 1; however, we are now going to work on refining the architectural components of the system to support primary function in detail. |

---

# **Step 2 — Establish Iteration Goal**

| Item            | Content                                                                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------------- |
| Iteration Goal  | Define specific modules, purpose and purpose interfaces to enable supporting structures to enable primary function. |
| Selected Driver | UC-1 (Query Academic Information) - Primary use case requiring AI interpretation                                    |
| Selected Driver | UC-2 (Manage Personal Academic Dashboard) - Requires data aggregation from multiple sources                         |
| Selected Driver | UC-3 (Manage Course Content & Analytics) - Involves authentication and analytics processing                         |

---

# **Step 3 — System Elements to Refine**

| Step   | Content                                                                                                                                                                                |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Step 3 | This version of the system functionality is based on the modules described in the layers of previous reference architectures.                                                          |
|        | The Application Logic Layer was broken down in order to create clear business logic modules that can separate responsibilities.                                                        |
|        | The AI Services Layer was expanded to specify AI and NLP components, as well as their interfaces for interaction with other interfaces of the system.                                  |
|        | Specific adapters were identified in the Integration Layer to describe communication with external systems and services.                                                               |
|        | The Data Layer was further refined to specify data access and persistence modules.                                                                                                     |
|        | Overall, the system functionality zone relies on the close connections of the interdependent modules in the layers to give a coherent and logical architectural view of functionality. |

---

# **Step 4 — Design Concepts**

| Design Decision                                                  | Rationale                                                                                                                                                                                                                |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Create a Domain Model for the application                        | Prior to conducting functional decomposition, an initial domain model for the system must be developed that defines the primary domain entities and the relationships between them.                                      |
|                                                                  | There are no good alternatives. The domain model must in the end be articulated in some fashion or it will emerge in an inefficient, ad hoc manner that is difficult to understand and maintain.                         |
| Identify Domain Objects that map to functional requirements      | Each unique functional aspect of the application needs to be encapsulated in a self-contained building block, a domain object.                                                                                           |
|                                                                  | An alternate option would be to not worry about domain objects and to go directly from layers to modules; however, this could leave the requirement open for consideration.                                              |
|                                                                  | Another option considered and dismissed was to make a single monolithic AI module that lacked keeping concerns separate and would make it difficult to change one AI capability without affecting others.                |
| Decompose Domain Objects into general and specialized Components | Domain objects represent complete functionality, but this functionality is backed by finer-grained pieces in the layers.                                                                                                 |
|                                                                  | The "components" in this pattern are what we are speaking of when we mention modules.                                                                                                                                    |
|                                                                  | Specializing modules is associated with the layers where they exist (for example, UI modules).                                                                                                                           |
|                                                                  | There is no suitable alternative to decomposing layers into modules if support functionality is needed.                                                                                                                  |
| Use Python/Django framework and related technologies             | Django is a popular framework that facilitates web application development.                                                                                                                                              |
|                                                                  | It works with Python AI/ML frameworks.                                                                                                                                                                                   |
|                                                                  | Another considered alternative is Flask (somewhat lighter) or Node.js, but Django was chosen because it provides more built-in capability for rapid development speed and the team is familiar with it for productivity. |
|                                                                  | Building custom NLP from scratch was also considered but voided as it would be too much time and less mature than existing solutions.                                                                                    |

---

# **Step 5 — Instantiate Architectural Elements**

| Design Decision and Location                           | Rationale                                                                                                                                                                                                                        |
| ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create only an initial domain model                    | The entities in the primary use case need to be to be identified and modeled but only an initial domain model is created to speed this early design phase.                                                                       |
| Map the system use cases to domain objects             | Domain objects can be initially identified by reviewing the use cases of the system.                                                                                                                                             |
|                                                        | To support work broken up across the teams, domain objects are identified for all of the use cases indicated in the use case model from Phase 1.                                                                                 |
|                                                        | If you can identify how the modules will support all use cases, that ensures you identify modules that support all of the functionality.                                                                                         |
|                                                        | **Mapping ensures all functionality is covered:**                                                                                                                                                                                |
|                                                        | • UC-1 → Query Processor, AI Interpreter, Knowledge Retriever                                                                                                                                                                    |
|                                                        | • UC-2 → Dashboard Manager, Data Aggregator                                                                                                                                                                                      |
|                                                        | • UC-3 → Content Manager, Analytics Engine                                                                                                                                                                                       |
|                                                        | The architect would do this for just the primary use cases. Another team member will want to identify the additional modules, leading to allocation of work to team members.                                                     |
|                                                        | Now the architect has determined the set of modules, and identifies that in order to assess these modules, they will need to test these modules.                                                                                 |
|                                                        | **CRN-6: A majority of modules shall be unit tested.**                                                                                                                                                                           |
|                                                        | A "majority of modules" are discussed because the modules for user interface functionality are not easily tested in isolation.                                                                                                   |
| Decompose the domain objects across the layers         | This approach guarantees that each module fulfills a unified functional responsibility, in compliance with the AIDAP architecture from Iteration 1.                                                                              |
|                                                        | **The decomposition leads to:**                                                                                                                                                                                                  |
|                                                        | • Presentation CS: QueryView, DashboardView, ContentManagementView                                                                                                                                                               |
|                                                        | • Business Logic CS: QueryViewController, DashboardViewController, ContentViewController                                                                                                                                         |
|                                                        | • Data CS: RequestManager                                                                                                                                                                                                        |
|                                                        | • Services SS: RequestService                                                                                                                                                                                                    |
|                                                        | • Business Logic SS: QueryController, DashboardController, ContentController, Domain Entities                                                                                                                                    |
|                                                        | • AI Services SS: NLPInterpreter, SemanticSearch, ResponseGenerator                                                                                                                                                              |
|                                                        | • Integration SS: IntegrationServiceFacade, LMSAdapter, CalendarAdapter, RegistrationAdapter, CacheManager                                                                                                                       |
|                                                        | • Data SS: UserRepository, CourseRepository, QueryHistoryRepository                                                                                                                                                              |
| Connect components using Django                        | This framework employs an approach that makes it possible to support various aspects and unit test the modules (CRN-6).                                                                                                          |
| Associate ORM framework with modules in the data layer | ORM mapping is abstracted into the modules found in the Data SS layer, and Django's built-in ORM framework, which was previously chosen, is associated with these modules to persist domain entities to the relational database. |

---
