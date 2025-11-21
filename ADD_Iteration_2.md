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

Completed views for this iteration:

### Refined Logical Architecture (Module View)

<img width="947" height="694" alt="image" src="https://github.com/user-attachments/assets/ed5c0677-4b15-4815-a5bd-ef5b972c295e" />

Sequence Diagrams

<img width="1175" height="700" alt="image" src="https://github.com/user-attachments/assets/0aa34733-7447-4788-8601-0960bdc58db4" />


### Table 1 — UC-1 & UC-2 Interface Specifications 

| **Component**          | **Method**                                | **Description** |
|------------------------|--------------------------------------------|-----------------|
| **QueryController**    | processQuery(text)                         | Receives the student’s query and starts the processing workflow. |
| **QueryController**    | Response(processed)                        | Returns the final processed response back to the student. |
| **AIServiceFacade**    | analyze(text, lang)                        | Performs NLP analysis and extracts intent from the user query. |
| **RegistrationAdapter**| fetchContext(userId / courseId)            | Retrieves contextual enrollment or course data needed for intent resolution. |
| **QueryHistoryRepo**   | save(query, response)                      | Stores query and response to maintain analytics and history. |
| **DashboardManager**   | getDashboard(session)                      | Initiates dashboard data assembly by calling IntegrationFacade. |
| **IntegrationFacade**  | fetchLMSData(userId)                       | Retrieves LMS-based data required for dashboard generation. |
| **IntegrationFacade**  | fetchCalendarData(userId)                  | Retrieves calendar events and academic deadlines. |
| **IntegrationFacade**  | enrichCourseInfo()                         | Combines LMS + calendar + metadata into unified dashboard results. |
| **LMSAdapter**         | getCourseProgress(userId)                  | Retrieves the student’s course completion and grade progress. |
| **CalendarAdapter**    | getUpcomingEvents(userId)                  | Returns upcoming university events, classes, and deadlines. |

<img width="1315" height="644" alt="image" src="https://github.com/user-attachments/assets/56468f66-6543-4846-a6e8-97c925dc4ac8" />

### Table - UC-4 & UC-5 Interface Specifications 

| **Component**              | **Method**                                      | **Description** |
|----------------------------|--------------------------------------------------|-----------------|
| **NotificationManager**    | broadcast(courseID, msg)                        | Initiates the announcement broadcast workflow triggered by the lecturer. |
| **RegistrationAdapter**    | getStudentList(courseID)                        | Retrieves all enrolled students for the selected course. |
| **EmailGateway**           | sendEmail(to, subject, body)                    | Sends an announcement email to each student in the list. |
| **EmailGateway**           | email_sent                                      | Confirmation signal indicating successful email delivery. |
| **DataRepository**         | saveCommunication(courseID, msg, timestamp)     | Logs the broadcast event and stores communication history. |
| **Admin**                  | updateConfig(LMS_API_KEY)                       | Updates LMS-related configuration values in the system. |
| **AdminController**        | testConnection(apiKey)                          | Validates whether the submitted API key is functional with LMS. |
| **LMSAdapter**             | testConnection(apiKey)                          | Performs an API-level test call to verify LMS API key correctness. |
| **ConfigurationRepository**| save(LMS_API_KEY)                               | Stores updated configuration values securely (encrypted). |
| **SystemLogger**           | logConfigChange(adminID, key, status)           | Records configuration changes for auditing and traceability. |

---
## Step 7 - Analyze Current Design & Review Iteration 

Driver / Item | Not Addressed | Partially Addressed | Completely Addressed | Decisions Made
-------------|----------------|----------------------|-----------------------|----------------
UC-1 Query Institutional Data |  |  | ✓ | Core modules (QueryController, AIServiceFacade, IntegrationFacade) fully mapped.
UC-2 Post Announcements |  |  | ✓ | Notification pipeline defined (NotificationManager, EmailGateway, DataRepository).
UC-4 Broadcast Notifications |  |  | ✓ | End-to-end broadcast flow introduced (student lookup → email send → log).
UC-5 Update Configurations |  |  | ✓ | AdminController + ConfigRepository + LMSAdapter integrated for secure updates.
UC-7 External Data Sync |  |  | ✓ | Synchronization logic established via ExternalAPIConnector + ExternalDataService.
QA-1 Performance |  |  | ✓ | Parallel data calls + caching support fast responses.
QA-2 Availability |  | ✓ |  | Recovery and failover strategies noted but not fully defined yet.
QA-3 Scalability | ✓ |  |  | No new improvements; relies on future cloud deployment.
QA-4 Security |  |  | ✓ | RBAC, config validation, encrypted storage incorporated.
CON-1 Security & Privacy | ✓ |  |  | No new compliance decisions in this iteration.
CON-4 REST API Integration |  |  | ✓ | Unified ExternalAPIConnector chosen for LMS/Calendar/Registration APIs.
CRN-4 Data Persistence |  |  | ✓ | Communication logs + config history stored via DataRepository + ConfigRepository.
CRN-5 Synchronization |  |  | ✓ | External sync workflow now complete for institutional data.

