---
title: 'assumptions in unit tests'
author: mlaug
layout: post
categories:
  - agile
  - TDD
  - CDC
---

Assumptions in Tests
=========================

In my current project I was working on a rather larger piece of frontend based on React. By virtue of the TDD approach we had a rather large set of unit and integration tests. After some weeks we still recognised regression while the tests indicated "all good!".

The issue lay down in root of how we designed our test cases. While the unit tests where aiming for the React Component the integration tests where testing the frontend interacting with the backend endpoints, there was the glue missing between both. Since the data sets we got from the backend were rather big we provided artificial subsets within our tests cases.

```javascript
it('should show display a TODO', () => {
  const todo = { checklist: [1,2,3,4] }
  const todo = mount(
      <Todo
        todo={todo}
      />
  );

  const checklist = price.find('CheckList');
  expect(checklist).toHaveLength(4);
  // ... further expects
});
```

A usual setup for a test case based on the [Hoare Logic](https://en.wikipedia.org/wiki/Hoare_logic).

``` text
{P} C {Q}
```

Where ```{P}``` is our ```todo``` constant, ```C``` our React Component and ```{Q}``` our expectations asserted in the end. Our key focus should lie on ```{P}```, an assumption on the data provided to the test case. Data which is ultimately provided by the backend but is stubbed for the sake of isolated testing. Yet this very assumption has to be consistent and frequently checked to make sure the test cases are actually providing correct feedback. We have been proven that those assumption diverge very fast from the truth.

We set it as a goal to ensure parity between our assumptions and the reality. A goal for which contract testing (e.g. with PACT) has been introduced in the rise of distributed application in which the complexity of integration tests would increase by a magnitude with each service introduced. This concept we wanted to utilise to validate our assumption within our unit tests.

So we started creating assumptions in a separated file to reuse in various unit tests.

```javascript
export const TODO = {
  todoId: "2c57df44-f01a-4d38-a52c-aca810392e1e",
  name: "Holiday"
  checklist: [
    {item: "Go fishing", done: false},
    {item: "Go buying food", done: true}
  ]
}
````

This assumption can be importet by any unit test to avoid code duplication on this matter. Our unit test would be adjusted as following

```javascript
import {TODO} from 'assumptions' // import our assumption
it('should show display a TODO', () => {
  // provide our assumption to our React Component
  const todo = mount(
      <Todo
        todo={TODO}
      />
  );

  const checklist = price.find('CheckList');
  expect(checklist).toHaveLength(2);
  // ... further expects
});
```

Still those assumptions have to be verified. Here we use a PACT tests using the very same assumption.

```javascript
import {TODO} from 'assumptions'
describe('Pact', () => {

  // ... full example can be found
  // on https://github.com/pact-foundation/pact-js

  describe('todo', () => {
    beforeAll(() => {
      return provider.setup()
        .then(() => {
          provider.addInteraction({
            state: 'I have a todo',
            uponReceiving: 'a request for a todo',
            withRequest: {
              method: 'GET',
              path: '/todo/123',
              headers: {'Accept': '*/*'}
            },
            willRespondWith: {
              status: 200,
              headers: {'Content-Type': 'application/json'},
              body: TODO
            }
          })
        })
    })
  })
})
```

Given the generated pact is being implemented against at the provider end we have all our assumptions, even those in the unit test are being validated.

Let's play it through. Given the provider is changing it definition, which will break the contract. The team implementing the provider would therefore get in contact with the consuming team and inform about the change and make it adjust their consumer test. This results in adjustments of the assumption which are all imported in the unit tests. If all tests run through the likelihood that those introduced changes did not impact our frontend is significantly higher.
