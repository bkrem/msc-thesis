# Chapter 2
# Background Information and Related Applications

## 2.1 Blockchains & Dropouts
(__intro is suboptimal__)  
A 2004 study by the National Research Council showed that upwards of 40% of high school students feel disengaged from learning and exert little effort on school work [(Center on Education Policy 2012)](http://cep-dc.org/displayDocument.cfm?DocumentID=405 ). More worryingly still, 70% of dropouts stated lack of motivation, in a 2006 study, as the key reason for their departure from school [(Bridgeland, J.M. et al., 2006. The Silent Epidemic: Perspectives of High School Dropouts)](). In liaison with these findings, a study which tracked the educational careers of a group of individuals found that 23% of those who dropped out cited a sense of not belonging as the reason for their departure from school [(Berktold et al., 1998, Table 6)]().  

While the factors for a student's motivation (or lack of it) regarding their education may stem from a myriad of factors, engaging the individual on a social level may thus be posited as a key driver in motivating them to remain engaged in the process [((Johnson et al., 2001; Newmann, Wehlage, and Lamborn, 1992; Tinto, 1993; Wehlage, Rutter, Smith, Lesko, and Fernandez, 1989))](). Of course, simply being bound into a social context does not mean that the student will make meaningful progress regarding their work, as attendance does not constitute attention to what is being taught [(_ibid_)]().  

Whereas there is little school staff can do to ensure the attention of a student within a physical school, supplementing the educational process with blockchain-based technology may provide a solution to this impasse between the individual student's aims and that of the educator. By establishing a system in which collaborative work is directly linked to both an individual and social reward, students are able to focus on a more immediate & manageable target than an abstract and potentially far-off seeming concept of a high school diploma.

> “Rewarding specific actions that students can control, such as completing homework, yields better results than rewarding accomplishments that may seem beyond their reach or out of their control” [(Usher, Kober; Student Motivation - An overlooked piece of school reform)]() **TODO confirm this quote, is this the full sentence?**

Using a blockchain as a data structure – in this case one which is built on a proof-of-stake consensus algorithm – to register, manage and save school-related educational tasks, along with a reward & ranking scheme linked to the blockchain, would enable students to reap immediate rewards from their individual & collaborative efforts. Furthermore, a blockchain's ability to as a pseudo-anonymous public ledger of events, in this case school-related tasks, would provide students with the ability to refer back to said tasks at a later point in time to demonstrate their teamwork skills, therefore making a previous scarcely confirmable skill set auditable.

## 2.2 Previous Work/Existing Applications
This section gives an overview of related work within the context of heightening student engagement described above.
- **(review this, maybe mention how there doesn't seem to be a blockchain-based collab tool => none of the related work has data security via distributed network)**
- **closed vs open stance, i.e. licensing?**

#### SocialX<sup>[(Learning from peers: motivating students through reputation systems )](#needsLink), [(Collaborative projects and self evaluation within a social reputation-based exercise-sharing system )](#needsLink)</sup>
SocialX is an exercise sharing tool which enables students to earn reputation points, which are visible to their peers and the given subject's teacher, by submitting solutions to exercises. A further source of reputation are endorsements, which a student may receive from their peers if they were inspired to reuse the student's solution. SocialX differs to QuantiTeam in three key ways: firstly, reputation within SocialX is calculated according to factors which involve the judgment of each other's work, whereas in QuantiTeam's reputation is a function of team size and is either rewarded in full to all team members or not at all, depending on whether the task/exercise in question was completed. Secondly, QuantiTeam has no teacher role, and is therefore able to avoid a bottleneck SocialX suffers from. Thirdly, SocialX defines the term "global ranking" as a measurement of student reputation across subjects/courses, whereas QuantiTeam strives for a truly global, decentralised ranking system.

#### WikiSpaces<sup>[(WikiSpaces)](https://www.wikispaces.com/content/classroom/about)</sup>
**TODO**

#### HaikuLearning<sup>[(HaikuLearning)](https://www.haikulearning.com/)</sup>
**TODO**

## 2.3 Programming Languages and Libraries
### 2.3.1 Blockchain
**Definition of what a blockchain & smart contracts even are?**

At the outset of the project, a key choice which had to be made was the choice of blockchain implementation to be used. This decision was critical as discovering a major flaw in the implementation, or an incompatibility between what the implementation could provide and what was needed, would have resulted in deadlock for the entire project.  
The author's initial experimentation with OpenChain<sup>[(OpenChain)](https://www.openchain.org/)</sup>, which seemed to offer the possibly useful ability to chain contracts (i.e. tasks) to each other, was quickly abandoned as the available documentation was insufficiently detailed and partially ambiguous. This would have left the project open to a lot of "stabbing in the dark"**(<--rephrase)** and thus the above mentioned risk of deadlock.  
MultiChain<sup>[(MultiChain)](http://www.multichain.com/)</sup>, on the other hand, was an implementation which had well-defined documentation, but it assumed a level of pre-existing familiarity with the API of a blockchain, as well as providing preciously little context as to how smart contracts could be developed for the platform in an effective way.  
The apparent drawbacks of the two mentioned blockchain implementations therefore led the author to the Eris<sup>[(Eris)](https://erisindustries.com/)</sup> platform. Eris itself provides a suite of tools which wrap and augment a Tendermint<sup>[(Tendermint)](http://tendermint.com/)</sup> blockchain implementation. Eris's wealth of documentation and its well-structured command line interface (henceforth CLI) were the deciding factor in it being the blockchain implementation of choice. The documentation provided not only step-by-step examples of how a developer could configure and deploy a blockchain, but also provided in-depth tutorials and examples on how to write & deploy Solidity<sup>[(Solidity)](http://solidity.readthedocs.io/en/latest/)</sup> smart contracts for the platform, highly valuable for a smart contract novice such as the author. Furthermore, the fact that Tendermint is an entirely separate software project meant that there was also an entire repository of documentation for this specific type of blockchain and the computer science theory backing it.  

**mention that Solidity was basically the only contract lang choice?**

### 2.3.2 Server-side
Considering the author's background as a JavaScript developer, the natural choice for a web server for the project was NodeJS<sup>[(NodeJS)](https://nodejs.org/en/)</sup>. NodeJS provides a great level of flexibility and extensibility thanks to its huge ecosystem of open-source libraries available through its package manager, NPM<sup>[(npm)](https://www.npmjs.com/)</sup>, thus providing an essential basis to help mitigate the possibly complicated technical details of interfacing with a low-level blockchain.  
As JavaScript is an ever-evolving language, a choice had to be made as to whether the commonly supported ECMAScript<sup>[(ECMAScript)](http://www.ecma-international.org/)</sup> 5 (ES5) specification should be used or the modern standard, ES6. ES5 was selected to provide the maximum level of compatibility for wherever the server is being deployed, as native support for ES6 is not wide-spread as of the point of writing and transpiling the server's code from ES6 to ES5 for each deployment would have been a lot more trouble than help for a regular web server.  

The server-side implementation makes use of a handful of key libraries:

#### Express
Express<sup>[(Express)](https://expressjs.com/)</sup> is a minimalist framework for NodeJS web servers which provides a powerful layer of abstraction on top of NodeJS's raw server interface. Express helped accelerate the development of the API, while leaving the possibility of fine-tuning the NodeJS server itself intact.

#### eris-wrapper & eris-logger
The eris-wrapper & eris-logger modules were adopted from a "Hello World"-style example Eris provides to showcase how a NodeJS server implementing their platform could be structured<sup>[(Hello Eris)](https://github.com/eris-ltd/hello-eris)</sup>.  
eris-wrapper provides a convenient abstraction of low-level bindings that have to take place between the NodeJS server and the Tendermint blockchain, while eris-logger simply sets up a simple logger with different levels of urgency, such as `ERROR`, `INFO` and `DEBUG`.  


#### Async
Async<sup>[(Async)](https://caolan.github.io/async/)</sup> is a utility library which helps manage the flow of asynchronous functions, which are a common occurrence in NodeJS. Within the scope of the server, Async was primarily used to chain together multiple sequential interactions with the blockchain.

#### bcrypt
**TODO (?)**

### 2.3.3 Client-side
During the MSc's GC02 "App Design" course, the author and his team had the opportunity to build a React web application which was also required to run as a mobile application. This was achieved by wrapping the web app with the Apache Cordova<sup>[(Cordova)](https://cordova.apache.org/)</sup> mobile development framework. This experience revealed two key insights regarding the intersection between web- and mobile-based client-side development:  
Firstly, that Facebook's React<sup>[(React)](https://facebook.github.io/react/)</sup> web development library, in liaison with Dan Abramov's Redux<sup>[(Redux)](http://redux.js.org/)</sup> library for data management, could be applied almost seamlessly to the context of a mobile application. React's philosophy of thinking in terms of individual view components, together with Redux's implementation-agnostic approach to managing the application's state, largely abstracted away the conceptual differences between building an HTML document and building a mobile app view.  
Secondly, that porting a web app to a mobile app simply does not provide an authentic native experience on a mobile device, as it is almost impossible to account for and implement all the differences and individual nuances in, for example, animation styles between mobile operating systems. This resulted in the look & feel of the application being closer to that of a mobile browser rather than a native mobile app.

Based on these two lessons learnt, the decision was made to use Facebook's React Native<sup>[(React Native)](https://facebook.github.io/react-native/)</sup> for the project, which grafts a truly native mobile development framework on top of the outstanding React library, along with the now familiar Redux library.  
React Native offers all the benefits of React (high levels of modularity & encapsulation, composability) while allowing the developer to hook into events and effects of the native mobile operating system. This means that transitioning between views smoothly or sending a push notification, for example, becomes a trivial undertaking.  
Redux provides a well-defined interface to manage the state of the application's data by managing all of it through a centralised `store` object, the contents of which can be altered exclusively by a set of actions previously defined by the developer. This provides a level of clarity of how the application's state mutates over time that is hard to achieve in a traditional Model-View-Controller (henceforth MVC) approach, in which many controllers have access to the application's state simultaneously.

Further libraries which play a key role within the client-side application are the following:

#### redux-thunk
redux-thunk<sup>[(redux-thunk)](https://github.com/gaearon/redux-thunk)</sup> is a helper library which enables the use of asynchronous functions as the aforementioned actions, which affect the application's state. This is essential to allow control over how and when the client application sends data to the blockchain, and how it fetches data from the blockchain to then integrates it into its local state.

#### redux-persist (?)
**TODO (?)**

#### redux-logger
redux-logger<sup>[(redux-logger)](https://github.com/evgenyrodionov/redux-logger)</sup> supplements Redux itself by automatically logging actions the state changes they trigger to the development console. This meant a significant amount of time could be saved in avoiding a custom implementation of a logger, or worse yet, littering the codebase with sporadic logging statements.

#### tcomb-form-native
tcomb-form-native<sup>[(tcomb-form-native)](https://github.com/gcanti/tcomb-form-native)</sup> provides a framework for forms and form validation in React Native. As form validation in particular can take up an inordinate amount of development time, this library was essential in order to keep the project on track given the severe time constraints relation to its size.


### 2.3.4 Testing
Composing unit tests for each public function of the system's API was an essential part of the development process, especially as the Tendermint blockchain was able to report errors within its own processes to the NodeJS server only in the most rudimentary of terms. This meant that if no testing suite was in place, silent breakages and undefined behaviour were likely to occur as the API grew.  

The following JavaScript libraries were used to implement a suite of unit tests on the server:  

#### Mocha
Mocha<sup>[(Mocha)](https://mochajs.org/)</sup> was chosen as the testing framework for the project due to its specific aim of simplifying the testing of asynchronous JavaScript code. This clearly applied to the needs of the planned API which would be largely built on asynchronous functions.

#### Chai
Chai<sup>[(Chai)](http://chaijs.com/)</sup> is an assertion library for both test-driven development (TDD) and behaviour-driven development (BDD). More specifically, Chai's TDD `assert` function was used to keep test assertions simple and testable against an expected result.

#### Istanbul
Istanbul<sup>[(Istanbul)](https://gotwarlost.github.io/istanbul/)</sup> is a code coverage tool which was used to gain an overview of how much of the API was currently covered by tests. This occurred via coverage reports, which were generated after every run of the test suite.


### 2.3.5 Databases: SQL vs NoSQL vs Blockchain -/location?/-
Using a blockchain as the pivotal data structure backing the system evoked an interesting question: could the entire system be built in a way that did not, at any point, rely on a traditional database to store data? This would automatically provide the ability to have a distributed database, assuming there are is more than one full-index blockchain node, avoiding any central point of failure and thus the potential for catastrophic data loss. Furthermore, this approach would be the significantly more cohesive one in terms of system architecture. Adding a further external data source, to manage auxiliary data which didn't neatly fit into the task management duties of the blockchain's smart contracts, would have introduced a further agent within the planned API, thus significantly increasing the complexity of data retrieval and storage operations.  

Yet, this approach could also have a significant impact on the size efficiency **(?)** and integrity of the stored data. Unlike with a typical SQL or noSQL database, there is – to my knowledge **(review this)** – no commonly accepted approach as to how data should be structured in terms of smart contracts for persistent storage in a Tendermint blockchain. This meant an initial attempt at implementing an interface to do so would possibly contain inefficiencies and anti-patterns in terms of data relations.  

After weighing up the above-mentioned points, the choice was made to go ahead and attempt to store all data generated by the system within the blockchain, as choosing to add a further form of database would likely have been significantly more bug-prone and costly in terms of development time.


## 2.4 Tooling
### 2.4.1 Requirements and Design Tools
GanttPro<sup>[(GanttPro)](https://ganttpro.com/)</sup>, an online creator and editor for Gantt charts, was used to create a timeline for the project, while Google Docs<sup>[(GoogleDocs)](https://www.google.com/docs/about/)</sup> in conjunction with draw.io<sup>[(draw.io)](https://www.draw.io/)</sup> were used to capture requirements and create UML diagrams.  
This document was written in markdown and transpiled to Latex format using pandoc<sup>[(pandoc)](http://pandoc.org/)</sup>.


### 2.4.2 Development Tools
#### Editor
In order to avoid switching between editors to get the best development support for disparate parts of the project's code, Github's Atom<sup>[(Atom)](https://atom.io/)</sup> editor was chosen due to its level of customisability and huge selection of plugins. Atom was particularly suited for writing React Native code thanks to Facebook's Nuclide<sup>[(Nuclide)](https://nuclide.io/)</sup> plugin, which enables Atom to approximate the richness of features that is typically only offered by Integrated Development Environments (IDEs) by offering an in-built debugger, code snippets, and Facebook's own static type analyser which is discussed below.

#### Static Type Analyser
Flowtype<sup>[(Flowtype)](https://www.flowtype.org/)</sup>, a static type analyser for JavaScript, is a further in-built feature of the Nuclide plugin. Flowtype enables the developer to define a type for a variable, a type signature for a function, and even to create custom union & intersection types. Flowtype then checks whether the defined type specifications are adhered to and warns if it detects a TypeError. This is an incredibly useful tool for a dynamically-typed language such as JavaScript, where unintended type coercion is a common ailment.

#### Linters
**Define term "linter"?**  
ESLint<sup>[(ESLint)](http://eslint.org/)</sup> was chosen as the linter for both the React Native app and the NodeJS server, due to its support for detecting semantic & stylistic errors in both regular JavaScript and JSX, a mix of JavaScript and HTML used by Facebook for React.  
For the smart contracts, written in Solidity, the `solidity-linter`<sup>[(solidity-linter)](https://atom.io/packages/linter-solidity)</sup> Atom plugin was used, which avoided a significant amount of frustration in comparison to waiting for errors to be detected at compile-time by the Solidity compiler built into Eris's tooling.


#### Debugging
Debugging within the project was performed in three different ways due to the variability of environments within the technology stack. For the React Native app, Google Chrome's DevTools<sup>[(devTools)](https://developers.google.com/web/tools/chrome-devtools/?hl=en)</sup> were utilised alongside the Xcode iOS simulator<sup>[(iOS Simulator)](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/Introduction/Introduction.html)</sup> in order to interact with the app and receive log outputs side-by-side in realtime.  
For the NodeJS server, the aforementioned `eris-logger` module was used to log API-related logging statements to the terminal.  
At the bare-metal level, Eris provided a way of continuously logging the activity of the Tendermint blockchain. Due to all operations approximating those of assembly language and thus being represented in hexadecimal, this was not useful in terms of locating bugs, but it provided a sanity check to ensure that the chain was performing the expected operations when instructed to do so by the NodeJS server, excluding a possible cause if a bug was being looked for.


#### Version Control
The `git`<sup>[(git)](https://git-scm.com/)</sup> command line utility was used to manage the codebase's development over time, in conjunction with GitHub<sup>[(GitHub)](https://github.com/)</sup> to provide a remote backup. For more involved `git` operations, such as merging branches, Atlassian's SourceTree<sup>[(SourceTree)](https://www.atlassian.com/software/sourcetree)</sup> GUI was used to avoid typing complex terminal commands and thus potentially performing unwanted changes to the version history.


#### Docker & Shell Scripts
Docker<sup>[(Docker)](https://www.docker.com/)</sup> is a platform which allows the creation of a standardised images for software development to mitigate arbitrary local differences between development environments. It was an essential part of the development process as Eris's tooling leverages Docker heavily to deploy blockchain instances.  
In order to automate the process of booting the Docker virtual machine and the local blockchain instance, the required sequence of commands was packaged into a Bash shell script, `envsetup.sh`, shown below.

**TODO: insert envsetup.sh**
