# Internal Program Error Management System

The Internal Program Error Management System (IPEMS) manages errors within a program in a universal system similar to HTTP status codes. The system is designed to avoid much of the ambiguity inherent in writing error messages in human language, while simultaneously allowing standardized, simple communication of the nature of an error via a numerical code, or a generated natural language message.

## Motivation

The world of networking has many standards: HTTP, TCP, IP, FTP, TLS, etc., yet in a field so essential to all forms of programming as error handling, such simple things as style guides are rare. Errors are usually written entirely in natural language, perhaps accompanied by an error code devised according to a nonstandard schema that isn't formally defined. When we as programmers encounter a perplexing error, we often resort to searching it online, yet if the error is long natural language, this process is clunky and inconvenient. Further, if someone has asked online about the same error for a different version, the solutions may be identical, but if the error message has changed slightly, finding the solution may take hours! This situation is far from ideal, especially for errors, the things developers spend most of their time on! This specification is an effort to provide some system of order to the wilderness of error management.

## Specification

This system is defined using a formal specification (a definition of how it should work), the storage and versioning of which is really the only purpose of this repository. As it's designed to be unambiguous and technical, the specification is not beginner-friendly, and beginners who would like to further understand the system are advised to read the [Basic Introduction](basic-introduction). If you think some aspect of the specification is unclear, please speak up! The whole purpose of a specification is to be clear, and if someone can't understand it, it could probably be improved! If you're in this position, please open an issue on this repository! If you'd like to add something to the specification, or alter it otherwise, please see *Contributing* below.

## Implementations

This system is written primarily as a specification, and so is formally defined (see *Specification* above). For it to be of any use though, it needs to be *implemented* (coded) in real-world programming languages. There are certain features that implementations must implement, others that they should but don't have to, and others further that are optional. They are marked in the specification. The aim of this project is to have many implementations for many different languages so everyone can add structure to their errors, however there is a very large number of programming languages in existence, so if you're a developer using a language that doesn't yet have an implementation and you're interested in using this system, please build one! Implementors needing guidance on certain points are welcome to open discussions on this repository and issues/pull requests if they think some part of the specification could be clearer (see *Contributing* below).

## Standardization

This system is not yet a standard recognized by any official body. Nevertheless the author has tried to keep it as formal as possible to avoid ambiguity in a potentially complex topic. Further, far too many programs have already been built in far too many languages for this to feasibly ever become a true *standard*. However, the author certainly hopes that one day this system or something like it may be considered best practice!

## Contributing

If you want to contribute to the project, that's fantastic! Thank you so much, we really appreciate it! Please see our [Contribution Guidelines](contributing).

## Authors

- @arctic_hen7 (uses TypeScript, loves errors)

## License

MIT, see [`LICENSE`](license).

[basic-introduction]: ./protocol/basic-introduction.md
[license]: ./LICENSE
[contributing]: ./CONTRIBUTING.md
