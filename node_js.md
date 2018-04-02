# Node.js

[Promises and Node.js event emitters don't mix](https://qubyte.codes/blog/promises-and-nodejs-event-emitters-dont-mix)

>If you want to ensure that unhandled error events lead to uncaught exceptions which aren't captured by a catch or a promise chain, then you can wrap the emission in a setTimeout or a setImmediate.
