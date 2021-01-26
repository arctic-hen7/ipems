---
version: 0.1.0-rc1
---

# IPEMS Basic Introduction

This document is a basic introduction to the IPEMS specification, aimed at beginners to programming and error management. If you think any part of this document is too difficult to understand easily, please file an issue on this repository!

## What is IPEMS?

IPEMS (Internal Program Error Management System) is a system for managing errors in programming in a standard, universal way. When programming, it's practically guaranteed that you'll have to program some logic that throws an error (e.g. if your website can't connect to the database). Most of the time, when programmers do this, we write errors in *natural language*, the way we speak. This means error messages can be anything from describing the exact error, telling you what likely caused it, and how you can fix it, to "the program failed successfully".

If you're a beginner working with a new module, you'll almost inevitably encounter an error that makes no sense to you, and then you'll have to search it up, though sometimes finding a solution can take hours or longer because you're searching a lengthy error message! On the other hand, some programs have *error codes*, numbers that represent certain errors, but there's no universal standard for what the heck these mean! Error number 16 is very likely completely different for Program A than it is for Program B.

IPEMS aims to solve these problems by providing an *API* (application programming interface, fancy name for a collection of functions) to generate errors. These can then be transformed into various *encodings* (formats for the error). For example, IPEMS defines one encoding called *standard*, which provides a minimal error message, another one called *numeric*, which provides a universal error code, and another called *verbose*, which explains the error and recommends possible ways to solve it (but the developer has to tell it what those solutions are). 

## Why is there no code in this repository?

IPEMS is written as a *specification*. It's designed to be a universal protocol for error management, like HTTP. However, IPEMS needs to be used in real-world programming languages, and there are hundreds of commonly-used languages, thousands, if not more, of less popular ones. There is no way one team of developers could implement IPEMS in all these languages, so instead, we decided to write a guide on what the protocol should look like, regardless of what language it's implemented in. This means we can tell everyone what IPEMS is and how it works across *all programming languages*. It's up to *implementers* to actually write the code. Of course, the author of the specification is also a programmer, and they developed an [implementation for JavaScript](https://npm.im/ipems)!

If you want to create an implementation of IPEMS, it's actually a great first coding project, and it'll teach you real-world skills! If you're just doing it to learn, you could probably make it based on what you learn in this document, but if you want to do it really properly, you should read the [full specification](./specification.md). Warning: it's kind of programming legalese!

## How does IPEMS classify errors?

IPEMS requires every error to have an *error class*, *error type*, and *error severity*.

Error classes describe the generic nature of the error. Some examples are *User* (an error caused by a user of the program, like a comment that's too long) and *System* (an error in the system underneath the program, like if your program can't write to its own configuration files for some reason).

Error types describe the cause of the error in greater detail. The idea is that you should be able to understand the basic cause of an error just from its class and type. Each class has its own set of types, and there are no types without a parent class. Some examples of error types are *Input* on the *User* class (for some kind of invalid user input) and *Network* on the *External* class (for errors in connecting to an external resource, like a database). Every class has at least one type, the *Generic* type, for errors that clearly fit into the error class, but don't really sit well with any of the predefined types.

Error severities describe the impact of an error on the program as a whole. The main three are *Critical*, *Error*, and *Warning*. Note that severities are defined on a per-type basis, so some types may not support all severities. Here's where things get a bit complicated. Read carefully.

- Critical errors are for when the program has to terminate (end) *completely*. It can't even continue by just displaying an error message to the user, it has to completely shut down.
	- Pulling the power plug.
	- Imagine the program got shot in the head and is completely dead.
- Error errors (yeah...) are for when the program can't do what it's supposed to, and has to do something else, like just display an error message and wait for the user to do something about it.
	- You're installing some software, and it can't be installed in the default folder for some reason, so it suspends its normal activities (installing itself), and prompts you to specify an alternative folder to install into.
	- Imagine the program stepped on a landmine and is in hospital recovering.
- Warning errors are for when the program encounters a relatively minor error that doesn't stop it from doing its normal duties.
	- You unplugged your webcam, so a video conferencing app can't connect to it, so it falls back to audio-only. It still works, it's just not the best scenario.
	- Imagine the program got shot in the arm, but it can still fight on!

## What classes, types, and severities are there?

Well, IPEMS is pretty easy on this one. You as a user can define your own custom error classes, types, and severities, and if you're in a team, you might all decide on a *paradigm* (form or model in this context) of classes, types, and severities that you all want to use.

The IPEMS specification defines the *default paradigm*, which all implementations have to implement, and that should be enough for most users, but you can always extend it if you want! However, the numerical codes from the default paradigm are universal, so **any custom classes, types, or severities you define can't have the same codes**. You can skip to the end to see exactly what's in the default paradigm.

## How does IPEMS represent errors for humans?

It's all very well to define an error internally as an object, but what about when humans need to see the thing? Well, IPEMS has *encodings* for this. Like classes, types, and severities, encodings are defined in paradigms, and they're defined per-type, just like severities. *Encoders* are functions that transform the an IPEMS error into a human-readable error message. The default paradigm defines quite a few encodings, you'll find those at the end.

A really cool feature of IPEMS is the *default encoding*. Most of the time, programs won't need to use a specific error encoding, and all errors can use the same encoding. But if you're developing a module, or you're developing with a team, or heck if you just want to change the encoding or many error messages at once, you'd normally have to use something like `encodeAs(universalEncoding)`, where `universalEncoding` is some variable you've defined elsewhere. Rather than this messiness, IPEMS manages an internal default encoding for you! This means implementations must provide a function like `encodeAsDefault()`, which encodes as whatever that default encoding is. Then, you can just change the default encoding for the IPEMS class, or whatever it is in your language, and then all your errors will use that encoding (unless you've explicitly specified one of course)! This means you can use short and sharp error messages normally, and when you're debugging, you can use one line of code to switch to highly verbose error messages to help you!

