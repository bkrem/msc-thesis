# Chapter 6
# Conclusions and Project Evaluation

## 6.1 Project Goals
At the outset of this project, the author intentionally defined the goals at a level which would be challenging, even possibly unfeasible, to reach in the time space available, with no other developers to split workloads with. Considering this context, the project's goals were largely met, forming a healthy basis for future endeavours of this kind to build on.  
The first and most challenging goal was to establish a functioning public API which would allow individuals to form teams of their own choosing and register the team's completed tasks on a blockchain. This was achieved in what the author would classify as a "naïve" implementation. Section 6.4 provides details on the author's reasoning for the term  "naïve".  
The second goal of the project was to build a client-side application to showcase the API's functionality in visual terms. This goal was achieved at a level satisfactory to the author, considering that the primary focus of the project lay in establishing the API and the stack of technologies backing it.  
The third goal was to deploy a basic network of validator nodes which would implement a true proof-of-stake consensus algorithm for the blockchain. This was a surprisingly smooth undertaking due to the high quality CLIs of Eris, Docker and AWS, allowing a smooth transition from a single local node to a truly distributed set of validator nodes for the blockchain.


## 6.2 Personal Aims Achieved
The following section reiterates each personal aim set out by the author, providing commentary on whether the aim has been achieved and in what way.

- _Become familiar with native mobile app development by utilising existing JavaScript knowledge in the context of React Native._  
Using React Native to build the client-side application was as useful an exercise as the author had hoped it would be. Previous experience with React and JavaScript allowed a full focus on elements specific to a mobile platform, such as self-contained ListViews, which are a common feature in mobile development but absent in web development, where a list is generally an abstract concept constructed from HTML elements.

- _Learn how to write smart contracts in Solidity that are useful in the context of the project and for existing platforms such as Ethereum._  
Despite initial struggles due to Solidity still being in its early days as a programming language, the project offered ample opportunity to learn how to design and implement various types of smart contracts and how to write idiomatic Solidity in general.

- _Implement a form of blockchain technology, by learning how to set up, run & maintain a a blockchain proficiently._  
The author feels that he was able to cover all key aspects involved in developing and maintaining a piece of software which leverages a form of blockchain technology.

- _Gain a fundamental understanding of Docker and how it leverages "containerisation" to achieve straightforward deployments of development environments._  
Working with the `docker-machine` CLI to run and manage virtual operating system environments which could in turn run pre-packaged ("containerised") software taught the author how much the usually arduous and fiddly task of setting up dependencies for a piece of software can be abstracted and simplified, by bundling the software along with its dependencies into a single unit, a container.


## 6.3 Critical Evaluation
### 6.3.1 Suitability of Chosen Technologies


### 6.3.2 Design & Implementation Scope
- should've focused on API only, not done client-side
- Requirements: must haves were implemented, number of shoulds still outstanding

### 6.3.3 Key Issues


- State of Solidity as a bottleneck
- Didn't make my tests atomic enough -> cascading failures if one test breaks -> lesson for next time
- Trickiness involved in getting ListViews right in RN -> FB's `PureListView` helped a lot here to understand optimal structuring and mapping of data flow
- Started with IDs in contract schema, didn't really need them -> addresses double as unique IDs
- Should've only done quick mocks of views as sketches and then implemented statically ad-hoc, too much time wasted on now not-implemented views

- [Chain] **!Issue!** Had to find a workaround for the `library` keyword bug in the eris platform --> simply turned it into a contract --> less flexible than an actual library type though.
- [Chain] **!Issue!** Tested out the LinkedList implementation and replaced it with the SequenceArray implementation after running into retrieval issues
- **!Issue!** Eris online compiler broke --> took forever to figure out that port `10113` was viable option because no goddamn documentation of it
- **!Issue!** Eris local compiler Docker image did not do anything basically, TOTAL FAIL


## 6.4 Future Work
- why naive
- Security (GPG)
- Ranking
- GPS
- Plagiarism detection
- Smart contract architecture


## 6.5 Final Thoughts
