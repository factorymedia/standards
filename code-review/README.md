# Factory Media Code Review Guide

  1. [General Rules](#general-rules)
  1. [Code Review Checklist](#checklist)
  1. [Pull Request Format](#pull-request-format)


## General Rules

  - All changes must be reviewed
  - If LOC is longer than 200 lines, the PR has to be broken down to smaller chunks
  - Before merging a PR, it should have positive review (thumb - :+1: / LGTM) by one / more developers.
  - Anyone can review and reviewers should be careful when leaving reviews. (No offense / criticism / judgement )
  - A review submitter shouldn’t take the review personally.
  - Always provide proper reason / facts to support your review / opinion.
  - Link to the relevant section in the standards repo when commenting on code style issues.
  - Getting blamed will NOT get you fired. We are extremely forgiving in this respect.


## Checklist

### General
  - Does the code work? Does it perform its intended function, the logic is correct etc.
  - Is all the code easily understood?
  - Does it conform to our agreed coding conventions?
  - Is there any redundant or duplicate code?
  - Is the code as modular as possible?
  - Can any global variables be replaced?
  - Is there any commented out code?
  - Do loops have a set length and correct termination conditions?
  - Can any of the code be replaced with library functions?
  - Can any logging or debugging code be removed?

### Security
  - Are all data inputs checked (for the correct type, length, format and range) ?
  - Where third-party utilities are used, are returning errors being caught?
  - Are output values checked and encoded?
  - Are invalid parameter values handled?

### Documentation
  - Do comments exist and describe the intent of the code?
  - Is any unusual behaviour or edge-case handling described?
  - Is the use and function of third-party libraries documented?
  - Are data structures and units of measurement explained?
  - Is there any incomplete code? If so, should it be removed or flagged with a suitable marker like “TODO”?

### Testing
  - Is the code testable? i.e. don’t add too many or hide dependencies, unable to initialise objects, test frameworks can use methods etc.
  - Do tests exist and are they comprehensive? i.e. has at least your agreed on code coverage.
  - Do unit tests actually test that the code is performing the intended functionality?
  - Are arrays checked for ‘out-of-bounds’ errors?
  - Could any test code be replaced with the use of an existing API?


## Pull Request Format
  - Prefix the branch & PR with the Jira ticket ID eg. `[TD-000] Brief summary of branch`
  - Include Jira ticket number, summary description of the changes, testing notes & example screenshots for UI changes.

### Example PRs
  - https://github.com/factorymedia/coresites/pull/287
  - https://github.com/factorymedia/coresites/pull/286
  - https://github.com/factorymedia/coresites/pull/282

Use this as a base PR template:

```md
**Jira Ticket**
https://factorymedia.atlassian.net/browse/TD-xxx

**Summary**
Quick description of changes and expected behaviour.

**Example**
A few links or images showing before / after changes.

**Testing / QA Notes**
Brief notes to save your tester some time, and let them know / what to test for.

```

