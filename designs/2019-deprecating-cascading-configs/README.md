- Start Date: (2019-11-16)
- RFC PR: (TBF)
- Authors: (唯然<weiran.zsd@outlook.com>)

# deprecating cascading configs

## Summary

<!-- One-paragraph explanation of the feature. -->
This RFC deprecates cascading and hierarchy configs, so ESLint will not merge cascading config files in a directory structure.

## Motivation

<!-- Why are we doing this? What use cases does it support? What is the expected
outcome? -->
Presently there are 2 ways to set up directory-specific configs: a) cascading and hierarchy; b) glob-based configuration.

1. cascading and hierarchy. (TODO: a short description)

2. glob-based configuration(introduced in v4.1.0). Sometimes a more fine-controlled configuration is necessary, for example if the configuration for files within the same directory has to be different. Therefore you can provide configurations under the overrides key that will only apply to files that match specific glob patterns, using the same format you would pass on the command line (e.g., app/**/*.test.js).

There are several reasons to deprecate the 1st, and encourage users using the 2nd:
1. Simplicity. It is not a good practice having another way to do things. And, the 2nd is more powerful and flexible.
2. Maintainability. As we have continued to add new features, the configuration file format has continued to be extended and evolve into a state where it is the most complicated part of ESLint. The `.eslintrc` way of doing things means a variety of different merging strategies (cascading, extending, overriding), loading strategies (for plugins, processors, parsers), and naming conventions (`eslint-config-*`, `eslint-plugin-*`). It would a fresh air to stop the automatic merging behavior.
3. Almost all users have been using v4.1.0 or greater version.(it was released in 2015, 4 years passed!)

## Detailed Design

<!--
   This is the bulk of the RFC.

   Explain the design with enough detail that someone familiar with ESLint
   can implement it by reading this document. Please get into specifics
   of your approach, corner cases, and examples of how the change will be
   used. Be sure to define any new terms in this section.
-->
Three steps:

1. **Soft deprecation**:
    - Update our documentation to announce to deprecate the cascading and hierarchy configs on the next minor release.
    - ESLint shows a warning when it encountered cascading and hierarchy configs(but still loaded).
2. **Hard deprecation**: Add the deprecation warning of the cascading and hierarchy configs in ESLint 7.0.0.
    - ESLint throws an error when it encountered cascading and hierarchy configs.
    - ESLint shows a warning when it encountered `"root": true`.
3. **Removal**: Remove the cascading and hierarchy functionality in ESLint 8.0.0.


## Documentation

<!--
    How will this RFC be documented? Does it need a formal announcement
    on the ESLint blog to explain the motivation?
-->
1. A formal announcement:
    - to explain the motivation
    - to encourage users migrating to glob-based configs + `extends`.
2. update the document:
    - Configuration: Cascading and Hierarchy

## Drawbacks

<!--
    Why should we *not* do this? Consider why adding this into ESLint
    might not benefit the project or the community. Attempt to think 
    about any opposing viewpoints that reviewers might bring up. 

    Any change has potential downsides, including increased maintenance
    burden, incompatibility with other tools, breaking existing user
    experience, etc. Try to identify as many potential problems with
    implementing this RFC as possible.
-->
Users may need to take some time to upgrade their ESLint configs. :)

## Backwards Compatibility Analysis

<!--
    How does this change affect existing ESLint users? Will any behavior
    change for them? If so, how are you going to minimize the disruption
    to existing users?
-->
* This is a breaking change for end users who use cascading and hierarchy configs.
* This change has no compatibility impact on shareable configs and plugins.

## Alternatives

<!--
    What other designs did you consider? Why did you decide against those?

    This section should also include prior art, such as whether similar
    projects have already implemented a similar feature.
-->

## Open Questions

<!--
    This section is optional, but is suggested for a first draft.

    What parts of this proposal are you unclear about? What do you
    need to know before you can finalize this RFC?

    List the questions that you'd like reviewers to focus on. When
    you've received the answers and updated the design to reflect them, 
    you can remove this section.
-->

## Help Needed

<!--
    This section is optional.

    Are you able to implement this RFC on your own? If not, what kind
    of help would you need from the team?
-->

## Frequently Asked Questions

<!--
    This section is optional but suggested.

    Try to anticipate points of clarification that might be needed by
    the people reviewing this RFC. Include those questions and answers
    in this section.
-->

## Related Discussions

<!--
    This section is optional but suggested.

    If there is an issue, pull request, or other URL that provides useful
    context for this proposal, please include those links here.
-->
https://github.com/eslint/eslint/issues/3611
