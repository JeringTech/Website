---
mimo_pageDescription: This article focuses on the use of semantic versioning (semver) in practice; it journeys through a software development lifecycle, providing examples of how semver should be used in each phase - this article describes the semantic versioning lifecycle.
mimo_pageTitle: The Semantic Versioning Lifecycle
mimo_pageID: the-semantic-versioning-lifecycle
mimo_date: Mar 3, 2018
mimo_shareOnFacebook: true
mimo_shareOnTwitter:
    hashtags: semver
    via: JeringTech
---

<!--
#region pre-release-phase-2
3.0.0
- Fixed bugs.
- Software finally production ready.

3.0.0-beta.2
- Fixed bugs.
- Made backward compatible changes.

3.0.0-beta.1
- Improved performance.
- Made backward incompatible changes.

3.0.0-beta.0
- Completed feature F.
- All features complete but software not been rigorously tested and optimized.

3.0.0-alpha.1
- Completed feature E.
- Made backward compatible changes.

3.0.0-alpha.0
- Removed feature A.
- Completed feature D.
- Made backward incompatible changes.
#endregion

#region maintenance-phase
2.1.0
- Marked feature A as deprecated.

2.0.0
- Fixed backward incompatible bugs

1.2.0
- Made backward compatible, non-bug-fix changes to private code.

1.1.0
- Added public API functionality.

1.0.1
- Made backward compatible bug fixes.

1.0.0
- Fixed bugs, improved performance.
- Made backward incompatible changes.
- Software finally production ready.
#endregion

#region pre-release-phase-1
1.0.0-beta.1
- Fixed bugs, improved performance.
- Made backward incompatible changes.

1.0.0-beta.0
- Completed feature C.
- All features complete but software not rigorously tested and optimized.

1.0.0-alpha.2
- Completed feature B.
- Made backward incompatible changes.

1.0.0-alpha.1
- Completed feature A.
- Made backward compatible changes.

1.0.0-alpha.0
- Architectural plans have stabilized and features have been locked but several features are incomplete.
- Made backward incompatible changes.
#endregion

#region initial-development-phase
0.3.0 
- Made backward incompatible changes.

0.2.0 
- Made backward compatible changes.

0.1.0 
- Initial release.
#endregion
-->

