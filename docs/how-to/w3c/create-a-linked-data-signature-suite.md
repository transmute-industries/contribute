# How to create a W3C Linked Data Signature Suite

## 1. Read the [latest specification](https://w3c-dvcg.github.io/ld-signatures/).

## 2. Create a repository 

`PROPOSAL-SuiteName2019` with [Apache License 2.0 License](http://www.apache.org/licenses/LICENSE-2.0) and a README describing why you believe a new signature suite is needed, and referencing an existing signature suites that did not meet your needs.

## 3. Develop your signature suite

Be sure to add unit tests and documentation. Keep the implementation and dependencies as small as possible. Be sure to define the following [terminology](https://w3c-dvcg.github.io/ld-signatures/#terminology) specifically:

##### canonicalization algorithm

> An algorithm that takes an input document that has more than one possible representation and always transforms it into a deterministic representation. For example, alphabetically sorting a list of items is a type canonicalization. This process is sometimes also called normalization.

_prefer [URDNA2015]._(https://github.com/digitalbazaar/jsonld.js/#canonize-normalize)_.

##### message digest algorithm

> An algorithm that takes an input message and produces a cryptographic output message that is often many orders of magnitude smaller than the input message. These algorithms are often 1) very fast, 2) non-reversible, 3) cause the output to change significantly when even one bit of the input message changes, and 4) make it infeasible to find two different inputs for the same output.

_prefer [sha256](https://www.movable-type.co.uk/scripts/sha256.html)._

##### signature algorithm

> An algorithm that takes an input message and produces an output value where the receiver of the message can mathematically verify that the message has not been modified in transit and came from someone possessing a particular secret.

_Be sure to link to the cryptographic protocol specification in as much detail as possible, including ideally a reference to a working implementation with 100% test coverage and a security review._

## 4. Consider Compatibility

As you are creating a new signature suite, it is unlikely that any software systems exists which can generate and verify compatible signed linked data. However, you should add tests that get as close to a compatible test as possible.

For example, you should ensure that the raw signatures can be verified with multitple implementations of the signature algorithm. Ensure the the JSON-LD can be expanded and compacted, and parsed by [jsonld.js](https://github.com/digitalbazaar/jsonld.js/), or a similar library for languages other than javascript. Ensure that your signature encoding process is clear, and that the signature payload is as small as it can reasonably be, for example: for ecdsa over secp256k1, you may be encoding r and s Big Number values as hex, concatonated them, and then base64url encoding such as `base64url.encode(${hex(BN(r))}${hex(BN(s))}${v})`, make it very clear what you are doing so its easy for someone to reconstruct your method in a different language.

_Prefer 100% test coverage, to ensure compatibility can be established via TDD._

## 5. Draft an edit to the [registry](https://w3c-ccg.github.io/ld-cryptosuite-registry/)

Create a repository for the suite specification that is seperate from your concrete implementation, for example [lds-rsa2018](https://github.com/w3c-dvcg/lds-rsa2018). 

Open a Pull Request [here](https://github.com/w3c-ccg/ld-cryptosuite-registry/) to include your specification.

Here is an example [example](https://github.com/w3c-ccg/ld-cryptosuite-registry/pull/2).


## Next Steps

- Add badges to your concrete implementation repo for CI, Greenkeeper, Snyk, Code Coverage, License, Documentation, etc...
- Open a pull request against [jsonld-signatures](https://github.com/digitalbazaar/jsonld-signatures) once your suite has been approved by the W3C.
- Share your implementation on social media and at conferences.
- Respond to issues, collaborate with the community.
