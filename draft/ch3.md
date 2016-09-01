# Chapter 3
# Requirements and Analysis

## 3.1 Problem Statement
**TODO: reiterate from Ch.1?**

![Concept Diagram](../diagrams/conceptDiagram.png)

## 3.2 Requirements
In a typical software engineering project, there is a need for strong levels of communication between all the stakeholders involved in the endeavour, in order to ensure all the needs the software should meet are in fact met _[(REF)]()_. A frequent phenomenon within these interactions is that requirements are revealed to the development team in a haphazard way or are simply misinterpreted by the team _[(REF?)]()_.  
Fortunately these common pitfalls were a moot point within this project, as the act of establishing requirements was entirely focused around the question: "What functionality does the system require, at a minimum, to fulfill its stated aims?". This provided a clear focus on what was absolutely needed for a minimum viable product (henceforth MVP)<sup>[(MVP)](https://www.techopedia.com/definition/27809/minimum-viable-product-mvp)</sup> that an individual and/or group of individuals could use in a meaningful way. This meant both functional and non-functional requirements were focused around three domains: tasks, users, and teams. The requirements were prioritised according to the MoSCoW system<sup>[(MoSCoW)](https://www.dsdm.org/content/moscow-prioritisation)</sup>, with requirements being ranked from "Must Have" through "Should Have" and "Could Have", with the final category being "Won't Have (in this development cycle)". The project's "Must Have" requirements had to be strictly limited to what was  as there was a large number of known unknowns (e.g. the Solidity language) and unknown unknowns (unforeseeable issues with the API, blockchain or development environment). The "Should Have" and "Could Have" categories therefore largely express targets for more sophisticated future iterations of the system.  

**TODO: include functional & non-functional requirements tables**


## 3.3 Use Cases
To remain realistic in regards to the allotted time for the project, use cases were aimed at providing a full exploration of the API rather than a rich user experience within the first iteration of the system, therefore ensuring that the API could remain agnostic regarding any particular client-side context.  
Use cases for the client-side application and the API itself were therefore straightforward, as the only actors within the initial scope of the system were the individual user and the user as part of a team, meaning there was no need for elevated permissions within the API or specialised UI interactions, as one might see with an administrative dashboard in other applications.

Use cases were constructed in parallel with the initial UI sketches, thus helping to visualise the flow of user-system interactions.  
**Figure xx** below represents an overview of the uses cases that were constructed, while the detailed use cases can be reviewed in **Appendix X**.  

**TODO ref for use case template, Arlow & Neustedt**  
**TODO more use cases, i.e. file uploads etc**

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


## 3.4 Sketches & Wireframes
**TODO**

## 3.5 System Design
The design of the system's architecture started with creating a simple deployment diagram **(fig X)**, which provided a high-level view of the system's required components and the roles they would play.  
Establishing a high-level understanding of what the system required to provide proficient communication between any client application and the Tendermint blockchain was an essential aspect of the initial design phase. Decoupling the system's functionality from the specific implementation with which the system may be represented to a user was a crucial provision to ensure maximum reusability and compatibility of the system, therefore following the common software engineering mantra of _"program to an interface, not an implementation"_<sup>[(REF)]()</sup>. Within the context of the web, the most appropriate form to follow this approach was by developing a RESTful<sup>[(REF)]()</sup> API, thus providing a uniform interface of data endpoints, regardless of the client requesting said data.  

![deployment diagram](../diagrams/deploymentDiagram.png)
_Figure x_


### 3.5.1 Consensus Algorithm Analysis
**TODO**

### 3.5.2 Smart Contract Analysis
The concept of a smart contract is usually evoked in terms of a digital protocol which enforces and/or verifies the performance of a contract between two or more parties<sup>[(smart_contracts_idea)](http://szabo.best.vwh.net/smart_contracts_idea.html)</sup>. Within the context of this project the role of the smart contracts played was more closely related to that of typical classes in object-oriented programming _[(REF?)]()_, with the majority of the contracts either acting as factories<sup>[(factory pattern)](http://www.oodesign.com/factory-pattern.html)</sup> for composite types or implementing operations on these types as "manager" contracts.

#### Factory Contracts
In order to define composite types, known in Solidity as `struct`s, a contract was defined for each domain of the system as revealed by the requirements. This meant there was a need for `User`, `Team` and `Task` contracts.


#### Data Structure Contract
Solidity – as a young and constantly developing language for smart contracts – does not currently provide a way to iterate over storage arrays. The language's version of objects, the `mapping` type, which approximates what is usually known as a hash map, is also non-iterable. This meant a custom data structure contract was needed to store, edit and retrieve collections of type contracts.


#### Manager Contracts
To avoid violating the Single Responsibility Principle<sup>[(SRP pattern)](http://www.oodesign.com/single-responsibility-principle.html)</sup> of object-oriented design, there was a need for a set of contracts which would perform operations on instances returned by the factory contracts. This meant there was a need for a `UserManager`, `TeamManager` and `TaskManager` contract.


#### Linker Contract
The previously discussed decision to encapsulate all data generated by the system within the blockchain led to an issue of data relations amongst the contracts: How could a new `Task` contract be linked to the appropriate `User` contract if there was no structured way to do so in comparison to, for example, an SQL interface? This meant there was a need for a higher-order `Linker` contract, dedicated to locating and linking related contracts in such a situation.


![Contracts Class Diagram](../diagrams/contractsClassDiagram.png)
Simplified class diagram to illustrate the relationships between Factory, Manager and Linker contracts.

#### Action Events
Solidity's lack of any real logging capabilities meant there needed to be consideration in regard to how function calls within contracts could be observed from the outside. Solidity does, fortunately, provide an `event` type, which opened up the possibility of triggering such an `event` where required and listening for it server-side, where logging the events was a trivial matter.

**-/BEGIN relevant?/-**  
The diagram below shows how smart contract actions could be structured. Within this initial iteration of the project, contracts like the `UserManager` were themselves responsible for dispatching action events related to their methods, as the level of abstraction shown would only become useful once the API grew to a larger scale, enabling actions to be centrally processed by an `ActionManager` and then cascading to their relevant Manager contracts.

![Action-driven smart contract architecture](../diagrams/actionEventArchitecture.png)  
Source: https://docs.erisindustries.com/tutorials/solidity/solidity-2/

**-/END relevant?/-**  


### 3.5.3 Server-side Analysis
Once the structure of the smart contracts had been established, the analysis of the server-side implementation quickly revealed that the server's structure would largely mirror that of the smart contracts. This was the case partially due to the way Eris's JavaScript library was implemented, but largely due to the fact that this would keep the the final API succinct and free of unnecessary cognitive load for the author and for any future developers.  
The server would therefore simply act as a relay and transformer for data travelling between the client-side application and the blockchain, acting as a _de facto_ middleman. In more concrete terms, this would involve calling relevant contract methods on the blockchain when a certain API endpoint was requested and transforming data between hexadecimal and UTF-8, for example. This is necessary due to Solidity's poor support for strings at the time of writing, meaning that all strings would have to be encoded into 32-byte fields of hexadecimal, known in Solidity as the `bytes32` type.

**TODO quick blurb about how it needed to double as a simple file upload**
**TODO 2-3 sequence diagrams**


### 3.5.4 Client-side Analysis
#### Abstracting view components with React
**TODO**
- React champions approach of component-based views to achieve DRY code with maximum reusability
- `common`
    - `Header` as example of cross-platform reusable abstraction
    - Globals (GlobalStyles); RN uses component-based hyper-modular CSS, GlobalStyles provided nice way to mix in common styling (e.g. view margins)

#### Managing state with Redux
While considering the responsibilities a client-side implementation would have to fulfill to meet the established requirements, it also became clear that even for a simple implementation there was a considerable amount of application state that would have to be managed by the client. Besides being the most common choice within React Native applications, the Redux library provides a well-structured approach to state management, derived from the philosophy behind the Elm programming language. Redux ensures that events which mutate state are well-defined and that they may only take place in a single direction (unidirectionally).  
In concrete terms, Redux achieves these orderly state mutations by following three interdependent principles<sup>[(three principles)](http://redux.js.org/docs/introduction/ThreePrinciples.html)</sup>:

_Single source of truth_ - All of the application's state is stored in a single object tree known as the `store`

_State is read-only_ - Actions, which are objects describing a possible state mutation, are the only way to modify the store and are therefore its only source of information

_Mutation through pure functions_ - Reducers, a type of pure function, are used to take the application's current state object in conjunction with an action describing a state mutation, to return a new state object

Based on these 3 principles which impose the need for a `store` object tree, actions and reducers, state mutations within Redux can be visualised in the following manner:

![Redux Behaviour](../diagrams/redux.png)  
Source: http://www.ibm.com/developerworks/library/wa-manage-state-with-redux-p1-david-geary/


To apply the Redux philosophy to QuantiTeam, actions and reducers were modularised into a schema which follows the system's data domain (users, teams, and tasks) as shown below:

```
...
├── reducers
│   ├── rootReducer.js
│   ├── tasks.js
│   ├── team.js
│   └── user.js
...
```

Splitting reducers in this manner ensured that the reasoning involved in managing the client application's state could be broken down into its constituent parts, providing minimal cognitive load when processing data coming from the system's API.

- Flow made specifying actionTypes even more effective and stringent
