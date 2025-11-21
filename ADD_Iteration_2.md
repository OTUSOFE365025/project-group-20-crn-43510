# Iteration 2: Identifying Structures to Support Primary Functionality

---

## Step 1: Review Inputs

For Iteration 2, we continue with the AIDAP system design. The inputs remain the same as Iteration 1, but now we focus on refining the architectural elements to support primary functionality in detail.

---

## Step 2: Establish Iteration Goal by Selecting Drivers

**Iteration Goal:** Identify structures to support primary functionality by defining specific modules, their responsibilities, and interfaces.

**Selected Drivers for this Iteration:**

- **UC-1 (Query Academic Information)** – Primary use case requiring AI interpretation
- **UC-2 (Manage Personal Academic Dashboard)** – Requires data aggregation from multiple sources
- **UC-3 (Manage Course Content & Analytics)** – Involves authentication and analytics processing

---

## Step 3: Choose One or More Elements of the System to Refine


- **Application Logic Layer** – Decompose into specific business logic modules
- **AI Services Layer** – Define AI/NLP components and their interfaces
- **Integration Layer** – Define specific adapters for external systems
- **Data Layer** – Define data access and persistence modules

In general, the support of functionality in this system requires the collaboration of components associated with modules that are located in the different layers.

---

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

In this iteration, several design concepts are selected. The following table summarizes the design decisions:

| **Design Decision** | **Rationale** |
|---------------------|---------------|
| **Create a Domain Model for the application** | Before starting a functional decomposition, it is necessary to create an initial domain model for the system, identifying the major entities in the domain, along with their relationships. There are no good alternatives. A domain model must eventually be created, or it will emerge in a suboptimal fashion, leading to an ad hoc architecture that is hard to understand and maintain. |
| **Identify Domain Objects that map to functional requirements** | Each distinct functional element of the application needs to be encapsulated in a self-contained building block—a domain object.<br><br>One possible alternative is to not consider domain objects and instead directly decompose layers into modules, but this increases the risk of not considering a requirement. Another discarded alternative was to create a single monolithic AI module, but this would result in a lack of separation of concerns and make it difficult to modify specific AI capabilities. |
| **Decompose Domain Objects into general and specialized Components** | Domain objects represent complete sets of functionality, but this functionality is supported by finer-grained elements located within the layers. The "components" in this pattern are what we have referred to as modules. Specialization of modules is associated with the layers where they are located (e.g., UI modules).<br><br>There are no good alternatives to decomposing the layers into modules to support functionality. Direct database access from the presentation layer was considered but discarded as it violates layered architecture principles. |
| **Use Python/Django framework and related technologies** | Django is a widely used framework to support web application development. It integrates well with Python-based AI/ML frameworks.<br><br>Alternative frameworks that were considered include Flask (more lightweight) and Node.js, but Django was selected because it provides more built-in functionality for rapid development and the team is already familiar with it, resulting in greater productivity.<br><br>Building custom NLP from scratch was also considered but discarded as it would be too time-consuming and less mature than existing solutions. |

---

## Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces


| **Design Decision and Location** | **Rationale** |
|----------------------------------|---------------|
| **Create only an initial domain model** | The entities that participate in the primary use cases need to be identified and modeled but only an initial domain model is created, to accelerate this phase of design. |
| **Map the system use cases to domain objects** | An initial identification of domain objects can be made by analyzing the system's use cases. To support work allocation, domain objects are identified for all of the use cases presented in the use case model from Phase 1. This technique ensures that modules that support all of the functionalities are identified.<br><br>**Mapping ensures all functionality is covered:**<br>• UC-1 → Query Processor, AI Interpreter, Knowledge Retriever<br>• UC-2 → Dashboard Manager, Data Aggregator<br>• UC-3 → Content Manager, Analytics Engine<br><br>The architect will perform this task just for the primary use cases. This allows another team member to identify the rest of the modules, thereby allocating work among team members.<br><br>Having established the set of modules, the architect realizes the need to test these modules, so a new architectural concern is identified here:<br><br>**CRN-6: A majority of modules shall be unit tested.**<br><br>Only "a majority of modules" are covered by this concern because the modules that implement user interface functionality are difficult to test independently. |
| **Decompose the domain objects across the layers to identify layer-specific modules with an explicit interface** | This technique ensures each module supports a cohesive functional responsibility aligned with the AIDAP architecture from Iteration 1. Only the primary use cases are decomposed in this iteration.<br><br>**The decomposition leads to:**<br>• **Presentation CS**: QueryView, DashboardView, ContentManagementView<br>• **Business Logic CS**: QueryViewController, DashboardViewController, ContentViewController<br>• **Data CS**: RequestManager<br>• **Services SS**: RequestService<br>• **Business Logic SS**: QueryController, DashboardController, ContentController, Domain Entities<br>• **AI Services SS**: NLPInterpreter, SemanticSearch, ResponseGenerator<br>• **Integration SS**: IntegrationServiceFacade, LMSAdapter, CalendarAdapter, RegistrationAdapter, CacheManager<br>• **Data SS**: UserRepository, CourseRepository, QueryHistoryRepository |
| **Connect components associated with modules using Django** | This framework uses an approach that allows different aspects to be supported and the modules to be unit-tested (CRN-6). |
| **Associate ORM framework with modules in the data layer** | ORM mapping is encapsulated in the modules that are contained in the Data SS layer. Django's built-in ORM framework previously selected is associated with these modules to handle persistence of domain entities to the relational database. |

While the structures and interfaces are identified in this step of the method, they are captured in the next step.

---
## Step 6: Sketch Views and Record Design Decisions