Semantic versioning (semver) is a system for versioning software. By defining a standard way to version software, semver facilitates software interoperability. The full what, why, and a detailed specification for 
semver can be found at [semver.org](https://semver.org). This article focuses on the use of semver in practice; it journeys through a software development lifecycle, providing examples of how semver should be used in 
each phase - this article describes *the semantic versioning lifecycle*.

## Terminology
Before delving into the semver lifecycle, some terms must be defined:

| Term | Description |
| ---- | :---------- |
| Public&nbsp;API | The *public API (public application programming interface)* of a piece of software refers to the portion of its surface area that is publically exposed for programmatic access. For example, the public API of a web service could refer to a REST API, the public API of a library could refer to its public members, and the public API of a command line application could refer to the list of commands that it accepts. |  
| Private&nbsp;code | The *private code* of a piece of software refers to code that isn't publically exposed. For example, the private code of a web service could include a repository layer, the private code of a library could refer to its private classes and members, and the private code of a command line application could include business logic. |
| Version | A *version* is a snapshot of a piece of software. |
| Backward&nbsp;compatible | *Backward compatible* is an adjective for versions. A version is backward compatible if its public API is equal to or a superset of its preceding version's public API. In other words, a version is backward compatible if it can be used as a substitute for its preceding version. For example, a version that only adds public API features is backward compatible, while a version that removes or changes public API features isn't. | 
| Version&nbsp;Number | A *version number* is an identifier for a version. Version numbers are strings of the form `major.minor.patch`<wbr>`[-pre_release`<wbr>`_identifiers]`<wbr>`[+build_metadata]`, where `major`, `minor` and `patch` are integers, and both `pre_release`<wbr>`_identifiers` and `build_metadata` are series of dot-separated strings, where each string contains only characters in the regex character set `[0-9A-Za-z\-]`. |
| Patch version | *Patch version* refers to the `patch` segment of a version number. |
| Minor version | *Minor version* refers to the `minor` segment of a version number. | 
| Major version | *Major version* refers to the `major` segment of a version number. |
| Release | A *release* is a production-ready version. |
| Pre-release | A *pre-release* is a non-production-ready version that closely precedes a release. |

@{"type": "warning"}
! Semver can only be used to version software that declares a public API, such as a library. Examples of software that do not declare a public API include many games and websites. 
! These kinds of software are typically standalone end-products that are not programmatically accessible. For such software, it's impossible to determine backward compatibility
! since 2 or more interdependent systems are required for compatibility to be determined (a standalone system has nothing to be compatible or incompatible with). Not being able to determine backward
! compatibility makes semver impossible to apply - alternative versioning systems should be used for such software.

! The build metadata segment of a version number has no significance in the semver lifecycle and isn't discussed in this article. 

## Initial Development Phase
! This article's focus is on the use of semver in practice. To that end, it journeys through a simplified software development lifecycle. Don't worry about the lifecycle's specifics like its phase's names. Getting the gist of 
! what each phase covers is enough to derive value from this article.

### Description
A piece of software is in this phase just after conception; architectural plans and feature requirements are typically in flux, development is rapid and the software's public API is very unstable. 

### Semver in this Phase
#### Rules
Semver suggests beginning this phase with the version number `0.1.0`. During this phase, each subsequent version number should only have its minor version incremented, regardless
of whether or not changes are backward compatible. For example, at the end of this phase, the simplified changelog of a piece of software might look like this:

+{ 
    "sourceUri": "./the-semantic-versioning-lifecycle.md",
    "clippings": [{"region": "initial-development-phase"}]
}

! Patch version can be incremented in this phase, however, semver [recommends](https://semver.org/#how-should-i-deal-with-revisions-in-the-0yz-initial-development-phase) only incrementing minor version for simplicity's 
! sake.

<!--
> [!alert-note]
> A typical software project has a set of files that serve as scaffolding; this set of files includes tests, continuous integration scripts, and configuration files. Often, changes to such files have 
> no effect on the software that is actually distributed; such changes alone do not merit new versions in this or any phase.
-->

#### Rationale
The reasons for the above-mentioned rules are as follows:
- Keeping major version at 0 lets potential consumers know that a piece of software is likely to undergo significant changes and isn't ready for use in production.
- The simplicity of only incrementing minor version facilitates quick iterations without compromising on the availability of an ordered history.

## Pre-Release Phase

### Description
A piece of software is in this phase when it isn't production ready, but is moving toward a release (a production-ready version); like in the initial development phase, development is rapid and 
the software's public API is very unstable. 

! Unfortunately, there is no standard way to decide when to advance from the initial development phase to this phase; some organizations use testing stages to demarcate these phases while 
! others use more arbitrary measures. One simple approach is to consider a piece of software to be in the pre-release phase once architectural plans are stable and features are locked. Similarly, there is no standard way to 
! demarcate this phase's sub-phases (alpha, beta, release candidate etc). A simple approach would be to use the following markers:
! - A piece of software is in the alpha sub-phase if it's still feature incomplete (feature locked but some features have not been fully implemented). 
! - A piece of software is in the beta sub-phase if it's feature complete but not yet rigorously tested and optimized.

! This phase can occur after the initial development phase or after a [maintenance phase](#maintenance-phase). After the initial development phase, a piece of software would 
! be working its way toward the `1.0.0` version - its first release. After a maintenance phase, a piece of software could be working toward any release, for example, `2.0.0` or `3.0.0`.

### Semver in this Phase
#### Rules
During this phase, semver specifies that version numbers should be the version number of the target release followed by a `-` and dot-separated pre-release identifiers, 
for example, `1.0.0-alpha.1`. Each subsequent version number should only have its pre-release identifiers incremented, regardless of whether or not changes are backward compatible. For example, at the end of this phase, 
the simplified changelog of a piece of software might look like this:

+{ 
    "sourceUri": "./the-semantic-versioning-lifecycle.md",
    "clippings": [{"region": "pre-release-phase-1", "afterContent": ""}, 
        {"region": "initial-development-phase"}]
}

#### Rationale
The reasons for the above-mentioned rules are as follows:
- Appending pre-release identifiers lets potential consumers know that a piece of software isn't ready for use in production. 
- The simplicity of only incrementing the pre-release identifiers facilitates quick iterations without compromising on the availability of an ordered history.

## Maintenance Phase

### Description
A piece of software is in this phase when it's production ready; typically, other software have begun depending on it in production, and as a result, backward compatibility has started to matter.

### Semver in this Phase
#### Rules
This phase begins with the version number of the release that the preceding pre-release phase was working toward, for example, `1.0.0`. During this phase, semver specifies that version numbers 
should be incremented in the following manner:
- If a version adds only backward compatible bug fixes, its version number must be the preceding version number with its patch version incremented.
- If a version adds only backward compatible changes and at least one of these changes isn't a bug fix, its version number must be the preceding version number with its minor version incremented and its patch version set 
to 0. Examples of backward compatible, non-bug-fix changes include:
  - Adding public API functionality, for example, adding new endpoints to a REST API.
  - Making backward compatible, non-bug-fix changes to private code, for example, optimizing a private method in a library. 
  - Marking public API functionality as deprecated.
- If a version makes **any** backward incompatible changes, its version number must be the preceding version number with its major version incremented, minor version set to 0, and patch version set to 0.

In short, if a version:

| Adds only Backward Compatible Changes | Adds only Bug Fixes | Action |
| ------------------------------------- | ------------------- | ------ |
| true | true | Increment patch version. |
| true | false | Increment minor version, set patch version to 0. |
| false | true | Increment major version, set minor version and patch version to 0. |
| false | false | Increment major version, set minor version and patch version to 0.  |

! Major version increments are often associated with significant changes and rewrites. Incrementing major version even for small, backward incompatible changes might seem unusual. 
! Moreover, doing so could cause major version to balloon, causing versions to end up with version numbers like `42.0.0`. That said, incrementing major version for every backward incompatible change in this phase is a
! critical rule, if it isn't followed,
! consumers in production could break because of unexpected backward incompatible changes.
! 
! Semver's FAQ
! [mentions](https://semver.org/#if-even-the-tiniest-backwards-incompatible-changes-to-the-public-api-require-a-major-version-bump-wont-i-end-up-at-version-4200-very-rapidly) 
! this issue. It suggests proper planning to minimize backward incompatible changes in this phase. Aside from proper planning, these practices could help:
! 
! - Avoid rushing into this phase. Whether or not a piece of software is production ready is subjective, not least because 
! software can almost always be improved upon. Setting a high bar for production readiness and staying in the pre-release phase for longer would allow for more kinks to be ironed 
! out before this phase. An especially prudent developer might wait until consumers begin expressing interest in using the software in production before advancing to this phase.
! 
! - Batch backward incompatible changes. For example, [Angular](https://github.com/angular/angular/blob/master/CHANGELOG.md) batches backward incompatible changes, 
! releasing new major versions only once every few months. This would mean that backward incompatible bug fixes 
! could take longer to reach consumers, so a release cycle must be carefully planned.

At the end of a maintenance phase, the simplified changelog of a piece of software might look like this:

+{ 
    "sourceUri": "./the-semantic-versioning-lifecycle.md",
    "clippings": [{"region": "maintenance-phase", "afterContent": ""}, 
        {"region": "pre-release-phase-1", "afterContent": ""}, 
        {"region": "initial-development-phase"}]
}

#### Rationale
The reasons for the above-mentioned rules are as follows:

- These rules ensure that whether or not a version is backward compatible can be deduced from its version number. This 
enables semver to facilitate software interoperability, for example, consumers can configure package managers to automatically upgrade dependencies to the latest compatible versions. 

- These rules also ensure that whether or not a version contains only backward incompatible bug fixes can be deduced from its
version number. This facilitates software interoperability as well, for example, consumers can configure package managers to only install backward compatible bug fixes.

## Coming Full Circle
At some point in a maintenance phase, the need for a rewrite might arise. A rewrite takes the project back to the pre-release phase. For example, after a rewrite, the simplified changelog of a piece 
of software might look like this:

+{ 
    "sourceUri": "./the-semantic-versioning-lifecycle.md",
    "clippings": [{"region": "pre-release-phase-2", "afterContent": ""},
        {"region": "maintenance-phase", "afterContent": ""},
        {"region": "pre-release-phase-1", "afterContent": ""},
        {"region": "initial-development-phase"}]
}

! A pre-release phase can take place in parallel with a maintenance phase, with each phase occurring on a different version control branch. This would allow bug fixes to be released even while a pre-release phase is underway.

## Conclusion
While this article describes the advantages of *the semantic versioning lifecycle*, semver should not be taken as dogma. Ultimately, when versioning a piece of 
software, its circumstances must be taken into account. Thanks for reading this article, feel free to share tips or to point out any mistakes in the comments below!