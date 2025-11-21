
---

# **Iteration 2**

## **Step 1: Review Inputs**

For Iteration 2, we continue with the AIDAP system design. The inputs remain the same as Iteration 1, but now we focus on refining the architectural elements to support primary functionality in detail.

---

## **Step 2: Establish Iteration Goal by Selecting Drivers**

**Iteration Goal:** Identify structures to support primary functionality by defining specific modules, their responsibilities, and interfaces.

**Selected Drivers for this Iteration:**

* UC-1 (Query Academic Information) – Primary use case requiring AI interpretation
* UC-2 (Manage Personal Academic Dashboard) – Requires data aggregation from multiple sources
* UC-3 (Manage Course Content & Analytics) – Involves authentication and analytics processing

---

## **Step 3: Choose One or More Elements of the System to Refine**

The elements refined in this iteration include modules located in the different layers defined by prior reference architectures:

* **Application Logic Layer** – Decompose into specific business logic modules
* **AI Services Layer** – Define AI/NLP components and their interfaces
* **Integration Layer** – Define adapters for external systems
* **Data Layer** – Define data access and persistence modules

Supporting system functionality requires collaboration among components across these layers.

---

# **Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers**


| **Design Decision**                                              | **Rationale**                                                                                                                                                                                                                                                                                                                                                                                        |
| ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create a Domain Model for the application                        | Before starting a functional decomposition, it is necessary to create an initial domain model for the system, identifying the major entities in the domain, along with their relationships. There are no good alternatives. A domain model must eventually be created, or it will emerge in a suboptimal fashion, leading to an ad hoc architecture that is hard to understand and maintain.         |
| Identify Domain Objects that map to functional requirements      | Each unique functional aspect of the application needs to be encapsulated in a self-contained building block, a domain object. An alternate option would be to not worry about domain objects and go directly from layers to modules, but this would result in a lack of separation of concerns and make it difficult to modify specific AI capabilities.                                            |
| Decompose Domain Objects into general and specialized Components | Domain objects represent complete functionality, but this functionality is backed by finer-grained pieces in the layers. These finer-grained “components” are what we refer to when we mention modules. Specialization corresponds to the layer where the module exists (e.g., UI modules). There are no good alternatives if functionality is to be properly supported.                             |
| Use Python/Django framework and related technologies             | Django is a popular framework that facilitates web application development. It works with Python AI/ML frameworks. Alternatives such as Flask or Node.js were considered, but Django provides more built-in capability for rapid development speed and is familiar to the team. Building custom NLP from scratch was considered but rejected due to lack of maturity and excessive development time. |

---

# **Step 5 – Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces**

| **Design Decision and Location**                | **Rationale**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Create only an initial domain model             | The entities in the primary use case need to be identified and modeled, but only an initial domain model is created at this stage to speed early design.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Map the system use cases to domain objects      | Domain objects can be identified by reviewing the system’s use cases. To support distributed team work, domain objects are identified for all use cases from Phase 1. This ensures full functional coverage. <br><br>**Mapping ensures all functionality is covered:**<br>• UC-1 → Query Processor, AI Interpreter, Knowledge Retriever<br>• UC-2 → Dashboard Manager, Data Aggregator<br>• UC-3 → Content Manager, Analytics Engine<br><br>The architect handles only primary use cases; additional modules are identified by other team members to divide work.                                                                                                                                                                                                                                                                 |
| Identify need for testing (CRN-6)               | With modules identified, the architect notes that testing is required. **CRN-6: A majority of modules shall be unit tested.** UI modules are excluded because they are harder to test independently.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Decompose the domain objects across layers      | Ensures each module supports a cohesive functional responsibility in alignment with the AIDAP architecture. Only primary use cases are decomposed this iteration. <br><br>**Decomposition results:**<br>• **Presentation CS:** QueryView, DashboardView, ContentManagementView<br>• **Business Logic CS:** QueryViewController, DashboardViewController, ContentViewController<br>• **Data CS:** RequestManager<br>• **Services SS:** RequestService<br>• **Business Logic SS:** QueryController, DashboardController, ContentController, Domain Entities<br>• **AI Services SS:** NLPInterpreter, SemanticSearch, ResponseGenerator<br>• **Integration SS:** IntegrationServiceFacade, LMSAdapter, CalendarAdapter, RegistrationAdapter, CacheManager<br>• **Data SS:** UserRepository, CourseRepository, QueryHistoryRepository |
| Connect modules using Django                    | This approach supports module-level unit testing and aligns with CRN-6.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Associate ORM framework with data-layer modules | ORM mapping is encapsulated in the Data SS layer. Django’s ORM is used to persist domain entities in the relational database.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

---
