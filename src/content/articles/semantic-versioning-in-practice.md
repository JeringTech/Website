---
mimo_pageDescription: A good grasp of semantic versioning (semver) is a must for most software developers. In this article, we explain in simple terms, everything you need to know to use semver effectively. 
mimo_pageTitle: Semantic Versioning in Practice
mimo_pageID: semantic-versioning-in-practice
mimo_date: Jan 15, 2019
mimo_shareOnFacebook: true
mimo_shareOnTwitter:
    hashtags: semver,semanticversioning,versioning,devops,programming,softwaredevelopment
    via: JeringTech
mimo_additionalFontPreloads:
  - /resources/open-sans-v15-latin-italic.woff2
---

<!-- 
    TODO improvements
    - Mention that the public API of a piece of software can be defined through its documentation. It doesn't need to rigidly conform to "all public members".
    - Version number precedence.
-->

<!--
#region pre-release-phase-2
3.0.0
- Fixed bugs.
- Software finally production-ready.

3.0.0-beta.2
- Improved performance.
- Made backward compatible changes.

3.0.0-beta.1
- Fixed bugs.
- Made backward incompatible changes.

3.0.0-beta.0
- Features complete. Architecture stabilized. But software needs rigorous testing and optimizing.
- Made backward compatible changes.

3.0.0-alpha.1
- Made backward compatible changes.

3.0.0-alpha.0
- Features locked but incomplete. Architecture hasn't stabilized.
- Made backward incompatible changes.
#endregion

#region production-phase-end-item
2.1.0
- Marked a feature as deprecated.
#endregion

#region production-phase-start-items
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
- Software finally production-ready.
#endregion

#region pre-release-phase-1-end-item
1.0.0-beta.1
- Fixed bugs, improved performance.
- Made backward incompatible changes.
#endregion

#region pre-release-phase-1-start-items
1.0.0-beta.0
- Features complete. Architecture stabilized. But software needs rigorous testing and optimizing.
- Made backward compatible changes.

1.0.0-alpha.1
- Made backward compatible changes.

1.0.0-alpha.0
- Features locked but incomplete. Architecture hasn't stabilized.
- Made backward incompatible changes.
#endregion

#region initial-development-phase-end-item
0.3.0 
- Made backward incompatible changes.
#endregion

#region initial-development-phase-start-items
0.2.0 
- Made backward compatible changes.

0.1.0 
- Initial release.
#endregion
-->

