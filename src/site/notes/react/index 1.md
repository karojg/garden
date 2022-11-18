---
{"dg-publish":true,"dg-permalink":"testing","permalink":"/testing/","dgHomeLink":true,"dgPassFrontmatter":false}
---


# Testing
https://btholt.github.io/complete-intro-to-react-v7/lessons/testing/testing-react
## Testing with Jest
Jest is the testing framework that Facebook puts out.
Jest is built on top of [Jasmine](https://jasmine.github.io/). Jasmine does the underlying testing part while Jest is the high level runner of the tests. Sometimes it's useful to consult the Jasmine docs too.

- Try to test functionality, not implementation.
- Make your tests interact with components as a user would, not as a developer would.
- Delete tests on a regular basis. Tests have a shelf life.

Jest has built into it: [Istanbul](https://istanbul.js.org/). Istanbul (which is not [Constantinople](https://youtu.be/vsQrKZcYtqg)) is a tool which tells you _how much_ of your code that you're covering with tests. Via an interactive viewer you can see what lines are and aren't covered. This used to be annoying to set up but now Jest just does it for you.

Add the following command to your npm scripts: `"test:coverage": "jest --coverage"` and go ahead run `npm run test:coverage` and open the following file in your browser: `open coverage/lcov-report/index.html`.