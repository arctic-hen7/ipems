# Contributing

Every kind of contribution to this project is welcome, no contribution is too small! Please read our [Code of Conduct](code-of-conduct) before contributing.

## Kinds of Contributions

Here are a few kinds of contributions we're particularly looking for at the moment, but don't be worried if the contribution you want to make isn't on this list, please go ahead anyway! These are just some suggestions.

- **Implementations of the specification:** This is just a specification of what we think is a good idea. It needs to be actually implemented as code in many different languages to be viable! If you'd like to submit an implementation, please refer to the [Implementor's Guidelines](implementor-guidelines) for further details.
- **Fixing typos and grammar mistakes:** Though everything gets spellchecked, some grammar mistakes in particular will be missed. If you find something like a sentence that doesn't make sense (probably because it was written over lunch...), please let us know!
	- Note: this specification is written in American English, though the author is more familiar with British English, so British spellings such as `colour` rather than `color` should be pointed out as well.
- **Clarity fixes:** A technical specification can easily get out of hand in terms of complexity. The specification is supposed to be written in *unambiguous, technical language*. That means that any issues of ambiguity, where something isn't crystal clear, should be pointed out.
- **Simplifying things:** If you think something in the specification could be reworded for greater clarity or simplicity, then please let us know! The author has tried to write clearly, though the author has also been known to regularly make no sense whatsoever. In the *Basic Specification*, everything should be worded very simply and clearly; that document is targeted at beginners in programming and error handling. If you think something could be simpler or worded better, please let us know!
- **Features to add to the default paradigm:** The default paradigm is severely limited right now, and there are undoubtedly many kinds of errors that cannot yet be classified properly by it. If you think of an error class, type, severity, or encoding you'd like to see in the default paradigm, please let us know! Not all feature requests will be accepted, but all suggestions are welcome!
	- The non-technical error type explanations recommended by the default paradigm have been run by non-technical people, though they need to be as simple as possible. If they aren't understandable to everyone, they're pointless (okay, you'll need to know what a computer is, but you get the idea). If you think they can be simplified, please let us know!

## I Found an Issue, But I Don't Want to Contribute

Okay, that's fine too! Just open a new issue, and we'll get to fixing it as soon as we can! Please make sure you fill out our issue template, it's designed to help us. If you don't use it, that just makes our job much harder. Please be polite and considerate in discussions about issues, and follow our [Code of Conduct](code-of-conduct) at all times.

## How to Contribute

If you've never contributed to an open-source project before, that's fine! Below is a rough outline of how to contribute to this project for the first time. If you've never used Git (the version control system we use) or need a refresher, see [here](https://medium.com/@ashk3l/a-visual-introduction-to-git-9fdca5d3b43a).

1. Find an issue on the repository you're interested in fixing (issues marked as `good-first-issue` are great places to start). If you've found an issue that no-one else has reported, please open a new issue and fill out our issue template!
2. Fork this repository to your GitHub account. This will give you a copy of the repository on which you can make your changes.
3. Clone your repository to your local machine with `git clone https://github.com/<your-github-username>/ipems-protocol`.
4. Create a new branch for your alterations. We create a separate branch for each alteration, and then merge it to `main` (the primary branch). No commits should ever be made directly to `main`. You can do this with `git checkout -b <branch-name>`. This will also move you into that branch. Your branch name should be a brief description of the changes you're going to make (e.g. `feat/add-blah-class-to-default`).
5. Make the changes associated with the issue you're working on.
6. When you're done, commit your changes (you should do this for each part of your change) with `git add <paths-of-changed-files>` and `git commit -m "<commit message>"`. Your commit message should describe the changes you've made.
7. Push your changes to the copy of the repository you forked into your GitHub account with `git push origin <branch-name>`.
8. Open a pull request (request to add your code to ours) on the official repository and fill out the template. Add any questions you have and any further details beyond the template to the description. Your pull request does not have to be perfect, we'll review it and suggest any changes that need to be made. Everyone is a beginner at first!
9. Wait for your pull request to be reviewed by a maintainer. We'll try to get to it as quickly as possible!
10. Make changes the reviewing maintainer suggests.
11. Celebrate your merged pull request!

If we reject your pull request, don't be downcast, it's in no way a reflection on you personally! If we do this, it's probably because you've suggested a feature we aren't interested in. That doesn't mean we won't be interested in the future, and it certainly doesn't mean you shouldn't contribute again, please do! All pull requests are welcome, but not all will be merged into the main project.

## Markdown

This whole project is written in Markdown, so you'll need to be familiar with it for most changes! If you're new to Markdown, see [this](https://www.markdownguide.org/getting-started) simple introduction.

You can write Markdown in any text editor, but if you want a more fully-featured application, you can use a code editor like VS Code or Atom, or an application specialized for Markdown, like [Abricotine](https://abricotine.brrd.fr/).

## Help!

If you need help with contributing, open a discussion on this repository under the *Q&A* category, or send us an email at [ipems@protonmail.com](mailto:ipems@protonmail.com).

[code-of-conduct]: ./CODE_OF_CONDUCT.md
[implementor-guidelines]: ./implementors-guidelines.md
