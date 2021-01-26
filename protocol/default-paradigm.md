---
author: arctic_hen7
version: 0.1.0-rc1
header-includes:
  - \usepackage{enumitem}
  - \renewlist{enumerate}{enumerate}{20}
---

# IPEMS Default Paradigm Specification

## Abstract

This document will describe the default paradigm that implementations of the IPEMS protocol are required to implement (see the [full specification](specification), section 3.5, *Definition of error classes, types, and severities*).

## Specification

1. **Definitions:**
	1. **RFC 2119:** The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).
	2. **Further keywords:**
		1. **Function:** The keyword 'Function' is to be interpreted to refer to the function, method, section of a program or the like that generated an error.
		2. **Acceptable Operational Parameters (AOPs):** The key phrase 'Acceptable Operational Parameters' or its corresponding acronym 'AOPs' is to be interpreted to mean the parameters within which a program can still perform its primary tasks.
			1. A program is still operating within AOPs if it can continue its primary tasks via a fallback of some kind, for example temporarily writing to memory if no file write access is available.
			2. A program is operating *outside* AOPs if it can no longer perform its primary tasks. For example, if a program must display an error message to a user asking them to manually edit a configuration file due to a corruption, it can no longer carry out its primary tasks, as it must wait for user action first. While it's doing so, it is operating outside AOPs.
			3. The definition of a program's *primary tasks* is mostly arbitrary. However, if one is developing a module, the module may be considered to be operating outside AOPs even when a program that uses it is still operating within AOPs. The two have differing primary tasks, and the failure for example of a module that simply formats text does not necessarily mean every program that depends on it will fail in the same situation (larger programs may have a fallback for example).
