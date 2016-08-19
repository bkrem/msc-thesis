



## <p style="text-align: center;">-/UCL LOGO/-</p>

# University College London

### Department of Computer Science

## MSc Summer Project Report

# QuantiTeam: Blockchain architecture as a medium to verify collaborative work
## A Feasibility Study?
</p>

_Author:_ Benjamin Kremer  
_Supervisor:_  Dr. Ghita Kouadri Mostefaoui

_This report is submitted as part requirement for the MSc Computer Science degree at UCL. It is substantially the result of my own work except where explicitly indicated in the text. The report may be freely copied and distributed provided the source is explicitly acknowledged._

September 2016

***
***

# Abstract
*TODO*

***
***

# Acknowledgements
*TODO*

***
***

# Contents
*ToC TODO*

# List of Figures
*TODO*

# Abbreviations
*TODO*

***
***

# Chapter 1
# Introduction

## 1.1 Problem Outline
Motivation is the key to human action. Without it, the source of any action performed is likely to be grounded in external pressures rather than the actor’s desire to act. In adolescents and young adults, this situation frequently arises within a key element affecting their future: their education.  
**...TODO...**  
**...specific reasoning for WHY a blockchain...***


## 1.2 Project Goals
_(placement?)_  
This project began as an attempt to address this phenomenon of student disengagement and, over time, developed into a wholly larger investigation of whether it is possible to utilise blockchain technology not just to form a network of re-engaged students, but as a tool for the verification of group-based work at large.  
_(/placement?)_  
The major goals of the project were thus twofold: in a broader sense, to demonstrate how the properties of a blockchain can be leveraged in a social, communal context, rather than the financial contexts they are usually applied to, and more concretely, to create a free, open and extensible platform for students of all ages to re-engage with their peers in a productive, educational manner, underpinned by a model of motivation which rewards collaborative work as a team.  

Working from this technological and philosophical objective, respectively, the following specific goals for the project were established:
- Create a public API - built on a NodeJS server - to interface between users and a blockchain, independent of a chosen client-side implementation.
- _/Offer a straightforward upload functionality to the blockchain for task-related files via said NodeJS server./_
- Provide an iOS client-side app for basic task/user/team management to provide maximum accessibility and familiarity for users.
- Establish a basic deployment of at least 4 distributed validator nodes to provide a true proof-of-stake consensus mechanism _[REF]()_ for the platform.

## 1.3 Personal Aims
**TODO: Blurb**  
*Talk about following the bitcoin movement since before I even started coding.*  
*Maybe talk about Golang-Docker relationship and how this initially piqued my interest in Docker*  
Having been a follower of the bitcoin _[REF]()_ movement for more than two years, the author has been looking for an appropriate opportunity to apply the blockchain data structure, which underpins bitcoin's public ledger, to a project of his own. This project's partial goal of verifiability of team work fits a blockchain's ability to provide a pseudo-anonymous public ledger of transactions or events.  

*(Facebook's React Native _[REF]()_ was chosen for client-side development after using React _[REF]()_ and Cordova _[REF]()_ heavily in a previous MSc project to provide a mobile application, but being unsatisfied with both the stability and interactive feel of the final product. to provide the simplest but powerful UX possible for a complex backend.)*

The following points provide an overview of the author's personal aims for the project:
- Gain familiarity with native app development by utilising existing JavaScript knowledge via React Native.
- Learn how to write smart contracts in Solidity _[REF]()_ that are useful in the context of the project and for existing platforms such as Ethereum _[REF]()_.
- Implementing a form of blockchain technology (in this case proof-of-stake), by learning how to set up, run & maintain a a blockchain proficiently.
- Gain a fundamental understanding of Docker _[REF]()_ and how it leverages _"containerisation"_ to provide ease of environment deployment.


## 1.4 Project Management
The project followed the methodology of the Unified Process (henceforth UP) framework _[REF]()_. Given the circumstances of two-thirds of the technologies being familiar to the author, whereas the blockchain aspect was almost entirely new ground, implementing **TODO: model x** made the most sense for the project.  
Given the limited amount of time available for a project of these dimensions, along with the research and experimentation required to validate whether the described goals were even achievable, user requirements were captured using a heuristic of what the essential functionality would be for the client-side application, and thus the API in general, in order for a user to create, manage and discuss their open tasks with their team. This was done by utilising the MoSCoW categorisation scheme _[REF]()_.  

The system was at first modeled with a theoretical deployment diagram in order to gain an understanding of which technological entities were required to neatly encapsulate responsibilities. Sequence diagrams were then used for more complex client-server-blockchain interactions at each new stage of the UP, where a new stage usually took the form of defining and implementing a new domain of the API, e.g. task management or user management.

As the API continued to grow laterally, adding an array of endpoints for each new domain, unit testing the API's public functions became an essential part of the project workflow. Due to the primitive error handling of the still very young language Solidity (used to construct smart contracts for blockchains), any responses coming from the blockchain had to be rigorously tested server-side in order avoid silent breakages and regressions.

**TODO: add UP diagram**  
**TODO: add Gantt chart?**


## 1.5 Report Overview
*TODO*


***
***

# Chapter 2
# Background Information and Related Applications

## 2.1 Context

## 2.2 Previous Work/Existing Applications

## 2.3 Programming Languages and Libraries
### 2.3.1 Blockchain

### 2.3.2 Server-side

### 2.3.3 Client-side
