:::::::::::: {#page}
:::::::: {#main .aui-page-panel}
:::: {#main-header}
::: {#breadcrumb-section}
1.  [Alex Ji](index.html)
2.  [Alex Ji's Home](377815074.html)
:::

# [ Alex Ji : CCO and CSAAS environment sanity checks ]{#title-text} {#title-heading .pagetitle}
::::

::::: {#content .view}
::: page-metadata
Created by [ Alex Ji]{.author}, last modified on Feb 07, 2024
:::

::: {#main-content .wiki-content .group}
As part of [CICD](378603116.html) deployment process, we need to
collect, build, and maintain tests that validate the integrity of the
environment configuration and infrastructure that supports it.

These checks will be built into a routine that runs every time a new
deployment happens. They should also be scheduled to run on a regular
basis: daily/weekly/monthly, etc.

## CCO {#CCOandCSAASenvironmentsanitychecks-CCO}

-   metrics server deployment runs under the `kube-system` namespace
    -   Symptoms if this is not installed:
        -   `kubectl top pods` produces this error:\

            `error: Metrics API not available`

        -   In controller UI, navigate to Clusters → Cluster in question
            → Scroll down to \"Kubernetes Events\" section and click
            \"Show All Events\" button. You\'ll see warning messages
            such as:\

            `failed to get cpu utilization: unable to get metrics for resource cpu: unable to fetch metrics from resource metrics API: the server could not find the requested resource (get `[`pods.metrics.k8s.io`](http://pods.metrics.k8s.io){.external-link
            rel="nofollow"}`)`
    -   Resolution
        -   Install Kubernetes Metrics Server:
            [https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html](https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html){.external-link
            rel="nofollow"}
-   Some kind of \"ping\" test of the controller itself, perhaps
    something like `fsoc login` just so no errors are detected
    -   Validation of controller name against credentials used?
        (demo-dev controller matches demo-dev clientId, need to figure
        out how).
-   Baseline modules installed check.
-   chaos-mesh check
    -   Make sure to install the version with the right runtime, docker
        vs containerd. Going forward, it probably should be containerd
        as default, because newer versions of EKS (since 1.24) uses
        containerd as default.
    -   If [controllerManager.enableFilterNamespace]{.token .assign-left
        .variable style="color: rgb(54,172,170);"}[=]{.token .operator
        style="color: rgb(57,58,52);"}[true, then ensure that the
        namespace you want to inject chaos has the correct annotation:
        kubectl annotate ns [\$NAMESPACE]{.token .variable
        style="color: rgb(54,172,170);"}
        [chaos-mesh.org/inject](http://chaos-mesh.org/inject){.external-link
        rel="nofollow"}[=]{.token .operator
        style="color: rgb(57,58,52);"}enabled\
        [https://chaos-mesh.org/docs/configure-enabled-namespace/](https://chaos-mesh.org/docs/configure-enabled-namespace/){.external-link
        rel="nofollow"}\
        ]{.token .plain style="color: rgb(57,58,52);"}

## CSAAS {#CCOandCSAASenvironmentsanitychecks-CSAAS}
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