## How do I specify the in-depth details of an error?

In most cases, you'll want to specify more details than just an error class, type, and severity. IPEMS allows this through *error type parameters*. These are special options unique to each error type, for example, the *Input* type on the *User* class has a parameter for specifying which input was invalid, and what the prerequisites are for it to be valid. Type parameters should cover most details of an error, but there are also *additional options* you can provide. There are three of them:

- Further details: any further details about the error (e.g. the exact error message your server sent)
- Solutions: an array of possible solutions to the error, you should add these if the error is reasonably complex
- Fallback: what your program is doing to work around an error, if anything

## What's in the default paradigm?

Here's a quick explanation of the classes, types, and severities defined in the default paradigm. Remember that all implementations have to provide this collection, so it should always be available to use!

### Classes and Types

- **Caller:**
	- **Parameter:** For errors where whatever called the function provided an invalid parameter (e.g. a user of your module provided a string instead of a number).
		- (REQUIRED) Name of the invalid parameter
		- (REQUIRED) Conditions for it to be valid
- **Callee:**
	- **Return:** For errors where another function you wrote, which your function depends on, returned invalid data. Don't use this for external resources like servers or databases. See below for using this with modules.
		- (REQUIRED) Name of the callee (function that failed)
		- (REQUIRED) Part of the return value that was invalid
		- (REQUIRED) Conditions for it to be valid
