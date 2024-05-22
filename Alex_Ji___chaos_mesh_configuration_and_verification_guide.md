::::::::::::::: {#page}
::::::::::: {#main .aui-page-panel}
:::: {#main-header}
::: {#breadcrumb-section}
1.  [Alex Ji](index.html)
2.  [Alex Ji's Home](377815074.html)
:::

# [ Alex Ji : chaos-mesh configuration and verification guide ]{#title-text} {#title-heading .pagetitle}
::::

:::::::: {#content .view}
::: page-metadata
Created by [ Alex Ji]{.author}, last modified on Mar 24, 2024
:::

::: {#main-content .wiki-content .group}
[chaos-mesh](https://chaos-mesh.org/docs/){.external-link
rel="nofollow"} is [an open source cloud-native Chaos Engineering
platform. It offers various types of fault simulation and has an
enormous capability to orchestrate fault
scenarios.]{style="color: rgb(28,30,33);"}

It is installed in all of our demo clusters. To minimize possible
mishaps, only the ad-ecommerce namespace is allowed for chaos
experimentation.

There are two ways to view and create chaos experimentation:

-   via CLI using kubectl commands
-   via web interface. The web url follows the convention of env-chaos:
    -   [https://fcs-chaos.demolabs.net](https://fcs-chaos.demolabs.net){.external-link
        rel="nofollow"}
    -   [https://dev-chaos.demolabs.net](https://dev-chaos.demolabs.net){.external-link
        rel="nofollow"}
    -   [https://gandalf-chaos.demolabs.net](https://gandalf-chaos.demolabs.net){.external-link
        rel="nofollow"}
    -   [https://demo1-chaos.demolabs.net](https://demo1-chaos.demolabs.net){.external-link
        rel="nofollow"}
    -   [https://demo2-chaos.demolabs.net](https://demo2-chaos.demolabs.net){.external-link
        rel="nofollow"}
    -   [https://demo3-chaos.demolabs.net](https://demo3-chaos.demolabs.net){.external-link
        rel="nofollow"}
    -   [https://demo4-chaos.demolabs.net](https://demo4-chaos.demolabs.net){.external-link
        rel="nofollow"}

## Chaos experiment using the CLI {#chaosmeshconfigurationandverificationguide-ChaosexperimentusingtheCLI}

You can create chaos using yaml files. chaos-mesh documentation site
provides some typical chaos sample code to get started. On Gandalf,
I\'ve created a network chaos event to test eBPF for Cisco EMEA 2024,
below is the yaml file I used:

    kind: Schedule
    apiVersion: chaos-mesh.org/v1alpha1
    metadata:
      namespace: ad-ecommerce-gandalf
      name: network-delay-example
    spec:
      schedule: '@every 20m'
      startingDeadlineSeconds: null
      concurrencyPolicy: Forbid
      historyLimit: 2
      type: NetworkChaos
      networkChaos:
        selector:
          namespaces:
            - ad-ecommerce-gandalf
          labelSelectors:
            app: ecom-ecommerce-svc
            version: ecom-ecommerce-green
        mode: all
        action: delay
        duration: 300s
        delay:
          latency: 30s
        direction: both
        target:
          selector:
            namespaces:
              - ad-ecommerce-gandalf
            labelSelectors:
              app: ecom-mysqldb
          mode: all

You just need to run `kubectl apply -f yamlFile` to apply it. You can
then use various describe inspect the results.

## Chaos experiment using the web dashboard {#chaosmeshconfigurationandverificationguide-Chaosexperimentusingthewebdashboard}

As mentioned above, the web dashboard for chaos-mesh is exposed via
ingress. Click on the appropriate links above to access. You will need a
token.

The token can be obtained in 2 ways:

-   Using the CLI, while under the ad-ecommerce namespace, run
    `kubectl create token account-ad-ecommerce-manager-chaos` 
-   For members of the demo developers, inside the \"AppD Demo Team\"
    WebEx room, ask alji_bot for it: `@alji_bot get fcs chaos token` 

Here is a screenshot of how to get the token via WebEx room. Start
copying the token at line \"ey\...\" all the way to \"\...tcQ\":

[![](attachments/409700257/409700255.png){.confluence-embedded-image
draggable="false" height="250"
image-src="attachments/409700257/409700255.png"
unresolved-comment-count="0" linked-resource-id="409700255"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-03-23 at 6.08.12 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="409700257"
linked-resource-container-version="4"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}

The token can expire. If you are asked to login again, just regenerate
the token. Below is the login screenshot. You can provide whatever name
you like, just paste the token value into the second text box:

[![](attachments/409700257/409700256.png){.confluence-embedded-image
draggable="false" height="250"
image-src="attachments/409700257/409700256.png"
unresolved-comment-count="0" linked-resource-id="409700256"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-03-23 at 6.13.46 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="409700257"
linked-resource-container-version="4"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}

After logging in, you will be at the landing page:

[![](attachments/409700257/409700263.png){.confluence-embedded-image
draggable="false" height="250"
image-src="attachments/409700257/409700263.png"
unresolved-comment-count="0" linked-resource-id="409700263"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-03-23 at 7.26.16 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="409700257"
linked-resource-container-version="4"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}

The image above is from Gandalf and had previously created some chaos
using the yaml files through the CLI, as you can see through the
Timeline and \"Recent Events\"

If the site keeps giving you warnings/errors, it might be time to log
out and log in again with a new token. Just go to Settings → Logout. Go
ahead and request a new token and try again.

\

## Chaos experiment using the web dashboard {#chaosmeshconfigurationandverificationguide-Chaosexperimentusingthewebdashboard.1}

The yaml file pasted above was applied to Gandalf for Cisco EMEA 2024.
It injects a network delay every 90 minutes between
\"ecom-ecommerce-green\" and the MySQL database. You can confirm this
by: CCO controller → Observe → Kubernetes → Ad-Ecommerce cluster →
Business Transactions → Filter View value \`

[attributes]{style="color: rgb(70,83,199);"}[(]{style="color: rgb(0,0,0);"}[[bt.name](http://bt.name){.external-link
rel="nofollow"}]{style="color: rgb(59,71,178);"}[)]{style="color: rgb(0,0,0);"}[
]{style="color: rgb(59,71,178);"}[\~]{style="color: rgb(141,78,237);"}[
]{style="color: rgb(59,71,178);"}[\'\*confirmOrder\*\'\` → Pick the
\"ecom-ecommerce-green-svc\" row → Change the time window to look back 1
day ago. You\'ll see the \"Average Response Time (ms)\" peaks every 90
minutes, shown below.]{style="color: rgb(117,59,204);"}

[[![](attachments/409700257/409700276.png){.confluence-embedded-image
draggable="false" height="250"
image-src="attachments/409700257/409700276.png"
unresolved-comment-count="0" linked-resource-id="409700276"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-03-23 at 7.50.16 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="409700257"
linked-resource-container-version="4"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}]{style="color: rgb(117,59,204);"}
:::

::::: {.pageSection .group}
::: pageSectionHeader
## Attachments: {#attachments .pageSectionTitle}
:::

::: {.greybox align="left"}
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-03-23 at 6.08.12 PM.png](attachments/409700257/409700255.png)
(image/png)\
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-03-23 at 6.13.46 PM.png](attachments/409700257/409700256.png)
(image/png)\
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-03-23 at 7.26.16 PM.png](attachments/409700257/409700263.png)
(image/png)\
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-03-23 at 7.50.16 PM.png](attachments/409700257/409700276.png)
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