2. **Error classes:**
	1. **Caller:** (REQUIRED)
		1. **When to use:** This class MUST be used for errors caused by the caller of the Function.
		2. **Types:**
			1. **Parameter:** (REQUIRED)
				1. **When to use:** This type MUST be used if the Function's caller provides an invalid parameter to the function. This includes if a parameter is not provided.
				2. **Parameters:**
					- (REQUIRED) Name of the invalid parameter
					- (REQUIRED) Prerequisites for the parameter to be valid
				3. **Severities:** This type MUST support all default paradigm severities.
				4. **Encodings:** This type MUST support all default paradigm encodings.
	2. **Callee:** (REQUIRED)
		1. **When to use:** This class MUST be used for errors caused by a function, method, or the like called by the Function. If the callee is an external service however, like a database or server API, the *External* type MUST be used instead if it's provided.
		2. **Types:**
			1. **Return:** (REQUIRED)
				1. **When to use:** This type MUST be used if one of the Function's callees produces an invalid return value.
				2. **Parameters:**
					- (REQUIRED) Name of the callee
					- (REQUIRED) Name of the part of the return value that's invalid (if the whole thing, `all` or the like)
					- (REQUIRED) Prerequisites for that part to be valid
				3. **Severities:** This type MUST support all default paradigm severities.
				4. **Encodings:** This type MUST support all default paradigm encodings.
	3. **External:** (RECOMMENDED)
		1. **When to use:** This class MUST be used for errors caused by an external service upon which the Function depends. For example, an external service would be a database, backend server API, or the like.
			1. **Modules:** Many programs utilize modules that the developer of the program did not write. In these scenarios, users MAY use either the *External* type or the *Callee* type. Of course, if *External* is not provided by the implementation, then *Callee* would be used, but otherwise it is the user's choice. For example, the user may consider modules they wrote as internal (using *Callee*), and modules they didn't write as external (using *External*). This specification will pass no recommendation on this matter (though this may change in future).
		2. **Types:**
			1. **Return:** (REQUIRED)
				1. **When to use:** This type MUST be used if an external resource the Function depends on produces an invalid return value.
				2. **Parameters:**
					- (REQUIRED) Name of the external resource
					- (REQUIRED) Name of the part of the return value that's invalid (if the whole thing, `all` or the like)
					- (REQUIRED) Prerequisites for that part to be valid
				3. **Severities:** This type MUST support all default paradigm severities.
				4. **Encodings:** This type MUST support all default paradigm encodings.
			2. **Network:** (REQUIRED)
				1. **When to use:** This type MUST be used if some kind of network error occurs while attempting to connect to an external resource the Function depends on. If the external resource returned invalid data (like a *404 Not Found* HTTP status code), then the *Return* type MUST be used. This type is only for errors in connecting to the external resource.
				2. **Parameters:**
					- (REQUIRED) Name of the external resource
				3. **Severities:** This type MUST support all default paradigm severities.
				4. **Encodings:** This type MUST support all default paradigm encodings.
				5. **Notes:** This type is very general, and the *Further details* option should also be provided with additional data specific to the error (see the [full specification](specification), section 6.1, *Further details*).
	4. **User:** (REQUIRED)
		1. **When to use:** This class MUST be used for errors caused by a user of the program, for example an invalid user input.
		2. **Types:**
			1. **Input:** (REQUIRED)
				1. **When to use:** This type MUST be used if a user input is invalid, for example if a comment the user enters is too long.
				2. **Parameters:**
					- (REQUIRED) Name of the invalid input
					- (REQUIRED) Prerequisites for the input to be valid
				3. **Severities:** This type MUST support all default paradigm severities except for the *Critical* severity, which it MUST NOT support. IPEMS encourages resilient error handling, and all user inputs SHOULD be treated as unknowns and handled accordingly, with no opportunity for them to cause a full program crash.
				4. **Encodings:** This type MUST support all default paradigm encodings.
			2. **Authentication:** (REQUIRED)
				1. **When to use:** This type MUST be used if a user input does not pass authentication logic, for example if the user entered an incorrect password.
				2. **Parameters:**
					- (REQUIRED) Name of the input that didn't pass authentication logic
				3. **Severities:** This type MUST support all default paradigm severities except for the *Critical* severity, which it MUST NOT support. IPEMS encourages resilient error handling, and all user inputs SHOULD be treated as unknowns and handled accordingly, with no opportunity for them to cause a full program crash. This type supports the *Error* severity for programs that for example might decrypt a file, and if the user enters an incorrect password, they can no longer operate within AOPs (decrypting the file).
				4. **Encodings:** This type MUST support all default paradigm encodings.
	5. **System:** (RECOMMENDED)
		1. **When to use:** This class MUST be used for errors caused by some part of the program's runtime failing, for example a hardware failure. However, in scenarios where this is due to some other cause (for example the user denying camera access), other classes SHOULD be considered (in that example, *User*).
			1. **Definition of *runtime*:** The definition of *the program's runtime* is open to some interpretation, and what constitutes a *System*-class error MUST be left to the user's discretion. However, failures in the underlying operating system, hardware, firmware, or engine interpreting/compiling the program SHOULD in most cases be considered to be of the *System* class.
		2. **Types:**
			1. **Permissions:** (REQUIRED)
				1. **When to use:** This type MUST be used if an error is encountered relating to filesystem permissions, for example missing write access to a configuration file.
				2. **Parameters:**
					- (REQUIRED) File/directory that produced the error
					- (REQUIRED) What permissions are required
					- (OPTIONAL) Why each of those permissions is required
					- (REQUIRED) What permissions are missing
				3. **Severities:** This type MUST support all default paradigm severities.
				4. **Encodings:** This type MUST support all default paradigm encodings.
3. **Severities:** (ALL REQUIRED)
	1. **Critical:** This severity MUST be used if the error in question prevents the program from continuing in any way, forcing a *complete* termination. If there is any way in which the program could continue, albeit if only showing an error until the user acts, this severity MUST NOT be used.
	2. **Error:** This severity MUST be used if the error in question prevents the program from continuing within AOPs. This severity MUST NOT be used if the error in question necessitates a complete program termination. Additionally, if the program can still continue within AOPs (for example via some kind of fallback), then this severity MUST NOT be used.
	3. **Warning:** This severity MUST be used if an error does not prevent the program from continuing within AOPs. For example, many user authentication errors where the user simply incorrectly entered their password would use this severity, as the user is prompted to try again and the program continues within AOPs. This severity MUST NOT be used if an error prevents the program from continuing within AOPs.
