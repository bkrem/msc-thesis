# Chapter 3
# Requirements and Analysis

## 3.1 Problem Statement
As outlined in chapters 1 & 2, the key problems the project was trying to solve the way in which students relate to their school work on an everyday basis, as well as investigating the feasibility of a system which uses a distributed blockchain data structure to verify and store data about events which represent collaborative work.  
The conceptual diagram below shows how the basic components and interactions within this system might look.

![Concept Diagram](../diagrams/conceptDiagram.png)
**Figure x - Concept diagram**

## 3.2 Requirements
In a typical software engineering project, there is a need for strong levels of communication between all the stakeholders involved in the endeavour, in order to ensure all the needs the software should meet are in fact met _[(REF)]()_. A frequent phenomenon within these interactions is that requirements are revealed to the development team in a haphazard way or are simply misinterpreted by the team _[(REF?)]()_.  
Fortunately these common pitfalls were a moot point within this project, as the act of establishing requirements was entirely focused around the question: "What functionality does the system require, at a minimum, to fulfill its stated aims?". This provided a clear focus on what was absolutely needed for a minimum viable product (henceforth MVP)<sup>[(MVP; better ref)](https://www.techopedia.com/definition/27809/minimum-viable-product-mvp)</sup> that an individual and/or group of individuals could use in a meaningful way. This meant both functional and non-functional requirements were focused around three domains: tasks, users, and teams. The requirements were prioritised according to the MoSCoW system<sup>[(MoSCoW)](https://www.dsdm.org/content/moscow-prioritisation)</sup>, with requirements being ranked from "Must Have" through "Should Have" and "Could Have", with the final category being "Won't Have (in this development cycle)". The project's "Must Have" requirements had to be strictly limited to what was  as there was a large number of known unknowns (e.g. the Solidity language) and unknown unknowns (unforeseeable issues with the API, blockchain or development environment). The "Should Have" and "Could Have" categories therefore largely express targets for more sophisticated future iterations of the system.  

ID     |  Functional Requirements                                                                                                                                                          |  Category              |  MoSCoW Priority
-------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|-----------------
FRQ1   |  Users should be able to sign up by providing simply a username, password and, optionally, an email address                                                                       |  Account registration  |  Must           
FRQ2   |  Users should be able to login using a username and password                                                                                                                      |  Login                 |  Must           
FRQ3   |  Any team member should be able to register a task for their team on the network, by specifying team members involved                                                             |  Tasks                 |  Must           
FRQ4   |  Any team member is able to view both outstanding and completed tasks                                                                                                             |  Tasks                 |  Must           
FRQ5   |  If user is not a member of a team, they should be able to start a new team by adding other members via their usernames                                                           |  Team                  |  Must           
FRQ6   |  Upon resolution the task reward should be added to the participants’ scores automatically and immediately                                                                        |  Tasks                 |  Must           
FRQ7   |  Users should be able to issue a request to join an existing team or create a new one upon signup                                                                                 |  Account registration  |  Should         
FRQ8   |  Users should receive sign up confirmation to verify email address                                                                                                                |  Account registration  |  Should         
FRQ9   |  Any team member should be able to opt out of a task they were entered for if they are not in fact involved                                                                       |  Tasks                 |  Should         
FRQ10  |  Each team member can see how many other members have completed a given task without identities being revealed in the process (i.e. by simply showing a count or percentage)      |  Tasks                 |  Should         
FRQ11  |  Individuals should be able to provide public links to their completed tasks and general profile                                                                                  |  Tasks                 |  Should         
FRQ12  |  Users should be able to see their team’s current global ranking                                                                                                                  |  Team                  |  Should         
FRQ13  |  Users should be able to recover their password if they have forgotten it                                                                                                         |  Login                 |  Should         
FRQ14  |  Users should be able to view incomplete tasks their involved in and past completed tasks                                                                                         |  Tasks                 |  Should         
FRQ15  |  Users should be able to upload attachments related to their tasks                                                                                                                |  Tasks                 |  Should         
FRQ16  |  Tasks should automatically resolve to status “Complete” once all participants have completed them                                                                                |  Tasks                 |  Should         
FRQ17  |  Users should be able to edit their account details after signing up (email address, password);                                                                                   |  User                  |  Should         
FRQ18  |  Users should be able to delete their account                                                                                                                                     |  Settings              |  Should         
FRQ19  |  An individual can request help for a task from other task participants                                                                                                           |  Team                  |  Could          
FRQ20  |  Members may offer help to the rest of the team without a specific request being present, to speed up any potential rendez-vous (i.e. set a flag on that user for a given task)   |  Team                  |  Could          
FRQ21  |  Once a rendez-vous occurs, the parties involved should be able to chat with each other                                                                                           |  Team                  |  Could          
FRQ22  |  Team members should be notified via a mobile alert if a help request affects them                                                                                                |  Team                  |  Could          
FRQ23  |  Users could be able to upload a profile picture                                                                                                                                  |  User                  |  Could          
FRQ24  |  Users should be able to mute help request and help offer notifications                                                                                                           |  Settings              |  Could          

**Figure X: Functional Requirements**  

ID     |  Non-Functional Requirements                                                                                                                                                     |  Category     |  MoSCoW Priority
-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|-----------------
NFRQ1  |  A User who is not logged in is prevented from accessing the app                                                                                                                 |  Security     |  Must           
NFRQ2  |  The system shall store all user passwords in the blockchain in an encrypted format                                                                                              |  Security     |  Must           
NFRQ3  |  The app should provide offline sync, meaning that any state changes made during a period of no-connection should be saved and synced with the DB at the next possible instance  |  Persistence  |  Should         
NFRQ4  |  A team member should be able to signal a task as complete within a maximum of 1~2 UI interactions                                                                               |  Tasks        |  Should         
NFRQ5  |  The system should accomodate for at least 1,000 teams                                                                                                                           |  Capacity     |  Should         
NFRQ6  |  The system should accomodate for at least 100,000 tasks                                                                                                                         |  Capacity     |  Should         
NFRQ7  |  The system should log in a user within approximately 5 seconds                                                                                                                  |  UX           |  Should         
NFRQ8  |  The system shall respond to any interaction in no more than 10 seconds                                                                                                          |  UX           |  Should         

**Figure X: Non-Functional Requirements**


## 3.3 Use Cases
To remain realistic in regards to the allotted time for the project, use cases were aimed at providing a full exploration of the API rather than a rich user experience within the first iteration of the system, therefore ensuring that the API could remain agnostic regarding any particular client-side context.  
Use cases for the client-side application and the API itself were therefore straightforward, as the only actors within the initial scope of the system were the individual user and the user as part of a team, meaning there was no need for elevated permissions within the API or specialised UI interactions, as one might see with an administrative dashboard in other applications.

Use cases were constructed in parallel with the initial UI sketches, thus helping to visualise the flow of user-system interactions.  
**Figure xx** below represents an overview of the uses cases that were constructed, while the detailed use cases can be reviewed in **Appendix X**.  

ID    |  Use Case             |  Primary Actor  |  Secondary Actor
------|-----------------------|-----------------|-----------------
UC1   |  Signup               |  User           |  System         
UC2   |  Login                |  User           |  System         
UC3   |  AddTask              |  User           |  System         
UC4   |  ViewTasks            |  User           |  System         
UC5   |  TaskComment          |  User           |  System         
UC6   |  OptOutOfTask         |  User           |  System         
UC7   |  CheckTaskCompletion  |  User           |  System         
UC8   |  ExternalLinkToTask   |  User           |  System         
UC9   |  CreateTeam           |  User           |  System         
UC10  |  CheckTeamScore       |  User           |  System         
UC11  |  RequestHelp          |  User           |  System         
UC12  |  OfferHelp            |  User           |  System         
UC13  |  MuteNotifications    |  User           |  System         
UC14  |  EditAccount          |  User           |  System         
UC15  |  SetProfilePicture    |  User           |  System         
UC16  |  DeleteAccount        |  User           |  System         
UC17  |  AttachTaskFile       |  User           |  System         

**Figure X: Use Case Overview**

## 3.4 Sketches
To get a sense of a viable layout and structure for the client-side app, the author used rough sketches **(see Appendix X)** to visualise each view that was needed to meet the requirements. Once a convincing layout had been established for a view, the author created a static mockup in React's JSX, which could then be broken down into separate components and given dynamic properties, such as data retrieval methods, at a later point.

## 3.5 System Design
The design of the system's architecture started with creating a simple deployment diagram **(fig X)**, which provided a high-level view of the system's required components and the roles they would play.  
Establishing a high-level understanding of what the system required to provide proficient communication between any client application and the Tendermint blockchain was an essential aspect of the initial design phase. Decoupling the system's functionality from the specific implementation with which the system may be represented to a user was a crucial provision to ensure maximum reusability and compatibility of the system, therefore following the common software engineering mantra of _"program to an interface, not an implementation"_<sup>[(REF)]()</sup>. Within the context of the web, the most appropriate form to follow this approach was by developing a RESTful<sup>[(REF)]()</sup> API, thus providing a uniform interface of data endpoints, regardless of the client requesting said data.  

![deployment diagram](../diagrams/deploymentDiagram.png)
_Figure x_


### 3.5.1 Smart Contract Analysis
The role of the smart contracts played in this project – rather than attempting to literally track and enforce contractual obligations between multiple parties – was more closely related to that of typical classes in object-oriented programming _[(REF?)]()_, with the majority of the contracts either acting as factories<sup>[(factory pattern; find better source)](http://www.oodesign.com/factory-pattern.html)</sup> for composite types or implementing operations on these types as "manager" contracts.

#### Factory Contracts
In order to define composite types, known in Solidity as `struct`s, a contract was defined for each domain of the system as revealed by the requirements. This meant there was a need for `User`, `Team` and `Task` contracts.


#### Data Structure Contract
Solidity – as a young and constantly developing language for smart contracts – does not currently provide a way to iterate over storage arrays. The language's version of objects, the `mapping` type, which approximates what is usually known as a hash map, is also non-iterable. This meant a custom data structure contract was needed to store, edit and retrieve collections of type contracts.


#### Manager Contracts
To avoid violating the Single Responsibility Principle<sup>[(SRP pattern; find better source)](http://www.oodesign.com/single-responsibility-principle.html)</sup> of object-oriented design, there was a need for a set of contracts which would perform operations on instances returned by the factory contracts. This meant there was a need for a `UserManager`, `TeamManager` and `TaskManager` contract.


#### Linker Contract
The previously discussed decision to encapsulate all data generated by the system within the blockchain led to an issue of data relations amongst the contracts: How could a new `Task` contract be linked to the appropriate `User` contract if there was no structured way to do so in comparison to, for example, an SQL interface? This meant there was a need for a higher-order `Linker` contract, dedicated to locating and linking related contracts in such a situation.


![Simplified Contracts Class Diagram](../diagrams/simplifiedContractsClassDiagram.png)
Simplified class diagram to illustrate the relationships between Factory, Manager and Linker contracts.

#### Action Events
Solidity's lack of any real logging capabilities meant there needed to be consideration in regard to how function calls within contracts could be observed from the outside. Solidity does, fortunately, provide an `event` type, which opened up the possibility of triggering such an `event` where required and listening for it server-side, where logging the events was a trivial matter.


### 3.5.2 Server-side Analysis
#### API Router
Once the structure of the smart contracts had been established, the analysis of the server-side implementation quickly revealed that the server's structure would largely mirror that of the smart contracts. This was the case partially due to the way Eris's JavaScript library was implemented, but largely due to the fact that this would keep the the final API succinct and free of unnecessary cognitive load for the author and for any future developers.  
The server would therefore simply act as a relay and transformer for data travelling between the client-side application and the blockchain, acting as a _de facto_ middleman. In more concrete terms, this would involve calling relevant contract methods on the blockchain when a certain API endpoint was requested and transforming data between hexadecimal and UTF-8, for example. This is necessary due to Solidity's poor support for strings at the time of writing, meaning that all strings would have to be encoded into 32-byte fields of hexadecimal, known in Solidity as the `bytes32` type.

#### Uploader
Meeting the requirement of letting task participants attach files related to the their tasks was trickier than anticipated, as choosing to develop the system's client-side as a mobile application came with a key drawback: file management. Mobile operating systems provide only limited capabilities to edit and manage even simple text files, meaning that the platform was suboptimal for attaching files related to a task. For example, if the task involves writing a report, task participants may want to attach the final report to the task in the blockchain, thus further corroborating their participation in it.  

To resolve this usability bottleneck, the author decided to utilise the server not just as a headless API router, but also as an uploader, composed of a single web page, to attach files to tasks present in the blockchain. To implement the uploader effectively, two key aspects had to be considered:

_UX consistency_ - From a usability perspective, the transition between the mobile app interface and the browser-based uploader should be as seamless as possible. To avoid disorienting the user, the uploader was therefore kept as straightforward as possible; a single page with a form and a submit button.

_Security_ - The user would have to enter a token – issued by the mobile app when a new task is created – to confirm that the upload has a legitimate task associated to it. The use of tokens therefore prevents automated spam and enables the identification of users uploading material which may be malicious or illegal in nature.


### 3.5.3 Client-side Analysis
#### Abstracting view components with React
React takes markedly different approach to view templating compared to the other major web development frameworks such as Google's AngularJS<sup>[(AngularJS)](https://angularjs.org/)</sup>. React's philosophy focuses on breaking a view down into its constituent parts to form reusable components. These components can then be composed in whichever way the developer sees fit. This level of composability comes to its full potential in React Native, as it enables the abstraction of common UI components within a mobile app, providing ample opportunity to even reuse said components across different platforms. The decision to abstract a given component was therefore made according to the following criteria, ordered by highest weighting first:

_Frequency & variability of use_ - How sensible it was to abstract a specific component into a more generic one heavily depended on the range of use cases it could be applied to. For example, creating a generic navigation bar component which could contain different buttons for different views made sense, whereas creating an abstract component for the user's profile summary did not, since it would only be used a single time, in a single place within the app.

_Cross-platform potential_ - A secondary consideration was whether the abstraction would be useful across platforms, i.e. between iOS and Android. For the initial iteration of QuantiTeam which would only focus on iOS this aspect was less significant than the frequency of use, but it played a significant role in keeping the client-side application open to extension at a later point nonetheless.

#### Managing state with Redux
While considering the responsibilities that a client-side implementation would have to fulfill to meet the established requirements, it also became clear that even for a simple implementation there was a considerable amount of application state that would have to be managed by the client. Besides being the most common choice within React Native applications, the Redux library provides a well-structured approach to state management, derived from the philosophy behind the Elm programming language. Redux ensures that events which mutate state are well-defined and that they may only take place in a single direction (unidirectionally).  
In concrete terms, Redux achieves these orderly state mutations by following three interdependent principles<sup>[(three principles)](http://redux.js.org/docs/introduction/ThreePrinciples.html)</sup>:

_Single source of truth_ - All of the application's state is stored in a single object tree known as the `store`.

_State is read-only_ - Actions, which are objects describing a possible state mutation, are the only way to modify the store and are therefore its only source of information.

_Mutation through pure functions_ - Reducers, a type of pure function, are used to take the application's current state object in conjunction with an action describing a state mutation, to return a new state object.

Based on these 3 principles which impose the need for a `store` object tree, actions and reducers, state mutations within Redux can be visualised in the following manner:

![Redux Behaviour](../diagrams/redux.png)  
Source: http://www.ibm.com/developerworks/library/wa-manage-state-with-redux-p1-david-geary/
