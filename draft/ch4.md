# Chapter 4
# Design and Implementation

## 4.1 System Architecture
QuantiTeam broadly follows the three-tier architecture of a typical Model-View-Controller<sup>[(Krasner, Glenn E., and Stephen T. Pope. "A description of the model-view-controller user interface paradigm in the smalltalk-80 system." Journal of object oriented programming 1.3 (1988): 26-49.)](http://heaveneverywhere.com/stp/PostScript/mvc.pdf)</sup> (henceforth MVC) application with the important distinction that the roles within the MVC pattern are applied to an entire system of various applications, rather than a single application. In concrete terms, this means that blockchain represents the Model element by establishing the system's data model through the smart contracts applied to it, while the NodeJS server represents the Controller element, providing a public interface for client applications to issue requests to the blockchain and handling raw responses from the blockchain. The "View" element of the implementation is therefore interchangeable, as the RESTful API formed by the web server and the blockchain provides a uniform set of endpoints to communicate with, tying no special or unique value to the client, in this case a mobile app.

![A typical MVC Model](../diagrams/mvc.png)  
Source: https://developer.chrome.com/apps/app_frameworks

In order to mirror the described approach of creating a standalone unit out of the Model and Controller aspects of the system's architecture, the project's source code was structured into two directories at the highest level. As represented by the shallow file tree below, the `app` directory contains the React Native client-side application, while the `chain` directory holds configurations and environment variables to run the Tendermint blockchain via the `eris` CLI, and provides the REST API server in its `js` subdirectory.  

```
.
├── app
│   ├── android
│   ├── index.android.js
│   ├── index.ios.js
│   ├── ios
│   ├── js
│   ├── node_modules
│   └── package.json
├── LICENSE
├── README.md
├── chain
│   ├── Dockerfile
│   ├── abi
│   ├── accounts.json
│   ├── app.js
│   ├── config
│   ├── contracts
│   ├── envsetup.sh
│   ├── epm.json
│   ├── epm.yaml
│   ├── js
│   ├── logs
│   ├── node_modules
│   ├── package.json
│   ├── simplechain.sh
│   └── test
├── log.md
└── npm-debug.log
```

## 4.2 Blockchain Architecture
### 4.2.1 The Model
Within the MVC paradigm, models represent the central structure of the application and "are concerned with neither the user-interface nor presentation layers but instead represent unique forms of data that an application may require"<sup>[(Learning JS Design Patterns)](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#detailmvc)</sup>.  
The Tendermint blockchain serves as a model by defining the system's domain through the collection of smart contracts it holds, thus setting the boundaries for what kinds of data the system is able to store and what kind of operations can be performed on the data.


### 4.2.2 Working with Solidity
To put a number of the design decisions made during the construction of the blockchain's smart contracts into perspective, some context regarding the current capabilities of the Solidity programming language is required.  

#### Strings and Numbers
As was briefly touched upon in chapter 3, Solidity is still in its infancy. This meant that some data types such as strings, had to be translated into fields of 32 byte arrays in hexadecimal when fed into the blockchain and translated vice versa when being retrieved from the blockchain.  
Furthermore, translating numeric values from a dynamically-typed language such as JavaScript to a strict, statically-typed one such as Solidity without side effects or data corruption is also no trivial task. A point which illustrates these possibly unpredictable side effects is the possibility of an unusually large integer being fed into the API by a client application, which is then translated to an 8-bit unsigned integer (`uint8`) field within a Solidity contract, thus writing an irreparably truncated and corrupted value to the blockchain.  
A conscious decision was therefore made to encode all values – aside from booleans which are able to move across the API unaltered – in hexadecimal before they are fed into the blockchain, regardless of initial type, to utilise Solidity's `bytes32` type. This approach provided two important advantages:

_Predictability_ - Transforming all data associated with the blockchain in a regular manner enhances the testability of the API by fixing the data's representation, both when it is entered into and retrieved from the chain. This increases the API's level testability by making expected outputs for a given input more uniform and free of special circumstances.  

_Simplicity_ - Establishing a standardised format for any data to be entered into the blockchain helped keep the API straightforward to work with. Any numeric or string data could be encoded for the chain by the NodeJS server using the `eris-wrapper` library module's `str2hex()` (string-to-hex) function, and decoded using its `hex2str()` (hex-to-string) function.  

#### Arrays and Objects
A further hindrance imposed by Solidity was its poor support for arrays and its lack of objects, also known as lists and dictionaries, respectively.

While JavaScript is composed entirely of objects, Solidity provides the `mapping` type; a data structure similar to what is commonly known as a hash map<sup>[(REF)]()</sup>. This would have provided a sufficient parallel to translate data formatted in JavaScript Object Notation<sup>[(JSON)](http://www.json.org/)</sup> (henceforth JSON) into a format digestible by Solidity contracts, were it not for the fact that Solidity's ability to store and perform operations on its mappings and arrays is limited at best. For example, storing user profiles (i.e. `struct`s) in a dictionary (i.e. a `mapping`) would have been possible, but retrieving a particular collection of dictionary entries would have been impossible, as neither Solidity's arrays nor its `mapping` type are iterable data structures by themselves. More specifically, a `mapping` can only be "probed" for a specific value, making it impossible to iterate over the structure and retrieve a subset of its properties which match a given criteria. This would have meant searching the blockchain in any non-trivial manner would have been highly unfeasible.  
To mitigate this functional bottleneck in Solidity, the author adopted an implementation of a SequenceArray<sup>[(_Data Structures And Algorithm Analysis in Java, p. 97_)]()</sup>, which became the `SequenceArray.sol` contract. The SequenceArray provided a well-defined interface of methods on top of Solidity's native array and `mapping` types, allowing the author to focus on _when_ and _why_ data should be manipulated or searched, rather than _how_ these operations are performed.

### 4.2.3 Smart Contracts
#### Data: Factory Contracts
The blockchain's factory contracts were a straightforward undertaking, both in regards of design and implementation, as their only role was to return a new instance of the contract, which would then be handled by the associated manager contracted. This meant the factory contract, in its simplest form, would contain its required fields and a constructor, as shown below:

```js
contract Task {

    bytes32 public id; // immutable
    bytes32 public title; // mutable
    bytes32 public desc; // mutable
    bytes32 public status; // mutable
    bytes32 public complete; // mutable
    bytes32 public reward; // immutable
    bytes32 public participants; // mutable
    bytes32 public creator; // immutable

    // Constructor
    function Task(
        bytes32 _id,
        bytes32 _title,
        bytes32 _desc,
        bytes32 _status,
        bytes32 _complete,
        bytes32 _reward,
        bytes32 _participants,
        bytes32 _creator
        ) {
        id = _id;
        title = _title;
        desc = _desc;
        status = _status;
        complete = _complete;
        reward = _reward;
        participants = _participants;
        creator = _creator;
    }
}
```

Solidity's inability to process an externally passed JavaScript object is nicely exemplified here, due to the sheer verbosity of the constructor function. While a task could be encapsulated as a single object and therefore passed as a single parameter within the client- and server-side JavaScript applications, the object had to be split into its constituent fields by the NodeJS server before it could be handed to the blockchain. This created what is commonly referred to as a "code smell"<sup>[(Tufano, Michele, et al. "When and why your code starts to smell bad." Proceedings of the 37th International Conference on Software Engineering-Volume 1. IEEE Press, 2015.)](http://www.cs.wm.edu/~denys/pubs/ICSE'15-BadSmells-CRC.pdf)</sup>, by forcing the author to implement an excessively large amount of parameters. This made the both the factory contracts themselves and the server's methods that deconstructed the initial JavaScript object for the blockchain brittle, as any change to a factory contract's fields had cascading effects throughout the API, thus creating an unnecessary opportunity for bugs to appear if refactoring was not done in a meticulous manner.


#### Operations: Manager Contracts
As a contract type, Manager contracts are chiefly responsible for indexing and modifying instances of data domain contracts, returned to them by their respective factory contract. How a Manager-type contract fulfills this role is exemplified with the code snippet below, which is an extract from the `UserManager` contract's `addUser()` method:

```js
// ...
if (isOverwrite) {
    return 0x0;
} else {
    User u = new User(_id, _username, _email, _name, _password);
    userList.insert(_username, u);
    return u;
}
// ...
```

The method previously checks whether the username already exists in the `userList` sequence array and writes the result to the `isOverwrite` variable, indicating that this would be an "overwrite" rather than a "create" operation. If the username already exists `addUser()` returns `0x0`, a null pointer address, thus indicating to the NodeJS server that adding the user failed. This again exemplifies the rather primitive state of the Solidity language at the time of writing, as the null address has to serve as an implicit error due to the absence of an error type within the language.  
If `isOverwrite` is false on the other hand, the `UserManager` contract creates a new instance of the `User` factory contract by invoking `new User(...)`, returning a pointer address which indicates the position of the newly created `User` contract in the blockchain. This address is then inserted into `userList`, where entries are indexed by using the passed `_username` variable as a key, to facilitate later retrieval.

Furthermore, the flexibility provided by the data structure contract – in this case a sequence array – became especially clear during the implementation of the Manager contracts. A Manager contract could simply instantiate a `SequenceArray.sol` contract for its own purposes and use only the minimum amount of SequenceArray methods it required to fulfill its functions, by creating a wrapper function around the method, and adding additional context as required, such as logging an event:

```js
contract UserManager {
    SequenceArray userList = new SequenceArray();

    // other methods...

    function isUsernameTaken(bytes32 _username) constant returns (bool) {
        registerActionEvent("IS USERNAME TAKEN");
        return userList.exists(_username);
    }
    // ...
}
```

- Shape of the Factory and Manager contracts determined the Node server's structuring almost entirely

#### Relations: Linker Contract
- Addresses as unique identifiers: Started with IDs in contract schema to follow PK/FK relational principle, didn't really need them -> addresses double as unique IDs
- PK/FK simulation via the `linker` and on-chain addresses
- `User` and `Team` contracts contain a SequenceArray to track associated tasks

**TODO Class diagram for contracts**

### 4.2.4 Tendermint and Eris
- `Eris` configs for deployment, eris CLI
- **!Issue!** Eris online compiler broke --> took forever to figure out that port `10113` was viable option because no goddamn documentation of it
- **!Issue!** Eris local compiler Docker image did not do anything basically, TOTAL FAIL

## 4.3 Server Architecture
As touched upon in the server-side analysis, the REST API server's role is first and foremost that of a data transformer and relay, forming a bridge between the blockchain and any given client-side implementation. The following subsections initially present how the server was designed to adhere to principles of both the MVC and REST design patterns, followed by an exploration how the server performs its bridging responsibilities in concrete terms.

### 4.3.1 The Controller
The server is able to fulfill its role as a Controller component within the system's MVC architecture, by virtue of being the deciding factor concerning the logic executed between an HTTP request being received and a response issued within the API.  
**TODO more**


### 4.3.2 RESTfulness
REST, which is an acronym for Representational State Transfer, is a commonly used web development pattern which attempts to ensure reliability and scalability of the web service implementing it<sup>[(REST)](http://whatisrest.com/rest_constraints/index)</sup>. Within the context of QuantiTeam, achieving RESTfulness was key for the system's API, as this would help ensure the system's usefulness to any context of client-side implementation, rather than specifically a mobile paradigm.  
This subsection therefore describes the properties of a RESTful service and how each applies to QuantiTeam's server-side architecture.

#### Client-Server Dichotomy
Crucial to the creation of an implementation-agnostic REST interface is a separation of concerns between the client and server<sup>[(client-server)](http://whatisrest.com/rest_constraints/client_server)</sup>. Strictly separating client and the server roles from each other provides portability and replaceability, as the underlying implementation of either may change without affecting the standardised form of communication established by the REST interface.  
Within QuantiTeam, there is a clear client-server dichotomy between the server which is solely concerned with handling incoming requests, and the React Native client app, which requests data from the server and then decides how to represent said data to the user within its local state.


#### Stateless
Statelessness is a key constraint within the REST design, which holds that each request should contain all information required by the service to process the request, and the service's response should contain all the required data to fulfill said request<sup>[(statelessness)](http://whatisrest.com/rest_constraints/stateless)</sup>.  
Applied to QuantiTeam, the REST pattern's property of statelessness not only helped to decouple the system's architecture by avoiding the need for managing state across different applications, it was also the most feasible approach to enable a straightforward way of communicating with the Tendermint blockchain, due to the previously described frequent requirement to transform data as it travelled to and from the blockchain. Statelessness, in this context, meant that there was no need to worry about intermediate representations of the data being stored and inappropriately forwarded to either the client or the blockchain at a later point in time.

A simple example of how the server remains stateless while fulfilling its function as an API interface is shown in the following snippet, taken from the `server.js` module:

```js
app.post('/user/taken', function (req, res) {
    var username = req.body.username;

    log.info("POST /user/taken: ", username);
    userManager.isUsernameTaken(username, function (err, isTaken) {
        _handleErr(err, res);
        res.json({isTaken: isTaken});
    });
});
```

This example shows the `/user/taken` API endpoint, which is responsible for validating the availability of usernames. When a user tries to sign up in the client-side app, the server receives the proposed username in the request body (the only piece of information required for the API to fulfill the request), with which it calls the `userManager`'s `isUsernameTaken()` method. The method returns an `isTaken` boolean which is written to the `res` response object, thus providing all data necessary for the client to make a decision regarding the availability of the proposed username.


#### Cacheable
Responses from the REST service being implicitly or explicitly declared as cacheable or non-cacheable is a further component of a RESTful service<sup>[(cache)](http://whatisrest.com/rest_constraints/cache)</sup>, as cacheable responses provide an opportunity to to optimise the amount of client-server communication that is needed.  
QuantiTeam's API currently does not provide explicit cache labels in its responses and is therefore implicitly cacheable on the client side. While cacheability is key to scaling a developed API, investing significant amounts of time to implement explicit cache invalidation within QuantiTeam's first iteration was outside of the project's scope. Implicit caching takes place within the React Native client-side app, which retrieves and then locally retains static information such as the user's profile data, for example.


#### Uniform Interface
A uniform technical interface – which provides a generic, high-level method of communication across all services within a REST architecture – is seen as the primary constraint which distinguishes the REST approach from other approaches to web service architectures<sup>[(uniform interface)](http://whatisrest.com/rest_constraints/interface_uniform_contract)</sup>.  
For QuantiTeam, this interface was constructed by using HTTP URI paths to define API endpoints, which a client could then issue requests to. Uniformity was further enforced by creating the following semantic template which all of the URI paths defined for the API would follow:

`/<data-domain>/<optional-subdomain>/:<optional-parameter>`

This URI template declares that the first path segment is mandatory and shall be the data domain the request is directed towards, for example `/user` or `/tasks`.  
The second segment may be composed of a further specification within the data domain, such as `/profile` for `/user`, thus forming the path `/user/profile`.  
The third and final segment consists of an optional request parameter, such as `:username` which may be used to pass a parameter to the API in a HTTP GET request. Building on the previous example, we may therefore be able to request the profile data for user `foo` by issuing a request to the API via the path `/profile/user/foo`.

By requiring all interactions with the API to take place in this form, the system is thus able to guarantee a high level of independence from concrete implementation details, as the requirements and formatting of HTTP do not vary across implementation contexts.


#### Layered System
The final constraint within the REST pattern concerns composability. A RESTful implementation should allow for intermediate layers, commonly known as middleware, to be inserted into the service without affecting the interface for communication in any way<sup>[(layered system)](http://whatisrest.com/rest_constraints/layered_system)</sup>.  
QuantiTeam meets this constraint through its HTTP URI interface, which enables middleware to implement the same interface and forward a given request to the service itself using the same exact URI once it has completed its part of processing the request.


## 4.3.3 Data Handling
**TODO**
...  
Said pipeline function was defined within the utility module `chainUtils.js` as `marshalForChain(<data-object>)`. The function takes  it's `<data-object>` parameter, identifies the original type which initially identified the  utilised the `eris-wrapper` library module's `str2hex()` (string-to-hex)
, responsible for preparing the data to be entered,  
...

## 4.4 Client-side Architecture
### 4.4.1 The View


### 4.4.x Common Components
Within the context of QuantiTeam, a good example of how common UI components were identified is the `Header` component.

- `common`
    - `Header` as example of cross-platform reusable abstraction
    - Globals (GlobalStyles); RN uses component-based hyper-modular CSS, GlobalStyles provided nice way to mix in common styling (e.g. view margins)

### 4.4.x Flowtype
Furthermore, the Flowtype static type analyser elevated the ability to define actions and the expected types of their payloads in a more fine-grained manner compared to regular JavaScript, by enabling the author to add type constraints to variables and functions where needed. The following extract demonstrates the usefulness of additional static typing:

`{ type: 'FETCH_TASKS_SUCCESS', tasks: Array<Task>, receivedAt: number }`

The action above is triggered upon successfully fetching the user's tasks from from the blockchain via the API. The payload includes a `tasks` property, which is expected to be an array of `Task` type objects, and a `receivedAt` property, which should be a Unix timestamp and is therefore expected to be of the type `number`.
