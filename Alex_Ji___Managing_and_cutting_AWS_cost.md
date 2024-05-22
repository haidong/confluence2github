::::::::::::::: {#page}
::::::::::: {#main .aui-page-panel}
:::: {#main-header}
::: {#breadcrumb-section}
1.  [Alex Ji](index.html)
2.  [Alex Ji's Home](377815074.html)
:::

# [ Alex Ji : Managing and cutting AWS cost ]{#title-text} {#title-heading .pagetitle}
::::

:::::::: {#content .view}
::: page-metadata
Created by [ Alex Ji]{.author} on Mar 04, 2024
:::

::: {#main-content .wiki-content .group}
There are some old objects under our Demo account that serve no purposes
now. We\'d like to learn what they are, then drop them to save cost.

# Find out the heavy hitters {#ManagingandcuttingAWScost-Findouttheheavyhitters}

Let\'s start with identifying heavy hitters and low hanging fruits. Once
they are removed, we will have less clutter which enables us to focus on
smaller number of services.

AWS Cost is confusing. The easiest and possibly most efficient place to
start is Cost Explorer

-   From Cost Explorer, find out the Service that costs us the most
    -   It is \"EC2-Other\" in our case
-   Focus on \"EC2-Other\"
    -   On the right side of the screen, Report parameters:
        -   In Filters section, under Service, pick \"EC2-Other\"
        -   Above the Filters section, in Group by, pick \"Usage type\"
        -   Play around with the Filters and Group By. It may give
            additional insight.

From the report here, it\'s obvious that we should focus on EBS volumes.

[![](attachments/401835849/401835842.png){.confluence-embedded-image
draggable="false" height="250"
image-src="attachments/401835849/401835842.png"
unresolved-comment-count="0" linked-resource-id="401835842"
linked-resource-version="1" linked-resource-type="attachment"
linked-resource-default-alias="Screenshot 2024-03-03 at 7.43.20 PM.png"
base-url="https://confluence.corp.appdynamics.com"
linked-resource-content-type="image/png"
linked-resource-container-id="401835849"
linked-resource-container-version="1"}]{.confluence-embedded-file-wrapper
.confluence-embedded-manual-size}

# Look for unused EBS Volumes and Snapshot {#ManagingandcuttingAWScost-LookforunusedEBSVolumesandSnapshot}

I wrote some bash script, and also with the help of some online tool
([https://gist.github.com/Eyjafjallajokull/4e917414cfb191391f9e51f6a8c3e46a](https://gist.github.com/Eyjafjallajokull/4e917414cfb191391f9e51f6a8c3e46a){.external-link
rel="nofollow"}), I found some unused volumes and snapshots to remove.

[[![](rest/documentConversion/latest/conversion/thumbnail/401835843/1){height="250"
draggable="false"}](/download/attachments/401835849/volumes_snapshots_costs.xlsx?version=1&modificationDate=1709517902330&api=v2){.confluence-embedded-file
nice-type="Excel Spreadsheet"
file-src="/download/attachments/401835849/volumes_snapshots_costs.xlsx?version=1&modificationDate=1709517902330&api=v2"
linked-resource-id="401835843" linked-resource-type="attachment"
linked-resource-container-id="401835849"
linked-resource-default-alias="volumes_snapshots_costs.xlsx"
mime-type="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
has-thumbnail="true" linked-resource-version="1" can-edit="true"
aria-label="volumes_snapshots_costs.xlsx"
draggable="false"}[]{.companion-edit-button-placeholder
.edit-button-overlay linked-resource-container-id="401835849"
linked-resource-id="401835843" template-name="companionEditIcon"
source-location="embedded-attachment"}]{.confluence-embedded-file-wrapper}

Here are the bash and Python program I
used.[[![](download/resources/com.atlassian.confluence.plugins.confluence-view-file-macro:view-file-macro-resources/images/placeholder-medium-file.png){height="250"
draggable="false"}[awsListStoppedEc2s]{.title}](/download/attachments/401835849/awsListStoppedEc2s?version=1&modificationDate=1709517902310&api=v2){.confluence-embedded-file
nice-type="null"
file-src="/download/attachments/401835849/awsListStoppedEc2s?version=1&modificationDate=1709517902310&api=v2"
linked-resource-id="401835845" linked-resource-type="attachment"
linked-resource-container-id="401835849"
linked-resource-default-alias="awsListStoppedEc2s"
mime-type="application/octet-stream" has-thumbnail="false"
linked-resource-version="1" can-edit="true"
aria-label="awsListStoppedEc2s"
draggable="false"}[]{.companion-edit-button-placeholder
.edit-button-overlay linked-resource-container-id="401835849"
linked-resource-id="401835845" template-name="companionEditIcon"
source-location="embedded-attachment"}]{.confluence-embedded-file-wrapper}[[![](download/resources/com.atlassian.confluence.plugins.confluence-view-file-macro:view-file-macro-resources/images/placeholder-medium-file.png){height="250"
draggable="false"}[awsUnassociatedSnapshots]{.title}](/download/attachments/401835849/awsUnassociatedSnapshots?version=1&modificationDate=1709517902289&api=v2){.confluence-embedded-file
nice-type="null"
file-src="/download/attachments/401835849/awsUnassociatedSnapshots?version=1&modificationDate=1709517902289&api=v2"
linked-resource-id="401835846" linked-resource-type="attachment"
linked-resource-container-id="401835849"
linked-resource-default-alias="awsUnassociatedSnapshots"
mime-type="application/octet-stream" has-thumbnail="false"
linked-resource-version="1" can-edit="true"
aria-label="awsUnassociatedSnapshots"
draggable="false"}[]{.companion-edit-button-placeholder
.edit-button-overlay linked-resource-container-id="401835849"
linked-resource-id="401835846" template-name="companionEditIcon"
source-location="embedded-attachment"}]{.confluence-embedded-file-wrapper}[[![](download/resources/com.atlassian.confluence.plugins.confluence-view-file-macro:view-file-macro-resources/images/placeholder-medium-file.png){height="250"
draggable="false"}[awsWastedEBS_Snapshots]{.title}](/download/attachments/401835849/awsWastedEBS_Snapshots?version=1&modificationDate=1709517902265&api=v2){.confluence-embedded-file
nice-type="null"
file-src="/download/attachments/401835849/awsWastedEBS_Snapshots?version=1&modificationDate=1709517902265&api=v2"
linked-resource-id="401835847" linked-resource-type="attachment"
linked-resource-container-id="401835849"
linked-resource-default-alias="awsWastedEBS_Snapshots"
mime-type="application/octet-stream" has-thumbnail="false"
linked-resource-version="1" can-edit="true"
aria-label="awsWastedEBS_Snapshots"
draggable="false"}[]{.companion-edit-button-placeholder
.edit-button-overlay linked-resource-container-id="401835849"
linked-resource-id="401835847" template-name="companionEditIcon"
source-location="embedded-attachment"}]{.confluence-embedded-file-wrapper}[[![](download/resources/com.atlassian.confluence.plugins.confluence-view-file-macro:view-file-macro-resources/images/placeholder-medium-file.png){height="250"
draggable="false"}[snapshots.py]{.title}](/download/attachments/401835849/snapshots.py?version=1&modificationDate=1709517902216&api=v2){.confluence-embedded-file
nice-type="null"
file-src="/download/attachments/401835849/snapshots.py?version=1&modificationDate=1709517902216&api=v2"
linked-resource-id="401835848" linked-resource-type="attachment"
linked-resource-container-id="401835849"
linked-resource-default-alias="snapshots.py"
mime-type="application/octet-stream" has-thumbnail="false"
linked-resource-version="1" can-edit="true" aria-label="snapshots.py"
draggable="false"}[]{.companion-edit-button-placeholder
.edit-button-overlay linked-resource-container-id="401835849"
linked-resource-id="401835848" template-name="companionEditIcon"
source-location="embedded-attachment"}]{.confluence-embedded-file-wrapper}
The spreadsheet above is produced using these programs, with slight
modification to call out the costs.

# Drop unused EBS Volumes and Snapshots {#ManagingandcuttingAWScost-DropunusedEBSVolumesandSnapshots}

Under our Demo account, as far as I know, pretty much the only thing we
care about right now are the k8s clusters. There are some EC2 instances,
such as our bastion hosts, CI/CD build machine, EC2 instances for Cisco
EMEA, and other purposes. So any volumes/snapshots not associated with
them should be safe to drop.

The spreadsheet file attached above lists volumes and snapshots that are
currently not used. So it is safe to drop them.

To be certain that volume cannot be dropped if used by EC2, I created an
instance and tested it. I was able to confirm that\'s not possible. That
eliminates the risk of wrongly identified volumes being dropped.

Side note: during my experiments of dropping volume, I was contacted by
Security about it. They want to make sure it was intended. So we may
come across some warnings when we drop stuff.
:::

::::: {.pageSection .group}
::: pageSectionHeader
## Attachments: {#attachments .pageSectionTitle}
:::

::: {.greybox align="left"}
![](images/icons/bullet_blue.gif){height="8" width="8"} [Screenshot
2024-03-03 at 7.43.20 PM.png](attachments/401835849/401835842.png)
(image/png)\
![](images/icons/bullet_blue.gif){height="8" width="8"}
[volumes_snapshots_costs.xlsx](attachments/401835849/401835843.xlsx)
(application/vnd.openxmlformats-officedocument.spreadsheetml.sheet)\
![](images/icons/bullet_blue.gif){height="8" width="8"}
[awsListStoppedEc2s](attachments/401835849/401835845)
(application/octet-stream)\
![](images/icons/bullet_blue.gif){height="8" width="8"}
[awsUnassociatedSnapshots](attachments/401835849/401835846)
(application/octet-stream)\
![](images/icons/bullet_blue.gif){height="8" width="8"}
[awsWastedEBS_Snapshots](attachments/401835849/401835847)
(application/octet-stream)\
![](images/icons/bullet_blue.gif){height="8" width="8"}
[snapshots.py](attachments/401835849/401835848.py)
(application/octet-stream)\
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
