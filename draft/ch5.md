# Chapter 5
# Testing

## 5.1 Testing Strategy
Once the required communication abilities for the system had been established, meaning the author was able to trigger smart contract events within the blockchain via HTTP requests, the system's API was established and expanded via test-driven development. This meant that any new functions of the system would be developed in the following manner<sup>[(Beck, Kent. Test-driven development: by example. Addison-Wesley Professional, 2003.)]()</sup>:

1. A new test is added for the unwritten, proposed function. This ensures that the functions requirements are clear from the outset.
2. The test suite is run and the new test should fail. This excludes false positives (i.e. the test always passes) and confirms that the functionality does not in fact exist yet.
3. The minimum amount of code necessary to make the new test pass is written.
4. The test suite is run again. If tests fail, adjustments are made to the new function until all tests pass.

The system was therefore tested through a sequence of tests which became increasingly high-level over time as its functionality grew; beginning with modular unit tests, then moving onto integration tests for the API as a whole, and concluding with functional testing from the perspective of the client-side GUI.

## 5.2 Unit Testing
Within QuantiTeam, unit tests are focused on providing extensive coverage for all public API functions (known in JavaScript as "exported functions") of the server's modules. As the server simply provides an interface to the the Solidity contracts on the blockchain, this provided a method of simultaneously testing the contracts' public functions; a significantly more robust solution compared to attempting to construct a reliable test suite within the Solidity language. Unit tests purposefully excluded modules' private functions to avoid fixating the test suite on implementation details and to avoid breaking the encapsulation of each module<sup>[(Hunt, A., and D. Thomas. "Pragmatic Unit Test: in Java with JUnit." (2003): 65-78.)](http://data.ceh.vn/Ebook/ebooks.shahed.biz/JAVA/JUNIT/Pragmatic%20Unit%20Testing%20in%20Java%20with%20JUnit%20-%20Andrew%20Hunt%20&%20David%20Thomas%20-%20The%20Pragmatic%20Programmer.pdf)</sup>. This allowed the author to focus on ensuring that the API's components provided expected outputs to given inputs, thus implicitly testing the functionality of private functions which are called in the process.

The Mocha test framework – in combination with the Chai assertion library's `assert` function – were used to compare a function's returned result to an expected value:

```js
describe("addTask", function () {
    it("adds the given task object to the chain and returns the registered task's hex address", function(done) {
        taskManager.addTask(testTask, function (error, address) {
            assert.isNull(error);
            assert.isString(address, "`address` should be the registered task's hex address");
            done();
        });
    });
});
```

The snippet above shows the unit test for the `TaskManager` module's public `addTask` function. Mocha's straightforward support for asynchronous JavaScript is nicely exemplified here. The asynchronous function's callback parameters (`error, address`) are checked, after which `done()` is declared, indicating to Mocha that no further values are being awaited for this unit test.  
API data was provided to the unit tests via mocked objects where required, an example of which can be found in **Appendix X** for the `TeamManager` unit tests.

## 5.3 Integration Testing
Integration tests were implemented in a similar manner to the system's unit tests, but were focused on providing a full examination of the API's endpoints the system would provide and thus how differing modules/contracts coordinate with one another. This was achieved by encapsulating the sequence of functions required for each API endpoint in a `chain` module with an associated `chain_test` suite of tests. For example, attempting to retrieve a user's tasks via the `/tasks/:username` endpoint calls the `chain` module's `getUserTasks` function, which in turn simply defines a sequence of `UserManager` and `TaskManager` functions required to fulfill the request, returning the final result. Through this encapsulation, the author was able to define the `chain_test` suite of API tests which examined the API's outputs for given input parameters, ensuring that its constituent parts integrated with one another as expected.

## 5.4 Functional Testing
As the blockchain and server had been tested rigorously with both unit tests and integration tests, functional testing efforts could be focused on examining the behaviour of the client-side app and ensuring that its interactions with the API were represented correctly within the user interface. Functional tests were therefore executed by running the React Native app in the iOS simulator alongside Google Chrome's developer console, in order to examine log output generated by Redux actions. The `redux-logger` library showed its strength during the functional testing stage, by letting the author easily examine the app's state object before and after any action was triggered, thus significantly simplifying any debugging that was required.

## 5.5 Coverage
**TODO: final coverage report will be added when implementation is complete**
