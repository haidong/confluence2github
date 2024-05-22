::::::::::::::: {#page}
::::::::::: {#main .aui-page-panel}
:::: {#main-header}
::: {#breadcrumb-section}
1.  [Alex Ji](index.html)
2.  [Alex Ji's Home](377815074.html)
:::

# [ Alex Ji : Create aws vpc for CloudKart ]{#title-text} {#title-heading .pagetitle}
::::

:::::::: {#content .view}
::: page-metadata
Created by [ Alex Ji]{.author} on Apr 24, 2024
:::

::: {#main-content .wiki-content .group}
If CloudKart demo app runs on aws, the VPC needs to be peered with the
AppDynamics internal network. Here is how.

-   After logging into aws web console, go to VPC. Then click on
    \"Create VPC\"

[![](attachments/432669385/432669338.png){.confluence-embedded-image
draggable="false" height="250"
image-src="attachments/432669385/432669338.png"
unresolved-comment-count="0" linked-resource-id="432669338"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-04-24 at 4.36.40 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="432669385"
linked-resource-container-version="1"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}

-   Pick \"VPC and more\" and also give it a descriptive name. Leave
    everything else as default. Then click \"Create VPC\"

[![](attachments/432669385/432669342.png){.confluence-embedded-image
.confluence-thumbnail draggable="false" height="250"
image-src="attachments/432669385/432669342.png"
unresolved-comment-count="0" linked-resource-id="432669342"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-04-24 at 4.39.55 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="432669385"
linked-resource-container-version="1"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}

\

-   On the left side bar, click \"Elastic IPs\", then \"Allocate Elastic
    IP address\"
-   On the left side bar, click \"NAT gateways\", then \"Create NAT
    gateway\". You can pick any public subnet created in the first step,
    then pick the Elastic IP from step above

[![](attachments/432669385/432669375.png){.confluence-embedded-image
.confluence-thumbnail draggable="false" height="250"
image-src="attachments/432669385/432669375.png"
unresolved-comment-count="0" linked-resource-id="432669375"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-04-24 at 4.57.33 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="432669385"
linked-resource-container-version="1"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}

-   Go to the private subnet(s) created in the first step, click \"Route
    table\" in the bottom half of the screen, then \"Edit route table
    association\". Add a route with destination 0.0.0.0/0 and target as
    the NAT gateway you created.

[![](attachments/432669385/432669382.png){.confluence-embedded-image
.confluence-thumbnail draggable="false" height="250"
image-src="attachments/432669385/432669382.png"
unresolved-comment-count="0" linked-resource-id="432669382"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-04-24 at 5.03.24 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="432669385"
linked-resource-container-version="1"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}

-   Create a NETENG Jira ticket, use [
    [![](https://jira.corp.appdynamics.com/secure/viewavatar?size=xsmall&avatarId=16740&avatarType=issuetype){.icon}NETENG-344](https://jira.corp.appdynamics.com/browse/NETENG-344?src=confmacro){.jira-issue-key} -
    [AWS Nat Gateway access to cni-dev, Perf, Rapid and c0]{.summary}
    [Done]{.aui-lozenge .aui-lozenge-subtle .aui-lozenge-success
    .jira-macro-single-issue-export-pdf} ]{.jira-issue .resolved
    jira-key="NETENG-344"} as an example
-   Create a SFDN Jira ticket, use [
    [![](https://jira.corp.appdynamics.com/secure/viewavatar?size=xsmall&avatarId=54903&avatarType=issuetype){.icon}SFDN-5588](https://jira.corp.appdynamics.com/browse/SFDN-5588?src=confmacro){.jira-issue-key} -
    [AWS Nat Gateway access to cni-dev, Perf, Rapid and c0]{.summary}
    [Closed]{.aui-lozenge .aui-lozenge-subtle .aui-lozenge-success
    .jira-macro-single-issue-export-pdf} ]{.jira-issue .resolved
    jira-key="SFDN-5588"} as an example

\

\

\
:::

::::: {.pageSection .group}
::: pageSectionHeader
## Attachments: {#attachments .pageSectionTitle}
:::

::: {.greybox align="left"}
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-04-24 at 4.36.40 PM.png](attachments/432669385/432669338.png)
(image/png)\
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-04-24 at 4.39.55 PM.png](attachments/432669385/432669342.png)
(image/png)\
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-04-24 at 4.57.33 PM.png](attachments/432669385/432669375.png)
(image/png)\
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-04-24 at 5.03.24 PM.png](attachments/432669385/432669382.png)
(image/png)\
:::
:::::
::::::::
:::::::::::

::::: {#footer role="contentinfo"}
:::: {.section .footer-body}
Document generated by Confluence on May 22, 2024 14:04

::: {#footer-logo}
[Atlassian](https://www.atlassian.com/)
:::
::::
:::::
:::::::::::::::