4. **Encodings:**
	1. **Full:** (REQUIRED)
		1. This encoding MUST be of the form `[ShortForm]([JSON Representation of Parameters])`.
		2. The short form used in this encoding MUST be that defined in this specification (see 4.3, *Short*).
		3. The JSON representation of parameters MUST include the exact user-provided values for all parameters provided to the relevant error type, as well as all additional options the user specified.
	2. **Standard:** (REQUIRED)
		1. This encoding MUST be of the form `[ShortForm]: [Minimal constructed sentence message from parameters]`.
		2. The short form used in this encoding MUST be that defined in this specification (see 4.3, *Short*).
		3. The minimal constructed message in this encoding MUST be based on the parameters to the relevant error type the user provides, though it MAY omit some of those parameters.
		4. This encoding SHOULD NOT include any additional options the user provided, given that the purpose of this encoding is a brief and minimal error message.
	3. **Short:** (REQUIRED)
		1. This encoding MUST be of the form `[Class][Type][Severity]`, where each element has its first letter capitalized (e.g. `CallerParameterCritical`).
		2. This encoding MUST NOT contain any information beyond that provided in 4.3.1 above.
	4. **Numerical:** (RECOMMENDED)
		1. This encoding MUST be of the form `[Class Code][Type Code]-[Severity Code]`.
		2. The codes for each element of this encoding MUST be the same as those defined in this specification.
		3. Implementations that provide additional classes, types, or severities MUST NOT use conflicting codes, and implementations SHOULD ensure that user-provided classes, types, or severities do not use conflicting codes.
	5. **Verbose:** (RECOMMENDED)
		1. Verbose form MUST be of the form `[ShortForm]: [Verbose, full-sentences message based off parameters]`.
		2. The short form used in this encoding MUST be that defined in this specification (see 4.3, *Short*).
		3. The message in this encoding MAY include multiple sentences, MAY span multiple lines, and MAY even include dot points or other formatting (however it MUST be renderable as plain text).
		4. This encoding MUST contain all information the user provided, including parameters to the relevant error type as well as additional options the user provided. Implementations MUST NOT omit any information in this encoding.
	6. **Non-technical:** (OPTIONAL)
		1. This encoding MUST be set by the user with a custom function that collates multiple elements.
		2. The user's custom function MUST have short form (see 4.3) and numerical form (if provided, 4.4) encodings provided to it, along with a non-technical explanation of the error type.
		3. The non-technical explanation SHOULD be reviewed by non-technical people before going into production, as programmers often think ordinary people know more about technology than they really do.
		4. The user MAY choose to use any, all, or none of the parameters provided to their custom function. In some scenarios, the user MAY choose to simply create their own error message entirely.
5. **Operations:** The following additional special mechanisms SHOULD be provided for user convenience.
	1. **Union:** This operation SHOULD be used if an error could be one of many types/severities. It could be likened to *or*.
	2. **Intersection**: This operation SHOULD be used if an error fits into multiple types/severities *simultaneously*. It could be likened to *and*.
6. **Special classes:** The following special classes SHOULD/MAY be provided for user convenience in certain situations. These classes MUST NOT support any types or severities. They MUST however support all encodings defined in this specification.
	1. **Unknown:** (RECOMMENDED) This special class MUST be used for errors whose class/severity cannot yet be ascertained. If the type cannot be ascertained, the *Generic* type MUST be used instead (see [full specification](specification), section 3.2.1, *Generic type*). If possible, this class SHOULD be avoided, and error generation should be deferred until further information can be ascertained.
	2. **Generic:** (OPTIONAL) This special class MAY be used in situations of incremental adoption of IPEMS, in which large numbers of errors need to be converted to IPEMS format. One of the largest hurdles here is deciding which class/type/severity each error falls in to. This special class MAY be used as an interim measure to defer that process to a later date, or to make it incremental, while enabling speedy adoption of the IPEMS system and its benefits. Implementations MAY log warnings when this class is used, however if they do so, they MUST provide an option for the user to silence those warnings (lest they be overwhelmed with potentially thousands of them).
7. **Numerical codes list:**
	1. **Classes and types:**
		- **Caller:** 1
			- **Generic:** 00
			- **Parameter:** 01
		- **Callee:** 2
			- **Generic:** 00
			- **Return:** 01
		- **External:** 3
			- **Generic:** 00
			- **Return:** 01
			- **Network:** 02
		- **User:** 4
			- **Generic:** 00
			- **Input:** 01
			- **Authentication** 02
		- **System:** 5
			- **Generic:** 00
			- **Permissions:** 01
	2. **Severities:**
		- **Critical:** 0
		- **Error:** 1
		- **Warning:** 2
	3. **Special classes:**
		- **Unknown:** 000-0
		- **Generic:** 050-0

[specification]: ./specification.md

\newpage
