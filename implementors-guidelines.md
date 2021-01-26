# IPEMS Implementor's Guidelines

So you want to make an implementation of IPEMS in your programming language? That's great! Seriously, thank you so much! Without implementations, IPEMS is just a useless specification. This document will outline most of what you need to know to build a great implementation!

## Where do I start?

Depends on how experienced you are. If you're an experienced programmer, you could probably dive straight into the [full specification](specification). For a more gentle introduction, you can check out the [basic introduction](basic-introduction). That goes through most of what you need to know, but you'll need to reference the full specification for a complete understanding.

## What features do I have to implement?

The IPEMS full specification uses the terms from [RFC 2119](https://tools.ietf.org/html/rfc2119), which defines a few terms like 'MUST', 'SHOULD', and 'MAY'. Anything marked as 'MUST' or 'REQUIRED', you have to implement. Anything marked as 'SHOULD' or 'RECOMMENDED', you are *highly* recommended to. Without these, your implementation will be very feature-barren. Finally, anything marked as 'MAY' or 'OPTIONAL' is up to you whether you implement it. It's completely your decision. 

## What if I don't implement a required feature?

Well, that's fine in beta (v0.x.x). But once you move your implementation into production, you have to implement all the required features at least. If you don't, then you haven't implemented the IPEMS specification, and you can't say you have. If you think a required feature should be recommended or optional instead, open an issue with the details and we'll consider making the change.

## How should I keep up-to-date when the specification changes?

Right now, the IPEMS specification is still in beta, but we expect to go to v1.0 around June 2021. Until then, you should be aware that there may be many breaking (major) changes to the specification. You don't have to update your implementation right away, but you need to explicitly state which version of the IPEMS specification your program implements.

## How can I get my implementation recognized?

Tell us about it! You should do that by opening an issue and asking us to list your implementation in our documentation. We'll have to review it first, and be warned that any features (required, recommended, or optional) that aren't implemented by your program will be listed. We use a compatibility table which explicitly states which features are supported and which ones aren't.

Please note that we won't list your implementation unless you ask us to. However, if your implementation becomes very popular and you don't ask, we may reach out to you and ask if we can list it. We won't list your implementation without your permission.

## I need help!

If you're having trouble with building your implementation, there are two main courses of action for you:

- If you think we should make our documentation clearer, open an issue
- If you need help implementing a feature, but our documentation is clear, open a discussion on this repository under the *Implementations* category. We're happy to have lengthy discussions about the intricacies of implementations there. You can also email us at [ipems@protonmail.com](mailto:ipems@protonmail.com).

## How should I license my implementation?

We prefer the MIT license, see [this](https://choosealicense.com/licenses/mit/) explanation of it. If you don't want to use that, then please research what license you're going to use carefully, and explicitly state it in your implementation. If you contact us asking us to list your implementation, we'll list your project's license as well.

## There's already an implementation in my language, but I think I can do better

By all means, go ahead! We're happy to list multiple implementations for a single language!

[specification]: ./protocol/specification.md
[basic-introduction]: ./protocol/basic-introduction.md
