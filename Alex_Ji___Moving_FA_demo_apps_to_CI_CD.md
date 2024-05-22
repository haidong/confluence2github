:::::::::::: {#page}
:::::::: {#main .aui-page-panel}
:::: {#main-header}
::: {#breadcrumb-section}
1.  [Alex Ji](index.html)
2.  [Alex Ji's Home](377815074.html)
:::

# [ Alex Ji : Moving FA demo apps to CI/CD ]{#title-text} {#title-heading .pagetitle}
::::

::::: {#content .view}
::: page-metadata
Created by [ Alex Ji]{.author}, last modified on Feb 17, 2024
:::

::: {#main-content .wiki-content .group}
## Coding standard {#MovingFAdemoappstoCI/CD-Codingstandard}

-   Since both Amir and I will be using Jetbrains, we should pick the
    same linter/formatter plugin to maintain coding style consistency
    -   Same formatting plugin should be available for VS Code for
        new/interested team members
-   We should improve this as we go

## Repo branching and commit messages {#MovingFAdemoappstoCI/CD-Repobranchingandcommitmessages}

-   Repository cleaning up
    -   Clean up `ad-ecommerce`  repo. Right now the application is NOT
        based on default branch
        `master. It is based on ``appd-dev-new`` . We should rebase it on top of master.`
        -   `We should name the branch ``main`` , to conform with industry trend of removing insensitive words such as ``master`` , ``slave`` `
    -   `Start PR reviews`
-   Branch naming: start with Jira project, separated by a dash, then
    Jira issue number, then a short description. For example,
    DEMO-1234-helm-chart-collector-temp-version
    -   There maybe cases when the commit is too small to warrant a Jira
        ticket, in which case a branch without Jira but with a sensible
        name will be fine
        -   These cases should be rare.
    -   **In the git commit message, it is critical that the subject
        start with Jira number followed by a space. For example,
        \"DEMO-1234 Temp change to force collector version\". The lambda
        function relies on this to infer the URL for Jira link.**
-   Provide good commit messages before pushing the code to remote repos
    and asking for PR review
    -   Wonderful video on git best practices, A Branch in Time:
    -   Put the Jira issue url link at the bottom of the commit message.
        -   There might some hook to automate this
-   Commit becomes a pull request(PR). In almost all cases except
    emergency, the PR needs to be approved before merging to main and
    released to production.
    -   Code PR notification via email or WebEx spaces
    -   WebEx space messages are sent using WebEx bot.

## Workflow and automated CI/CD process {#MovingFAdemoappstoCI/CD-WorkflowandautomatedCI/CDprocess}

-   Our source code is stored using AWS codeCommit. It lacks in feature
    and CI/CD support, compared with github, gitlab, bitbucket, and so
    on.
    -   There is still value in creating our own CI/CD process based on
        codeCommit. This could be an interesting use case that can be
        sold or demo\'ed.
-   Bug fixes, improvements, feature requests are first entered into our
    issue tracking system. Typically this would be a story under the
    DEMO project. Let\'s call this DEMO-1234
-   We pull in the latest code from our main branch (currently
    appd-dev-new), then create a new branch. Name the branch with a
    descriptive name like `DEMO-1234-helm-chart-collector-temp-version` 
-   Once the work is done, push the branch upstream, then create a PR
    (Pull Request) in the codeCommit repo
-   Our WebEx bot will send a notification to a developer WebEx Space,
    linking the PR and Jira story. This provides enough context for
    fellow team members to review, provide feedback as needed, and then
    approve and merge.
    -   Our team is small, so currently we don\'t need sophisticated
        approval rules. But if the team gets bigger, we may need more
        rules and disciplines here.
    -   Behind the scene, technologies used are WebEx bot and aws Lambda
        functions.
-   Once the code is merged, a WebEx message will be sent to the Space
    using aws Lambda function
-   Concurrently, another Lambda function will invoke code checkout,
    build, and deployment into Dev environment on an EC2 instance
    -   The EC2 instance was built previously with necessary tools and
        libraries
    -   Remember to update the tools such as JDK/node.js/etc. versions
        as technology advances
-   Another process checks the image tags used in dev, demo1, demo2,
    demo3, demo4 and other environment we own. Using smoke/sanity
    checks, after code has been running successfully on dev for a number
    of days, those container images will be promoted to other
    environment automatically.
    -   I haven\'t finished building this yet. I have started collecting
        and writing down good smoke tests in the section below.
    -   Also plan to integrate with Jira, so Jira tickets and stories
        can be closed automatically
-   Separately, we also do a complete k8s cluster rebuild on Saturday
    morning, so as to minimize interruptions to our colleagues around
    the globe. This also ensures the demo environments start with a
    clean slate on Monday.

## Our own test cases for demo environment validation {#MovingFAdemoappstoCI/CD-Ourowntestcasesfordemoenvironmentvalidation}

-   Collect some invariants for demo environment: [CCO and CSAAS
    environment sanity
    checks](CCO-and-CSAAS-environment-sanity-checks_389729349.html)
-   Write tests to enforce those invariants
-   Not sure how much effort the above will take, but worth thinking
    about\
    -   It\'d be nice if there is a visual component showing Jira
        stories of the last deployment in a stage contains, via a UI
        bubble or something like that

## IaC (Infrastructure as Code) for EC2, S3, RDS, ALB, etc. needs to be established {#MovingFAdemoappstoCI/CD-IaC(InfrastructureasCode)forEC2,S3,RDS,ALB,etc.needstobeestablished}

-   TerraForm or OpenTofu?
-   Plumi?
-   Start incorporating IaC into CI/CD process above
-   Eventually IaC, along with perhaps git repository mirroring to
    Azure, GCP, OCI, and other cloud providers, will enable us to stand
    up the demo environment on Azure, GCP, etc.
:::
:::::
::::::::

::::: {#footer role="contentinfo"}
:::: {.section .footer-body}
Document generated by Confluence on May 22, 2024 14:04

::: {#footer-logo}
[Atlassian](https://www.atlassian.com/)
:::
::::
:::::
::::::::::::
