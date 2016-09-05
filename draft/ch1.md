# Chapter 1
# Introduction

## 1.1 Problem Outline
Motivation is the key to human action. Without it, the source of any action performed is likely to be grounded in external pressures rather than the actor’s desire to act. In adolescents and teenagers, this situation frequently arises within a key element affecting their future: their education.  
This observation stems from the author's personal experiences during his youth and, more recently, from experiences as a voluntary English language teacher. To the author, there were two clearly identifiable steps which could lessen the amount of disengagement from school-related matters that is often observed in adolescents and teenagers. Firstly, providing a way for students to quantify their achievements could drive re-engagement with the learning material, by breaking it into manageable chunks with clearly defined rewards for each chunk.  
Secondly, adding an explicitly social aspect to this quantification process by focusing on collaborative work could further drive motivation and re-engagement, by creating a social net for students to feel valued and as carriers of responsibility towards others. Beyond achieving a greater sense of purpose for each individual, this quantification of collaborative work could also double as a useful metric of team work skills to refer back to at a later point in time, filling a gap for a skill that is currently not quantifiable or verifiable on paper.


## 1.2 Project Goals
This project began as an attempt to address this phenomenon of student disengagement and, over time, developed into a wholly larger investigation of whether it is possible to utilise blockchain technology not just to form a network of re-engaged students, but as a tool for the verification of group-based work at large.  
The major goals of the project were thus twofold: in a broader sense, to demonstrate how the properties of a blockchain can be leveraged in a social, communal context, rather than the financial contexts they are usually applied to, and more concretely, to create a free, open and extensible platform for students of all ages to re-engage with their peers in a productive, educational manner, underpinned by a model of motivation which rewards collaborative work as a team.  

Working from this technological and philosophical objective, respectively, the following goals were established for the project:
- Create a public Application Programming Interface (henceforth API) to allow users to interact with a blockchain in a well-defined manner, independent of a chosen client-side implementation.
- Provide an iOS client-side app for basic task/user/team management to offer a graphical representation of the API in a format familiar to users.
- Establish a basic deployment of at least 4 distributed validator nodes to implement a true proof-of-stake consensus mechanism<sup>[PoW vs PoS](http://bitfury.com/content/5-white-papers-research/pos-vs-pow-1.0.2.pdf)</sup> for the platform.


## 1.3 Personal Aims
Having been a follower of the bitcoin<sup>[(Bitcoin)](https://bitcoin.org/en/)</sup> movement for more than two years, the author has been looking for an appropriate opportunity to apply the blockchain data structure, which is a key part of the technology underlying bitcoin, to a project of his own. This project's goal of verifiability of team work fits a blockchain's ability to provide a distributed ledger of transactions or events.  

The following points provide an overview of the author's personal aims for the project:
- Become familiar with native mobile app development by utilising existing JavaScript knowledge in the context of React Native<sup>[(RN)](https://facebook.github.io/react-native/)</sup>.
- Learn how to write smart contracts in Solidity _[REF]()_ that are useful in the context of the project and for existing platforms such as Ethereum _[REF]()_.
- Implement a form of blockchain technology, by learning how to set up, run & maintain a a blockchain proficiently.
- Gain a fundamental understanding of Docker _[REF]()_ and how it leverages "containerisation" to achieve straightforward deployments of development environments.


## 1.4 Project Management
The project followed the methodology of the Unified Process (henceforth UP) framework for software development<sup>[(Jacobson, Ivar, et al. The unified software development process. Vol. 1. Reading: Addison-wesley, 1999.)](http://tocs.ulb.tu-darmstadt.de/82047766.pdf)</sup>, and was thus structured in terms of iterative phases. The system was at first modeled as a whole by using a theoretical deployment diagram in order to gain an understanding of which technological entities were required to neatly encapsulate the responsibilities within the system. Sequence diagrams were then used to model more complex client-server-blockchain interactions during each UP iteration, where a new stage usually took the form of defining and implementing a new domain within the API, e.g. task management or user management.

Given the limited amount of time available for a project of these dimensions, along with the research and experimentation required to validate whether the described goals were even achievable, user requirements were captured using a heuristic of what the essential functionality would be for the client-side application, and thus the API in general, in order for a user to create, manage and discuss their open tasks with their team. This was done by utilising the MoSCoW categorisation scheme _[REF]()_.   

During the implementation stage of the project, a daily log (**see Appendix X**) was kept of the key advances and issues encountered during that day of development. This served as a useful tool to summarise the progress made, as well as providing an immediate opportunity to document issues that were encountered.  
Throughout the implementation stage, required programming work was broken down into separate units of work by using Github Issues<sup>[(REF)]()</sup> and organised into a Kanban board<sup>[(REF)]()</sup> with the help of waffle.io<sup>[(waffle.io)](https://waffle.io/)</sup>.

As the API continued to grow laterally, adding a multitude endpoints for each new domain, unit testing the API's public functions became an essential part of the project workflow. Due to the primitive error handling of the still very young Solidity programming language (used to construct smart contracts for blockchains), any responses coming from the blockchain had to be rigorously tested server-side in order avoid silent breakages and regressions.

Additionally, a Gantt chart (**see Appendix X**) was established at the outset of the project in order to visualise the progression of the project over time and to ensure the amount of work set out to be completed was within a realistic margin for the amount of time available to the author.


## 1.5 Report Overview
#### Chapter 1
This chapter outlines the problem addressed by the project, as well as stating its goals, the author's personal aims, and the approach to managing the project.

#### Chapter 2
This chapter establishes context for the stated problem domain, surveys previous work either directly or tangentially related to the project, and provides an overview of the programming languages, libraries and development tools used within the project.

#### Chapter 3
This chapter specifies the project's requirements and their associated use cases, whilst also assessing the system's architecture by analysing each major component of the its technology stack.

#### Chapter 4
This chapter details the key features and patterns that make up QuantiTeam's implementation, assessing the blockchain, the server and the client-side app in turn.

#### Chapter 5
This chapter explains the approach of test-driven development used for the project, as well as outlining how the system was tested at different levels of granularity, using unit tests, integration tests, and functional testing.

#### Chapter 6
This chapter assesses to what extent the project's goals and the author's personal aims were achieved, provides a critical review of the system as a whole, and offers pointers for possible future work.