- **External:** (implementations are highly recommended to include these, but they technically don't have to, so they might not be available everywhere)
	- **Return:** For errors where an external resource (like a server or database) that your function depends on returned invalid data. Don't use this for internal functions you've written. See below for using this with modules.
		- (REQUIRED) Name of the external resource
		- (REQUIRED) Part of the return value that was invalid
		- (REQUIRED) Conditions for it to be valid
	- **Network:** For errors where your function can't connect to an external resource. Don't use this if a server for example tells you a resource isn't found, only use it if you can't connect to the server at all. This usually means the user is having connection problems.
		- (REQUIRED) Name of the external resource
- **User:** (doesn't support the *Critical* severity because errors coming from the user should always be handled gracefully, they should never be able to shut down a whole app)
	- **Input:** For errors where something the user inputted (e.g. their email into a form field on a website) was invalid (say they didn't include '@' in the email address).
		- (REQUIRED) Name of the input field where the user entered invalid data
		- (REQUIRED) Conditions for it to be valid
	- **Authentication:** For errors where the user entered a value that resulted in an authentication failure. This is usually an incorrect password, but it could also be a bad two-factor authentication token or something similar.
		- (REQUIRED) Name of the input field where the user entered invalid data
- **System:** (implementations are highly recommended to include these, but they technically don't have to, so they might not be available everywhere)
	- **Permissions:** For errors to do with file permissions. Use this if your program is denied read/write/access/etc. permissions to some file (e.g. the user has accidentally changed things so only root can access your program's configuration files).
		- (REQUIRED) File/directory that produced the error
		- (REQUIRED) What permissions are required
		- (OPTIONAL) Why each of those permissions is required
		- (REQUIRED) What permissions are missing

### What about modules?

The question here is this: if I use a module someone else wrote in my program, and it throws an error, is that classified as a *CalleeReturn* error because it's still part of my program, or an *ExternalReturn* error because it's not code I wrote? The answer is, you decide. Generally, you should decide which you're going with at the beginning of developing a program, and stick with that for consistency, but it's up to you to decide whether you want to use *External* or *Callee* for modules. We pass no judgement on the matter (yet).

If at some stage in the future it becomes glaringly obvious that one is clearly the worse choice, we'll recommend one, but that would be a breaking change to the IPEMS protocol (a very big change in other words).

### Severities

See *How does IPEMS classify errors?* above. That goes through the three severities of the default paradigm: *Critical*, *Error*, and *Warning*.

### Encodings

- **Full:** For when you want to convey the full meaning of the error, complete with additional options and type parameters, as a string. This uses the short encoding, followed by all the options you provided as a stringified JSON object.
- **Standard:** For when you want a brief and to the point error message. This doesn't include any of the additional options, but bases the generated message off the type parameters. It also includes short form.
- **Short:** `[ClassName][TypeName][SeverityName]`, e.g. `CallerParameterCritical` or `UserAuthenticationWarning`.
- **Numerical:**(implementations are highly recommended to implement this encoding, but they don't have to, so this might not be available everywhere) For when you want a numerical code. The severity is attached to that as `-[Severity code]`. See the [codes list](./default-paradigm.md#Specification) in the default paradigm for what the codes are for each class/type/severity.
- **Verbose:**(implementations are highly recommended to implement this encoding, but they don't have to, so this might not be available everywhere) For when you want a highly detailed error message explaining everything known about the error. This includes short form and a message composed from type parameters and all additional options. This may span multiple sentences, and potentially could have extra formatting (like dot points).
- **Non-technical:** (this encoding is optional, so many implementations may not implement it) For when you want to display an error message to a non-programmer, say because your website crashed. This requires you to provide a function to string together a few pieces of information: the short form, numerical form, and a non-technical type explanation. The IPEMS protocol advises what these explanations could be, but they may vary between implementations.
 
## How stable is IPEMS?

Well, right now, not that stable. The protocol is still in beta (0.x.x), and we'll likely change quite a few things before 1.0. Right now, we're hoping to have IPEMS 1.0 finalised by around June 2021. Until then, the IPEMS specification is in flux, and large changes may be introduced regularly. However, IPEMS *implementations*, the things you'll actually use as a developer, may be considered stable before the specification reaches 1.0, or unstable after, you'll have to check the documentation of the implementation you're using.

## How can I help?

If you'd like to help out with the project, you can open issues for any errors you notice in the IPEMS specification, and you might even want to follow our [Contributing Guidelines](./CONTRIBUTING.md) to fix a problem yourself! You could also create an implementation of IPEMS in your language, that's the best way to help IPEMS grow!
