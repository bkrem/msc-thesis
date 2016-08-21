



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
**TODO**


***
***

# Chapter 2
# Background Information and Related Applications

## 2.1 Blockchains & Dropouts
(__intro is suboptimal__)  
A 2004 study by the National Research Council showed that upwards of 40% of high school students feel disengaged from learning and exert little effort on school work [(Center on Education Policy 2012)](http://cep-dc.org/displayDocument.cfm?DocumentID=405 ). More worryingly still, 70% of dropouts stated lack of motivation, in a 2006 study, as the key reason for their departure from school [(Bridgeland, J.M. et al., 2006. The Silent Epidemic: Perspectives of High School Dropouts)](). In liaison with these findings, a study which tracked the educational careers of a group of individuals found that 23% of those who dropped out cited a sense of not belonging as the reason for their departure from school [(Berktold et al., 1998, Table 6)]().  

While the factors for a student's motivation (or lack of it) regarding their education may stem from a myriad of factors, engaging the individual on a social level may be thus be posited as a key driver in motivating them to remain engaged in the process [((Johnson et al., 2001; Newmann, Wehlage, and Lamborn, 1992; Tinto, 1993; Wehlage, Rutter, Smith, Lesko, and Fernandez, 1989))](). Of course, simply being bound into a social context does not mean that the student will make meaningful progress regarding their work, as attendance does not constitute attention to what is being taught [(_ibid_)]().  

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


### 2.3.3 Client-side


### 2.3.4 Databases: SQL vs NoSQL vs Blockchain


## 2.4 Tooling
### 2.4.1 Requirements and Design Tools
### 2.4.2 Development Tools
