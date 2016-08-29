# Chapter 4
# Design and Implementation

## 4.1 System Architecture
QuantiTeam broadly follows the three-tier architecture of a typical Model-View-Controller<sup>_[(REF)]()_</sup> (henceforth MVC) application with the important distinction that the roles within the MVC pattern are applied to an entire system of various applications, rather than a single application. In concrete terms, this means that blockchain represents the Model element by establishing the system's data model through the smart contracts applied to it, while the NodeJS server represents the Controller element, providing a public interface for client applications to issue requests to the blockchain and handling raw responses from the blockchain. The "View" element of the implementation is therefore interchangeable, as the RESTful API formed by the web server and the blockchain provides a uniform set of endpoints to communicate with, tying no special or unique value to the client, in this case a mobile app.

![A typical MVC Model](./diagrams/mvc.png)  
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
**TODO**

As was briefly touched upon in chapter 3, the Solidity programming language for smart contracts is still in its infancy. This meant that some data types, such as strings, had to be translated into fields of 32 byte arrays in hexadecimal when fed into the blockchain and translated vice versa when being retrieved from the blockchain.

## 4.3 Server-side API Architecture
As touched upon in the server-side analysis, the REST API server's role is first and foremost that of a data transformer and relay, forming a bridge between the blockchain and any given client-side implementation. The following subsections initially present how the server was designed to adhere to principles of both the MVC and REST design patterns, followed by an exploration how the server performs its bridging responsibilities in concrete terms.

### 4.3.1 The Controller
The server is able to fulfill its role as a Controller component within the system's MVC architecture, by virtue of being the deciding factor concerning the logic executed between a an HTTP request being received and a response issued within the API.  
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

By requiring all interactions with the API to take place in this form, the system is thus able to guarantee a high level of independence from concrete implementation details as the requirements and formatting of HTTP do not vary across implementation contexts.


#### Layered System



## 4.3.3 Structure

## 4.4 Client-side Architecture
### 4.4.1 The View
