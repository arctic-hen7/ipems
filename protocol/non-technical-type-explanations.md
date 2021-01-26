---
version: 0.1.0-rc1
---

# IPEMS Non-Technical Type Explanations

This document will define a series of non-technical explanations of the error types present in the IPEMS Default Paradigm (see [the default paradigm specification](./default-paradigm.md)). These are used in the *non-technical* encoding defined in that specification, and are provided to the custom function the developer provides, so that pre-made error explanations can be shown to the user quickly.

## Error Classes
### Caller
#### Parameter
This kind of error is the result of an internal programming mistake, and unfortunately there's nothing you can do to solve it right now. Please file a bug report so this problem can be fixed.

### Callee
#### Return
This kind of error is the result of an internal programming mistake, and unfortunately there's nothing you can do to solve it right now. Please file a bug report so this problem can be fixed.

### External
#### Return
This kind of error means an external resource that this program depends on (like a server or database) has provided invalid data. There's probably nothing you can do to fix this right now, but please file a bug report so this problem can be fixed. If you're also having problems with other programs, check your internet connection.

#### Network
This kind of error means the program was unable to connect to an external resource (like a server or database). This may be an issue on our end, but if you're having problems with other programs, you should check your internet connection. If you can connect to the internet, then you should file a bug report so this problem can be fixed.

### User
#### Input
This kind of error means you've entered invalid data in an input field somewhere. If further details have not been provided, you should file a bug report so this error message can be made more specific.

#### Authentication
This kind of error means data you entered caused an authentication issue. This likely means you entered your password or something similar incorrectly. If further details have not been provided, you should file a bug report so this error message can be made more specific.

### System
#### Permissions
This kind of error means the program needed to interact with a file or folder on your device, but it didn't have the necessary permissions to do so. If further details have not been provided, you should file a bug report so this error message can be made more specific.

## Special Classes
### Unknown
This kind of error means an unknown problem occurred. If no further information has been provided, you should file a bug report so this error message can be made more specific.

### Generic
This kind of error means an unknown problem occurred, but there should be further. If no further information has been provided, you should file a bug report so this error message can be made more specific.
