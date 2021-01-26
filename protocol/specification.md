---
author: arctic_hen7
version: 0.1.0-rc1
---

# Internal Program Error Management System Specification

## Abstract

This document will describe a system of error management to simplify and make more convenient and universal the management of errors in programming. HTTP status codes are a universal method of conveying errors in a network transaction, and yet there is no equivalent in programming. The Internal Program Error Management System (IPEMS) aims to address this.

## Specification

1. **Definitions:**
	1. **RFC 2119:** The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).
	2. **Further keywords:**
		1. **Function:** The keyword 'Function' is to be interpreted to refer to the function, method, section of a program or the like that generated an error.
		2. **Acceptable Operational Parameters (AOPs):** The key phrase 'Acceptable Operational Parameters' or its corresponding acronym 'AOPs' is to be interpreted to mean the parameters within which a program can still perform its primary tasks.
			1. A program is still operating within AOPs if it can continue its primary tasks via a fallback of some kind, for example temporarily writing to memory if no file write access is available.
			2. A program is operating *outside* AOPs if it can no longer perform its primary tasks. For example, if a program must display an error message to a user asking them to manually edit a configuration file due to a corruption, it can no longer carry out its primary tasks, as it must wait for user action first. While it's doing so, it is operating outside AOPs.
			3. The definition of a program's *primary tasks* is mostly arbitrary. However, if one is developing a module, the module may be considered to be operating outside AOPs even when a program that uses it is still operating within AOPs. The two have differing primary tasks, and the failure for example of a module that simply formats text does not necessarily mean every program that depends on it will fail in the same situation (larger programs may have a fallback for example).
2. **Error Processing Control:** Implementations MUST by default, on the initialization of a new IPEMS error instance, create and return a language-specific error constructor, method, or the like. The user MUST be able to control whether they throw that error, return it gracefully, transmit it over the network, or take any other action. Implementations MUST NOT lock users into any one of these choices or the like.
	1. Implementations SHOULD, however, provide an option to allow users to easily extract a string error message rather than a fully-fledged native error instance. This option MUST NOT be the default though.
	2. Implementations MAY provide the user with the ability to provide a custom constructor, method, or the like in the place of a native error instance.
3. **Error formation:** All standard IPEMS errors MUST be composed of an *error class*, an *error type*, and an *error severity*. Special classes such as `Generic` and `Unknown` are exempt from this rule.
	1. **Error classes:** Error classes specify a general cause of the error in question. These are comparable to the classes in HTTP status codes (1xx, 2xx, 3xx, 4xx, and 5xx).
	2. **Error types:** Every error class MUST have at least one type. Types specify a more specific type of error. It SHOULD be possible to understand the basic cause of an error by simply knowing its error class and type. Types MUST be specific to classes, there are no global types. Each type takes a series of unique parameters that SHOULD explain the exact cause of the error. In addition to a stack trace, they SHOULD provide all the necessary information for debugging an error.
		1. **Generic type:** All error classes SHOULD have a `Generic` type in case none of the provided types fit the error in question. Implementations SHOULD provide this on all their default error classes, however custom user-created classes SHOULD NOT be forced into having a `Generic` type; users SHOULD be able to add one if they wish though. `Generic` types MUST NOT take any parameters.
	3. **Error severities:** Each error type MUST support at least one severity. Severities MUST be defined per-type. The severity of an error indicates what impact it has on the broader program. Note that IPEMS is designed as an *error* management framework, not a framework to manage successful responses. Thus, implementations and users MUST NOT add severities that do not indicate a problem (for example some equivalent to HTTP's 2xx status code class).
		1. The severity of an error MUST be considered relative to the program in which it occurs. An error inside a module may be critically severe from the perspective of that module, and should be generated as such, however from the perspective of a broader program using the module, it may be minor.
	4. **Paradigms:** A paradigm is a collection of error classes, types, and severities all defined together.
	5. **Definition of error classes, types, and severities:** Implementations MUST provide users with the ability to define their own error classes, types, and severities. Implementations MAY provide their own custom paradigms. Implementations MUST provide the default paradigm (see [default paradigm specification](default-paradigm)).
4. **Encodings:** Errors returned by implementations MUST be instances of an IPEMS error class or the like, and MUST provide methods to encode them according to an encoding. Encodings MUST be defined per-type, though all types in a paradigm SHOULD support the same encodings except in cases where this is not possible. All paradigms SHOULD support an encoding that includes all data about an error, including the parameters to its type, in programmatic form (for example JSON).
	1. **Default encoding:** In most cases, nearly all errors in a program can be encoded in the same way, however users of the program need to be able to change this encoding to aid their debugging (otherwise encodings are mostly pointless). To this end, implementations SHOULD provide a mechanism to encode as a default, however they MUST provide the ability for users to change this default. Further, modules using IPEMS SHOULD export a function, method, or the like to change the default encoding of the module.
5. **Defaults:** IPEMS encourages verbose and self-documenting code when describing errors, so implementations SHOULD NOT provide a mechanism to set a default error type on a class, or a default severity on a type. This would likely result in developers accidentally using improper types or severities. Default encodings are exempt from this rule.
6. **Options for all types:** Implementations MUST/SHOULD/MAY allow the following additional options on all error types (including those a user defines). If one or more of these types is not supported by an implementation, that implementation MUST NOT allow parameters of the same names on error types it or the user defines. Otherwise, confusion may arise between implementations as to what a particular parameter does. Unless otherwise stated, all these options MUST be optional for the user.
	1. **Further details:** (REQUIRED)
		1. An unrestricted (freeform) parameter for any further details about the error.
		2. This might be used for example when an error in a transaction with a server occurs to encapsulate the error message the server sent back.
		3. Error types, their parameters, and stack traces are sometimes not enough, and the requirements of additional details are so varied between programs, languages, and each error that this option MUST be freeform (though implementations MAY force the user to provide only a specific type if needed, like a string).
	2. **Solutions:** (RECOMMENDED)
		1. An array of possible solutions to an error.
		2. For simple errors, error types, their parameters, and stack traces are usually enough to solve an error. However, for more complex errors, developers SHOULD provide solutions to assist other developers. Module developers in particular SHOULD provide solutions to as many errors as possible to provide help for beginners, who may use their modules.
	3. **Fallback:** (RECOMMENDED)
		1. An action the program will take to attempt to bypass the error in question. For example, a video conferencing program that's been denied camera access might fallback to microphone only.
		2.  The action specified here can be within AOPs or outside them. Implementations MAY allow the user to specify which one for further verbosity and clarity.
		3.  Users SHOULD provide details of a fallback if one will be taken by their program.

[default-paradigm]: ./default-paradigm.md

\newpage
