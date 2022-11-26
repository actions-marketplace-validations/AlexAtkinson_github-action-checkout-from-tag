# Introduction

![version](https://img.shields.io/github/v/release/AlexAtkinson/github-action-checkout-from-tag?style=flat-square)
![license](https://img.shields.io/github/license/AlexAtkinson/github-action-checkout-from-tag?style=flat-square)
![language](https://img.shields.io/github/languages/top/AlexAtkinson/github-action-checkout-from-tag?style=flat-square)
![repo size](https://img.shields.io/github/repo-size/AlexAtkinson/github-action-checkout-from-tag?style=flat-square)
![HitCount](https://hits.dwyl.com/AlexAtkinson/github-action-checkout-from-tag.svg?style=flat-square)
![Count of Action Users](https://img.shields.io/endpoint?url=https://AlexAtkinson.github.io/github-action-checkout-from-tag/github-action-checkout-from-tag.json&style=flat-square)
<!-- https://github.com/marketplace/actions/count-action-users -->

Unlike other clone/checkout actions, this action _automatically_ detects the depth of a specified tag or commit hash, and clones the specified repo from that depth.

While the shallow clone is often the answer when handling large repos with a lot of history, there are often times when it would be beneficial to be able to clone from 'origin/HEAD' back to a specific tag or commit hash.<br>
Such scenarios have been countered several times through the years as the bones behind the [GitOps Automatic Versioning](https://github.com/marketplace/actions/gitops-automatic-versioning) Action were hardening. With the release of that action, the scope of this impediment across it's potential userbase and it's associated cost(s) became well worth mitigating.

> NOTE: The GitOps Automatic Versioning action will receive this update in a future release (post 0.1.7).

No, there is no git option to clone a repo _after_, or _between_ specified commits.

## Usage

### Inputs

<dl>
  <dt>tag: [string] (Required)</dt>
    <dd>The tag or commit hash to clone to.</dd>
  <dt>repo: [string] (Optional)</dt>
    <dd>The repository to clone.<br>
    Default: The user action repository.</dd>
  <dt>dir: [string] (Optional)</dt>
    <dd>The directory into which to clone the repository.<br>
    Default: The name of the repository.</dd>
</dl>

### Example GH Action Workflow

This is a valid workflow utilizing this action.

    name: checkout-from-tag

    on:
      workflow_dispatch:

    jobs:
      use-action:
        name: Checkout From Tag
        runs-on: ubuntu-latest
        steps:
        - uses: AlexAtkinson/github-action-checkout-from-tag@latest
          with:
            repo: github-action-gitops-autover
            tag: 0.1.6
            dir: /tmp/foo_bar
        - name: Verify Checkout
          run: |
            cd /tmp/foo_bar
            git log --pretty=oneline
            echo -e "\n\nThere are several versions prior to 0.1.6, so as long as the log ends there, this action has executed successfully."

## Code Logic

For those interested, here's some pseudo code:

    1. Clone only the administrative files to get access to the full git log.
    2. Count the commits between the specified tag/commit and origin/HEAD to determine the depth.
    3. Clone the repo to the detected depth.

## [Known|Non]-Issues

None yet.

## Future Enhancements

PR's welcome...