A good grasp of [semantic versioning](https://semver.org/) (semver) is a must for most software developers. In this article, we explain in simple terms, everything you need to 
know to use semver effectively. Also, we provide insights into our use of semver here at Jering.

## Why is a Good Grasp of Semver a Must for Most?

You might be wonder why a good grasp of semver is a *must* for most. Here's the thing - just about every major package manager mandates or 
prefers semver. The list includes:
- [Cargo](https://doc.rust-lang.org/cargo/reference/manifest.html#the-version-field) for rust
  > Cargo bakes in the concept of Semantic Versioning...
- [Nuget](https://docs.microsoft.com/en-us/dotnet/standard/library-guidance/versioning) for .Net 
  > CONSIDER using SemVer 2.0.0 to version your NuGet package...
- [NPM](https://docs.npmjs.com/about-semantic-versioning) for Javascript
  > Following the semantic versioning spec helps other developers who depend on your code understand the extent of changes in a 
  > given version, and adjust their own code if necessary...

A good grasp of semver is a *must* for *responsible versioning of software* in many ecosystems. Now that we've gotten
that out of the way, let's dive in.

## Terminology
We must first define key semver terms:
### Public API 
A piece of software's *public API* is the part of its surface area publically exposed for programmatic access. E.g., the *public API* of a:
- library refers to its public members
- command line application refers to the commands it accepts
- web service refers to a REST API
### Private Code
A piece of software's *private code* is the part of its code it doesn't publically expose. E.g., the *private code* of a:
- library refers to its private classes and members
- command line application could include business logic
- web service could include a repository layer
### Version 
A *version* is a snapshot of a piece of software.
### Backward Compatible 
The phrase *backward compatible* describes [version](#version)s. A version can be *backward compatible* with an earlier version. This is when its [public API](#public-api) is **equal to** 
or a **super-set** of the earlier version's public API. In short, a version is *backward compatible* with an earlier version if it can substitute for it. 
A version that:
- **only adds** public API features **is** *backward compatible* with its preceding version
- **removes and/or changes** public API features **isn't** *backward compatible* with its preceding version
  
### Version Number 
A *version number* is an identifier for a [version](#version). *Version numbers* are strings of the form `major.minor.patch[-pre_release_identifiers][+build_metadata]`. 
`major`, `minor` and `patch` are non-negative integers. `pre_release_identifiers` and `build_metadata` are series of dot-separated strings. 
Each string can only contain characters in the regex character set `[0-9A-Za-z\-]`.

### Patch Version 
*Patch version* refers to the `patch` segment of a [version number](#version-number).
### Minor Version 
*Minor version* refers to the `minor` segment of a [version number](#version-number). 
### Major Version 
*Major version* refers to the `major` segment of a [version number](#version-number).
### Release 
A *release* is a production-ready [version](#version).
### Pre-release 
A *pre-release* is a non-production-ready [version](#version) that closely precedes a [release](#release).

## What Software Can Semver Version?
Using semver to version the wrong software is an all too common source of frustration. 
Semver **[can't version](https://semver.org/#spec-item-1)** software that doesn't declare a [public API](#public-api). 

Software **that declares** a public API include libraries 
and command line applications. Software **that doesn't declare** a public API include many games and websites. 
Consider a blog; unlike a library, it has no public API. Other pieces of software cannot access it programmatically. As such, [version](#version)s of a blog can't be 
[backward compatible](#backward-compatible) or incompatible. As we'll [explain](#rules-2), semver [version number](#version-number)s depend on backward compatibility. 
Because of this dependence, semver can't version software like blogs.

## [Initial Development Phase](https://semver.org/#how-should-i-deal-with-revisions-in-the-0yz-initial-development-phase)
Now we've explained the basics, let's move on to the use of semver. Semver has 3 phases: 
*initial development*, [*pre-release*](#pre-release-phase), and [*production*](#production-phase). We begin with the initial 
development phase.

### Description
A piece of software is in this phase just after conception. 
Significant changes to its [public API](#public-api) are likely and it might be unstable. 
It's a long way from production-ready.

### Semver in this Phase
#### Rules
The first [version number](#version-number) in this phase must be `0.1.0`. Only increment [minor version](#minor-version) for subsequent version numbers. Do this regardless of whether changes 
are [backward compatible](#backward-compatible). At the end of this phase, the [version](#version) history of a piece of software looks like this:

@{
    "highlightLineRanges": [{"startLineNumber": 1, "endLineNumber": 1}, 
        {"startLineNumber": 4, "endLineNumber": 4},
        {"startLineNumber": 7, "endLineNumber": 7}]
}
+{ 
    "clippings": [{"region": "initial-development-phase-end-item", "afterContent": ""},
        {"region": "initial-development-phase-start-items"}] 
}

#### Rationale
- Keeping [major version](#major-version) at 0 lets consumers know software is a long way from production-ready.
- Incrementing only minor version is simple, and so, convenient for quick iterations. Also, doing so does not compromise on an ordered history.

## [Pre-Release Phase](https://semver.org/#spec-item-9)

### Description
A piece of software is in this phase when it's working toward a [release](#release). 
Changes to its [public API](#public-api) are likely and it might be unstable. It isn't production-ready. 
[Version](#version)s in this phase are [pre-release](#pre-release)s.  

This phase can occur after the [initial development phase](#initial-development-phase) or a [production phase](#production-phase). After the 
initial development phase, a piece of software works toward the `1.0.0` release. After a production phase, it could 
work toward any release. E.g., `2.0.0`, `3.21.0`, or `4.5.1`.

! You might have noticed overlap between this and the initial development phases. We'll [expand on](#demarcating-the-initial-development-and-pre-release-phases) this issue later.

### Semver in this Phase
#### Rules
[Version number](#version-number)s in this phase must be of the form `major.minor.patch-pre_release_identifiers`. 
`major.minor.patch` must be the version number of the release the software is working toward. E.g., `1.0.0-alpha.0`, `3.1.3-beta.3` or `4.5.1-rc.1`. 
Only increment pre-release identifiers for each subsequent version number. 
Do this regardless of whether changes are [backward compatible](#backward-compatible). At the end of this phase, the version history of a piece of software looks like this:

@{
    "highlightLineRanges": [{"startLineNumber": 1, "endLineNumber": 1}, 
        {"startLineNumber": 5, "endLineNumber": 5},
        {"startLineNumber": 9, "endLineNumber": 9},
        {"startLineNumber": 12, "endLineNumber": 12}]
}
+{ 
    "clippings": [{"region": "pre-release-phase-1-end-item", "afterContent": ""},
        {"region": "pre-release-phase-1-start-items", "afterContent": ""}, 
        {"region": "initial-development-phase-end-item", "afterContent": "\n..."}]
}

! We didn't provide much information on `pre_release_identifiers`. This is because semver doesn't specify standard values for it. We'll [touch on](#pre-release-sub-phases) this issue later.

<!-- TODO ! What does it mean to *increment pre-release identifiers*? We'll clarify this in the section on [version number precedence](#version-number-precedence). -->

#### Rationale
- Appending pre-release identifiers lets consumers know software isn't production-ready. 
Also, it lets consumers know software has attained some stability relative to the initial development phase.
- Incrementing only pre-release identifiers is simple, and so, convenient for quick iterations. Also, doing so does not compromise on an ordered history.

### Notes
#### Demarcating the Initial Development and Pre-Release Phases
In the [initial development phase](#initial-development-phase), software is likely to see [public API](#public-api) changes, is unstable, and isn't production-ready. Also, software in that phase is technically working toward a 
[release](#release). All this is pretty much the same for software in the [pre-release phase](#pre-release-phase). So when does software move from the initial development phase to the pre-release phase? 
Semver doesn't specify a clear marker.

It's a good idea to **define** and **articulate** a marker so consumers know what to expect. Here at Jering:
- A piece of our software moves to the pre-release phase once we **fix the set of features** (lock features) for its upcoming release.

Apart from being a good marker, we find that locking features helps us complete projects.

#### Pre-Release Sub-Phases
Semver doesn't specify pre-release sub-phases. Developers often use the alpha, beta, and rc (release candidate) sub-phase **names**. But these do not represent **standardized** sub-phases. 
The lack of standardization has caused trouble. In [Angular 2+](https://angular.io/)'s early days, rc [version](#version)s received many [backward incompatible](#backward-incompatible) changes. This caused a 
[backlash](https://www.reddit.com/r/Angular2/comments/4x23ae/eli5_why_are_new_featuresmajor_changes_being/) from developers expecting stable public APIs.

It's a good idea to **define** sub-phases and **articulate** their definitions. Here at Jering, a piece of our software is in the:
- Alpha sub-phase while its **features are incomplete** and its **architecture hasn't stabilized**.
- Beta sub-phase when it's past alpha but still **needs rigorous testing and optimizing**.

We do not use the rc sub-phase.  

@{"type": "warning"}
! Software development can be hard to predict. So it isn't **always** possible to stick to these definitions. Nonetheless, they help manage our consumer's expectations.

What should `pre_release_identifiers` be for each sub-phase? Semver doesn't specify. Here at Jering, we use the format `sub_phase_name.iteration`. 
`iteration` is an integer that starts from 0 and increments for each subsequent version number. E.g., `alpha.0`, `alpha.1`, `beta.0`, `beta.1`.

## Production Phase

### Description
A piece of software is in this phase when it's production-ready. It likely has consumers in production, so [backward compatibility](#backward-compatible) 
matters. [Version](#version)s in this phase are [release](#release)s. 

### Semver in this Phase
#### Rules
[Version number](#version-number)s in this phase must be of the form `major.minor.patch`. `major` must be larger than 0. E.g., `1.0.0` or `6.7.2`. Increment version numbers according to the following rules:
- If a version makes only backward compatible **bug fixes**, increment [patch version](#patch-version).
- If a version makes only backward compatible **changes** and at least one of them **isn't a bug fix**, increment [minor version](#minor-version) and set patch version to 0.
- If a version makes **any** backward incompatible changes, increment [major version](#major-version) and set both minor and patch version to 0.

In short, if a version:

| Makes only Backward Compatible Changes | Makes only Bug Fixes | Action |
| ------------------------------------- | ------------------- | ------ |
| true | true | Increment patch version. |
| true | false | Increment minor version, set patch version to 0. |
| false | true/false | Increment major version, set minor version and patch version to 0. |

At the end of this phase, the version history of a piece of software looks like this:

@{
    "highlightLineRanges": [{"startLineNumber": 1, "endLineNumber": 1}, 
        {"startLineNumber": 4, "endLineNumber": 4},
        {"startLineNumber": 7, "endLineNumber": 7},
        {"startLineNumber": 10, "endLineNumber": 10},
        {"startLineNumber": 13, "endLineNumber": 13},
        {"startLineNumber": 16, "endLineNumber": 16}]
}
+{ 
    "clippings": [{"region": "production-phase-end-item", "afterContent": ""},
        {"region": "production-phase-start-items", "afterContent": ""},
        {"region": "pre-release-phase-1-end-item", "afterContent": "\n..."}]
}

#### Rationale
- This phase's version numbers let consumers know software is production-ready.
- Major version rules make it easy for consumers to figure out whether versions are backward compatible. 
This, in turn, makes it easy for them to update software to the latest version that **does not break their software**.  

  Before semver, developers used all sorts of versioning systems. There was no standard way to tell whether versions were backward compatible. This made updating dependencies dangerous. 
Semver mitigates that danger. Some argue that semver isn't useful because not everyone follows its rules. Consider traffic rules; delinquents flout them, sometimes 
with tragic consequences. But most still abide by them because doing so keeps us safer. Likewise, following semver's rules keeps us safer, 
regardless of what others do.
- Minor and patch version rules give consumers granular control. Some consumers find this useful.

### Notes

#### Excessive Major Version Increments
Developers often associate [major version](#major-version) increments with significant changes and rewrites. Incrementing major version for small changes might seem odd. 
What's more, doing so could cause major version to balloon. [Version number](#version-number)s like `42.0.0` might occur. That said, incrementing major version for every [backward incompatible](#backward-incompatible)
change is **critical**. Not doing so could **break consumers in production**.

Semver's [FAQ](https://semver.org/#if-even-the-tiniest-backwards-incompatible-changes-to-the-public-api-require-a-major-version-bump-wont-i-end-up-at-version-4200-very-rapidly) 
mentions this issue. It suggests proper planning to reduce backward incompatible changes in the [production phase](#production-phase). Aside from proper planning, these practices might help:

- Avoid rushing into the production phase. Whether a piece of software is production-ready is subjective. Consider setting a high bar for production readiness and 
staying in the [pre-release phase](#pre-release-phase) for longer. That will iron out more kinks before the production phase.

- Batch backward incompatible changes. Be cautious though - bug fixes could take longer to reach consumers. 
Consider planning a release cycle to manage consumer's expectations.

#### Production > Pre-Release > Production Cycle
Before any [release](#release), you can go through the pre-release phase again. Why bother? As [mentioned](#rationale-1), it lets consumers know software isn't 
production-ready and facilitates quick iterations. After a production > pre-release > production cycle, the version history of a piece of software looks like this:

@{
    "highlightLineRanges": [{"startLineNumber": 1, "endLineNumber": 1}, 
        {"startLineNumber": 5, "endLineNumber": 5},
        {"startLineNumber": 9, "endLineNumber": 9},
        {"startLineNumber": 13, "endLineNumber": 13},
        {"startLineNumber": 17, "endLineNumber": 17},
        {"startLineNumber": 20, "endLineNumber": 20}]
}
+{ 
    "clippings": [{"region": "pre-release-phase-2", "afterContent": ""},
        {"region": "production-phase-end-item", "afterContent": "\n..."}]
}

! A pre-release phase can take place in parallel with a production phase. Create two version control branches for this. 
! On one, continue fixing bugs for the ongoing production phase. On the other, add new features and tweak the public API in 
! the pre-release phase.

## Build Metadata
When we defined [version number](#version-number), we mentioned the `build_metadata` segment. Version numbers in all 3 of semver's phases can contain this segment.  

What is it used for? Developers often use continuous integration (CI) systems to build their software. Usually, pushing changes to a specific 
branch of a repository triggers a CI build. Each CI build produces a [version](#version) of the software, referred to as a CI artifact. Most CI artifacts 
aren't **published**. For example, in the run-up to version `0.3.0`, a developer may push changes several times. Every push generates its own CI 
artifact. The developer **publishes** only the last one as `0.3.0`. Each **unpublished** CI artifact needs a **unique** version number for identification. This is where `build_metadata` 
comes in. `build_metadata` usually follows an incrementing format like `YYYYMMDD.<build number for the day>`. Values generated from such a format are **unique**. 
CI builds append `build_metadata` to the upcoming to-be-published version number. E.g., `0.3.0+20190111.4` or `0.3.0+20190114.6`. CI 
artifacts are thus distinguishable by their `build_metadata`. Also, `build_metadata` link CI artifacts to the builds that produced them.  

You might wonder what developers do with CI artifacts. Often, they're used to test specific commits before publishing a new version. Also, 
consumers who urgently need changes made by a commit might use a CI artifact in the interim.  

Including CI artifacts, the version history of a piece of software looks like this: 

@{
    "highlightLineRanges": [{"startLineNumber": 6, "endLineNumber": 6}, 
        {"startLineNumber": 9, "endLineNumber": 9},
        {"startLineNumber": 12, "endLineNumber": 12},
        {"startLineNumber": 15, "endLineNumber": 15},
        {"startLineNumber": 27, "endLineNumber": 27},
        {"startLineNumber": 30, "endLineNumber": 30},
        {"startLineNumber": 41, "endLineNumber": 41},
        {"startLineNumber": 44, "endLineNumber": 44},
        {"startLineNumber": 47, "endLineNumber": 47}]
}
```
...

2.1.0
- Marked a feature as deprecated.

2.1.0+20190111.4
- Pushed commits leading up to 2.1.0.

2.1.0+20190111.3
- Pushed commits leading up to 2.1.0.

2.1.0+20190111.2
- Pushed commits leading up to 2.1.0.

2.1.0+20190111.1
- Pushed commits leading up to 2.1.0.

2.0.0
- Fixed backward incompatible bugs

... 

1.0.0-beta.0
- Features complete. Architecture stabilized. But software needs rigorous testing and optimizing.
- Made backward compatible changes.

1.0.0-beta.0+20190109.1
- Pushed commits leading up to 1.0.0-beta.0.

1.0.0-beta.0+20190108.3
- Pushed commits leading up to 1.0.0-beta.0.

1.0.0-alpha.1
- Made backward compatible changes.

...

0.3.0 
- Made backward incompatible changes.

0.3.0+20190105.1
- Pushed commits leading up to 0.3.0.

0.3.0+20190104.2
- Pushed commits leading up to 0.3.0.

0.3.0+20190104.1
- Pushed commits leading up to 0.3.0.

0.2.0
- Made backward compatible changes.

...
```

<!-- TODO ## Version Number Precedence --> 

## Conclusion
We hope this article has given you a good grasp of semver. Also, we hope we've made it easier to consume Jering software with confidence.

Have questions or spot any mistakes? [Create a Github issue](https://github.com/JeringTech/Website/issues/new). If you found this article useful, consider sharing it on:
- [Twitter](https://twitter.com/intent/tweet?via=JeringTech&url=https://www.jering.tech/articles/semantic-versioning-in-practice&hashtags=semver,semanticversioning,versioning,devops,programming)
- [Facebook](https://www.facebook.com/sharer/sharer.php?u=https://www.jering.tech/articles/semantic-versioning-in-practice)

Thanks for reading!

## References
- [Tom Preston-Werner](http://tom.preston-werner.com/) 2013, *https://semver.org/spec/v2.0.0.html*, accessed 15 January 2019.