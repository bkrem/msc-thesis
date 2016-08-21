# QuantiTeam Report Outline Draft

- Cover page
- Abstract
- Acknowledgements
- List of figures
- Abbreviations  

## Introduction (~2-4 pages)
- Problem Outline
    - Personal frustration with non-engaging educational contexts as a youth
    - Interest in why students drop out and how this could be prevented
    - Investigated student engagement via social peers (SocialX, Wikispaces etc)
    - Realisation that A) there is no way to verify your school achievements/completed tasks in general (-> no incentive to commit) B) there is no way to demonstrate one's participation in work as a team, which is not only socially rewarding/engaging, but also provides a critical professional skill which is of interest to employers
- Goals/Scope
    - Blurb
        - Two targets: broadly, provide technical groundwork for the use of blockchains as ledgers for social activity/action. Concretely, aiming to provide high school/university students an opportunity to re-engage with their work in a communal way and focus on a target ahead of them.
    - Specifics:
        - Working public API interface & file upload for blockchain via Node server
        - iOS client-side app for basic task/user/team management
        - Basic deployment of distributed validator nodes
- Personal Aims
    - Learn how to write smart contracts (Solidity)
    - Gain hands-on familiarity with blockchain technology (in this case proof-of-stake), and how a chain is set up, run & maintained proficiently
    - Experiment with native dev via React Native after using React and Cordova heavily previously, to provide the simplest but powerful UX possible for a complex backend

- Approach to Project/Project Management/Project Overview
    - **UP design** (include a figure)
    - MoSCoW, based on MVP use cases from a client-side perspective
    - Importance of API testing (Testing against an API not an implementation)(Writing an interface not an implementation)

<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
    - Project required an initial proof-of-concept regarding the API interface and whether the whole thing was even doable
    - Proof-of-concept with the chain running on single node on localhost
    - Experimentation with low-level features of chain & solidity via cURL requests
    - Static mockup of the React Native components initially, linked up with a Redux store once API bridge was established
    - Familiarity with React/JSX & Node as my primary languages and tools in previous projects allowed me to plan everything bar the blockchain itself carefully beforehand, minimising risk of critical failure at a late stage from the waterfall model, to a subsection of the system.
    - Following the PoC, it made more sense to gradually scale each domain of the system (tasks -> users -> teams), as having users without the ability to link tasks to them (i.e. do anything with them) is pretty pointless, having teams before users even more so.
<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
- Report Overview

## Background Information & Related Applications/Work (~8-10 pages)
- Blockchains & Dropouts (blurb)
- Existing applications
    - SocialX
    - WikiSpaces
    - HaikuLearning
    - **TODO 1-2 more**

- Basis of design
    - Talk about the SocialX paper as a major influence on this work
    - Blockchains as commonplace novel application for financial auditing/smart contracts
    - Taking blockchains and smart contracts beyond finance: verifiability & incorruptibility of team work metadata
    - Possible application domain: Create opportunity for open-source version of HaikuLearning or WikiSpaces
- Background Research (investigation) - Software engineering methodologies and technology stack
    - _(((Agile, Scrum, Kanban (justify why I ended up with simple Kanban board) )))_
    - Blockchain:
        - OpenChain (contract chaining, but documentation/implementation sucked) vs MultiChain vs Eris+TenderMint
        - Smart contract design & Solidity's capabilities (Eris Tutorials, Solidity docs)
    - Databases: SQL vs NoSQL vs Blockchain
        - Ease of data structuring/scalability vs. no central point of failure but less storage efficiency and more complex integrity maintenance compared to SQL. Favourable in comparison to noSQL?
    - Server-side:
        - Express as useful abstraction from raw Node server+routing
        - Functional principle from GC16: stateless server architecture
    - Client-side:
        - React Native vs WebView implementations (i.e. Cordova), React: DRY, modularity, encapsulation, composable
        - MVC vs Unidirectional dataflow: Advantages of Redux, single source of truth, easy to reason
    - _(((Redux vs Flow vs Reflux etc.)))_
    - Smart components vs Dumb components
- Existing Related Applications
- Programming Languages/Technologies/Libraries/Software Tools Selected
    - React Native (incorporates HTML5, CSS/Flexbox, AsyncStorage(?)
    - Redux
    - Node
    - Docker/Docker-Machine
    - Kanban board (GH issues + waffle.io)
    - Testing frameworks: Mocha, Chai (TDD with `assert`), Istanbul (code coverage)
    - Dev tools: Version control (Git), Code hosting (Github), Editor (Atom+Nuclide+ESLint), Flow (ES6 static type analyser), `envsetup.sh` & `simplechain.sh` scripts
    - Debugging: Redux logger+Chrome console, Log4JS+erisLogger, raw logs from the TenderMint chain

## Requirements & Analysis (~5-6 pages)
- Problem Statement
- Requirements/Requirements Gathering/Functional requirements vs Non-functional requirements
- Use Cases
- Analysis & Data Modelling
    - Initial Deployment diagram
    - Development of SQL/noSQL-free data structure with blockchain as verifier and store simultaneously (add updated deployment diagram)
    - PK/FK simulation via the `linker` and on-chain addresses
    - UML class diagrams
    - Blockchain analysis:
        - _(((ERD)))_
        - Docker/Docker-Machine
        - Data structure/model
            - SequenceList contract
            - Type contracts
            - Manager contracts
            - Linker contract
    - Pipeline server analysis:
        - data relay/transformer
        - (almost?) stateless
        - 2-3 sequence diagrams
    - Client side data flow and structure
        - UML state diagrams: perfect for Redux
        - Redux store
        - Unidirectional data flow (dispatcher, stores, actions etc.)

## Design & Implementation (~10+ pages)
- Source code structure
    - `chain`
        - Solidity contracts
        - `Eris` configs for deployment
        - Node server as pipeline
    - `app`
        - Example implementation in React Native
- Client side architecture
    - `Flow` static typing
    - UI: React components: template design (abstraction with `common` components), component lifecycle, composing in React
    - Client side data: Redux actions, stores, reducers, `types.js`
    - `Navigator`
    - `actions/util.js`
    - `TabsView`
- Server side architecture
    - `server` - server startup, endpoint resolution & route handling
    - `auth` - user signup/login authentication
    - API
        - `/tasks/`
        - `/user/`
        - `/new-id/` (comes back to serverside `util`)
        - `/team/` (?)
    - `taskManager`/`userManager` modules; what they do and why like this
    - `linker`
    - Library modules
        - `eris-logger` - better stack traces, clearer control flow post-mortems
        - `eris-wrapper` - significantly abstracts fiddly low-level setup for the eris-blockchain JS lib

## Testing (~2-4 pages)
**TODO complete me**
- Testing phases/strategy
- Unit Testing
- API endpoint testing (?)
- Testing libraries/frameworks: mocha, chai `assert`, istanbul

### Deployment
- MVP 4-validator-node deployment to DigitalOcean

### Results
- Launch
- Feedback

## Conclusions & Evaluation (~2-4 pages)
- Achievement of Goals/Aims/Fulfilment aims
- Critical Evaluation
    - Technology
    - Object Oriented Design
    - User Interface Heuristics
- Future Work/Further Iterations
- Final thoughts

## Bibliography

## Appendices
- High-Fi Sketches (given)
- List of Requirements
- Use Cases
- User Stories
- System Manual (README from Github)
- User Manual (derive from Help/FAQ page)
- Code Examples/Snippets

## Glossary
