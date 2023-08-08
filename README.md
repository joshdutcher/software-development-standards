
# Company Development Standards

**Written By:** Company Development Standards Committee

**Project Lead:** Josh Dutcher

**Project Contributors:**

---

## Introduction

[[Company]]'s Development Standards ("the Standards") are intended to be a living set of requirements, able to iterate and evolve as needed. This repository exists to provide tracking of the Standards documents as they change over time and to provide an interface familiar to developers with which to request changes and make updates.

Company as a technology company should be capable of quickly adapting to a changing environment, and as such the Standards are not intended to be a binding whitelist of the only acceptable languages, methods, or practices. Company is always open to considering emerging technologies or better ways of doing things if they are appropriate to our needs.

## Implementation

All software engineers must read and understand the sections of the Standards related to their roles, however it is strongly recommended to read *everything* contained herein. Understanding the Standards for *all* languages improves cross-team efficiency and broadens the skill set of each developer.

There are three facets to implementing these Standards:

1. Follow the Standards when developing new software and, if possible, when refactoring existing code. If the Standards have not been followed in a particular place, comment the code explaining why.
2. When peer reviewing pull requests, consider whether the Standards have been adhered to in the reviewed code.
3. Keep these Standards current and update them with appropriate changes as we move forward.

## Contents

**The bulk of Company's Development Standards are spelled out in the [Style Guide](style_guide.md) and [Guiding Principles](guiding_principles.md) documents. Team processes are described in the [Processes](processes.md) document.**

### [Style Guide](style_guide.md)

The **[Style Guide](style_guide.md)** describes the technical coding standards we should attempt to follow as we develop software applications at Company. This primarily consists of existing industry standards, and in a few cases some less formalized style guides were adapted for Company's use specifically.

### [Guiding Principles](guiding_principles.md)

The **[Guiding Principles](guiding_principles.md)** describe the more philosophical and high level approach that should be taken when designing a software application, including principles of Object Oriented Programming, Design Patterns, Testing, etc.

### [Processes](processes.md)

The **[Processes](processes.md)** describe workflow processes followed by Company software engineers including Agile methodology and Git workflow.

## Interacting with the Standards

### Branches

* The `master` branch should always be considered to be the current active state of the Standards, much like the `master` branch of any software project.
* The `drafts` branch is equivalent to the `develop` branch in a software project. It exists to aggregate proposed changes. Any changes to `master` should be merged only from the `drafts` branch.

### Submitting change requests/proposals

To submit an addition or change to the Standards, [create a new Issue](https://github.com/joshdutcher/Software-Development-Standards/issues) on the github repository. Be prepared to defend your proposal and convince other developers of its merit. If through discussion it is determined to adopt the change, submit a pull request to the github repo reflecting the change.

1. Check out a new branch named using the format `{name}-draft-{proposed change}`. For example: `$ git checkout -b josh-draft-php`.
1. Make your changes to the `.md` documents and push them up to the `Software-Development-Standards` repository.
1. Submit a pull request which will be reviewed and merged by the Standards Development Committee.
