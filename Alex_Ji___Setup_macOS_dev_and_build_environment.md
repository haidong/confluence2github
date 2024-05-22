:::::::::::::: {#page}
:::::::::: {#main .aui-page-panel}
:::: {#main-header}
::: {#breadcrumb-section}
1.  [Alex Ji](index.html)
2.  [Alex Ji's Home](377815074.html)
3.  [How-to articles](How-to-articles_378602068.html)
:::

# [ Alex Ji : Setup macOS dev and build environment ]{#title-text} {#title-heading .pagetitle}
::::

::::::: {#content .view}
::: page-metadata
Created by [ Alex Ji]{.author}, last modified on Feb 19, 2024
:::

::::: {#main-content .wiki-content .group}
Follow this guide if you are setting up your macOS environment for the
first time. Existing macOS users may also use this for cross validation
and getting inspirations.

## Shell and packaging management using brew {#SetupmacOSdevandbuildenvironment-Shellandpackagingmanagementusingbrew}

Newer versions of macOS uses Z by default. Other than `brew`  listed
below, `oh-my-zsh`  and `iterm2`  is not mandatory.

1.  Install `oh-my-zsh` framework. Please follow instructions at
    [https://ohmyz.sh/#install](https://ohmyz.sh/#install){.external-link
    rel="nofollow"}
2.  Install the software package management system for macOS, Homebrew
    1.  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
            Be patient. This step may take awhile.

    2.  You may need to add `export PATH="/opt/homebrew/bin:$PATH"` to
        your `~/.zshrc`  file and `source ~/.zshrc`  to be able to use
        `brew`  command directly
3.  A popular terminal software is `iterm2` . You may want to install it
    by running `brew install iterm2` 

## Utilities, CLI tools, Java SDK, and Node.js related {#SetupmacOSdevandbuildenvironment-Utilities,CLItools,JavaSDK,andNode.jsrelated}

-   Install Docker by following instructions from
    [https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/){.external-link
    rel="nofollow"} Create a Docker account if you don\'t have one. Run
    Docker and sign in.

<!-- -->

-   Install `aws`  CLI tools by following instructions from
    [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html){.external-link
    rel="nofollow"}. Manually create `~/.aws`  directory if needed.
-   Install `sdkman`  by following instructions from
    [https://sdkman.io/install](https://sdkman.io/install){.external-link
    rel="nofollow"}
-   Run the following on a terminal

<!-- -->

-   -   `brew install jq` 

    -   `brew install helm` 

    -   `brew install derailed/k9s/k9s` 

    -   helm plugin install https://github.com/hypnoglow/helm-s3.git

    -   `sdk install java 11.0.21-amzn` 

    -   `sdk use java 11.0.21-amzn` 

    -   `sdk install gradle 6.8.3` 

    -   `brew install nvm`  *(Manually run `mkdir ~/.nvm`  if
        necessary)*

    -   `nvm install v18.13.0` *(In my environment, I had to install
        different versions of node. In such a case, you may want to
        reset your default node version. Run
        `nvm alias default 18.13.0`  for example. You may also need to
        run `nvm use default` )*

    -   `npm install -g @angular/cli` 

## aws credential refresh {#SetupmacOSdevandbuildenvironment-awscredentialrefresh}

1.  `Run "aws sso login" on a terminal`
2.  Go through Okta and sign in process, navigate to aws site. Follow
    the link to \"Command line or programmatic access\"
3.  Follow Option 2. You may need to put the creds under the `[default]`
    profile
    1.  The daily manual credential copy and paste may not be entirely
        necessary. Follow the steps below:\
        Update `~/.aws/config`  with `sso-session`  section. Here is how
        mine looks like:

        :::: {.code .panel .pdl style="border-width: 1px;"}
        ::: {.codeContent .panelContent .pdl}
        ``` {.syntaxhighlighter-pre syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" theme="Confluence"}
        ➜  ad-ecommerce git:(appd-dev-new) cat ~/.aws/config
        [default]
        region = us-west-2
        sso_region = us-west-2
        sso_account_id = 632713433352
        sso_role_name = appd-aws-632713433352-dev
        sso_session = demo
        [sso-session demo]
        sso_start_url = https://d-92670e9c76.awsapps.com/start
        sso_region = us-west-2
        sso_registration_scopes = sso:account:access
        ```
        :::
        ::::

        On command line, run `aws sso login` \
        Follow the authentication process, \"Confirm and Continue\"\
        Allow \"Allow botocore-client-demo to access your data?\"\
        Afterwards, `git pull`  worked for me successfully\
        There are perhaps `helm`  commands that need this `sso-session` 
        integration. I\'ll update when I figure that out.

## git setup and clone source repo {#SetupmacOSdevandbuildenvironment-gitsetupandclonesourcerepo}

-   open okta → AWS Foundation SSO → AWS Accounts -\> AppDynamics Demo
    -\> Command Line or programmatic access -\> copy Option 2 contents
    into \~/.aws/credentials -\> change the entry
    \[632713433352_appd-aws-6327134333342-dev\] (or whatever it is) to
    \[default\] and save credentials file

-   If present, comment out the following line by adding \# prefix in
    `/Library/Developer/CommandLineTools/usr/share/git-core/gitconfig`  

`helper = osxkeychain` 

Note that macOS may restore this file after an update or patching. If
you see error like \`fatal: unable to access
\'[https://git-codecommit.us-west-2.amazonaws.com/v1/repos/MyDemoRepo/](https://git-codecommit.us-west-2.amazonaws.com/v1/repos/MyDemoRepo/){.external-link
rel="nofollow"}\': The requested URL returned error: 403\`, it\'s very
likely that the line above is uncommented. Comment the line by adding a
\# prefix.

-   Run the following:\
    `git config --global credential.helper '!aws codecommit credential-helper $@'` \
    `git config --global credential.UseHttpPath true` 

## Repo setup for git and helm {#SetupmacOSdevandbuildenvironment-Reposetupforgitandhelm}

-   Create a `projects`  directory under your home directory.

-   `cd ~/projects` 

-   git clone https://git-codecommit.us-west-2.amazonaws.com/v1/repos/ad-ecommerce

-   Run the following commands from a terminal:\
    helm repo add demo
    [s3://appd-demo-helm-charts/charts]{rel="nofollow"}\
    helm repo add descheduler
    [https://kubernetes-sigs.github.io/descheduler/](https://kubernetes-sigs.github.io/descheduler/){.external-link
    rel="nofollow"}\
    helm repo add grafana
    [https://grafana.github.io/helm-charts](https://grafana.github.io/helm-charts){.external-link
    rel="nofollow"}\
    helm repo add appdynamics-cloud-helmcharts
    [https://appdynamics.jfrog.io/artifactory/appdynamics-cloud-helmcharts/](https://appdynamics.jfrog.io/artifactory/appdynamics-cloud-helmcharts/){.external-link
    rel="nofollow"}\
    helm repo add eks
    [https://aws.github.io/eks-charts](https://aws.github.io/eks-charts){.external-link
    rel="nofollow"}

## ssh and pem file {#SetupmacOSdevandbuildenvironment-sshandpemfile}

1.  `ssh-keygen -t ed25519 -a 100`  (I left password blank)
2.  Ask your teammate for `demo-admin.pem`  file and put it under
    `~/.ssh/`

## Kubenetes verification {#SetupmacOSdevandbuildenvironment-Kubenetesverification}

Create `~/.kube`  if doesn\'t exist, and ask your teammate for `config` 
file and put it there

  - run \"`kubectl config get-contexts"`  and you should see various
contexts listed, such as `demo-west-demo1-saas` , `docker-desktop` ,
etc.

## Build, push, and deploy {#SetupmacOSdevandbuildenvironment-Build,push,anddeploy}

Add this to your `~/.zshrc`  file:
`export NODE_OPTIONS=--openssl-legacy-provider` . *I found this line is
necessary to build the demo app.*

After steps above, you should be able to build the AD-ECommerce
applications

\- On command line, go to `~/projects/ad-ecommerce` 

\- `./build-all.sh` 

\- When the build finishes, it is time to push the images and binaries
to aws by running `push-images.sh` 

\- After the images have been pushed, in the last line of output, you
should see the `BUILD_NUMBER`   value, something like
`2023-12-11T14-34-28` . Take a note of it since you will need it if you
want to deploy it.

\- To deploy, edit the `./helm/ecommerce/values-dev.yaml`  file, find
the line that starts with `app_tag`  and replace its value with the
output tag value from step above

\- Run `cd .helm` 

`- Run ./deploy.sh dev install` \# This command reinstalls everything in
the demo-dev environment.

## Access to the bastion host {#SetupmacOSdevandbuildenvironment-Accesstothebastionhost}

\- Get on VPN, using San Jose\
- Go to the aws console (Okta -\> aws -\> expand the account -\>
Management console\
- Go to EC2 page\
- In the list of instances, search for \"bastion\"\
- Take a note of the public IPV4 column and grab the IP address of the
bastion host you want to hop on\
- In a terminal, run \`ssh -i \~/.ssh/demo-admin.pem
[ubuntu@35.166.3.46](mailto:ubuntu@35.166.3.46){.external-link
rel="nofollow"}\`. Customize the IP address as necessary.\
- Once on the terminal, run `./decrypt_ssh_keys.sh`  and supply with the
password you get from your teammate\
- Afterwards, you should be able to get to other hosts. For example, run
`ssh -i .ssh/demo-admin.pem inf-blds-usw2` 

Be sure to exit out of the bastion ssh session by typing \"exit\",
otherwise, the decrypted access file will linger. We will get a warning
due to security scan

## More shell customization {#SetupmacOSdevandbuildenvironment-Moreshellcustomization}

This customization assumes that you use Z shell, and the `oh-my-zsh` 
framework. See first section of this doc on how to set that up on macOS.

-   Enable the following `plugins`  in `~/.zshrc` 

    :::: {.code .panel .pdl style="border-width: 1px;"}
    ::: {.codeContent .panelContent .pdl}
    ``` {.syntaxhighlighter-pre syntaxhighlighter-params="brush: java; gutter: false; theme: Confluence" theme="Confluence"}
    plugins=(
            git
            aws
            kubectl
            kube-ps1
    )
    ```
    :::
    ::::

    \

    -   `git`  plugin prints the branch you are under, if it is dirty,
        etc.
    -   `aws`  plugin makes managing multiple `aws`  accounts (profiles)
        easier. It also adds aws profile/account being used to the
        command prompt.
        -   `asp profile-name` to switch to different aws accounts and
            profiles.
    -   `kubectl`  adds nice aliases. For example, instead of typing
        `kubectl` , you can just type `k` . Other aliases like `kgp`
        (\`kubectl get pod\`) is also nice.
    -   `kube-ps1`  adds the current kubectl context and namespace to
        the command prompt. This is really important because we have a
        number of k8s clusters to manage. Knowing the context can avoid
        making costly mistakes.

-   Note you may want further tweaking of the plugins. For example, the
    `aws`  plugin adds prompt text on the right side of terminal, which
    I don\'t like. I remove it.

\

\

\

:::: {.confluence-information-macro .confluence-information-macro-information}
[]{.aui-icon .aui-icon-small .aui-iconfont-info
.confluence-information-macro-icon}

::: confluence-information-macro-body
:::
::::

## Related articles {#SetupmacOSdevandbuildenvironment-Relatedarticles}

-   <div>

    [Page:]{.icon .aui-icon .content-type-page title="Page"}

    </div>

    ::: details
    [Setup macOS dev and build
    environment](/display/~alji@appdynamics.com/Setup+macOS+dev+and+build+environment)
    :::
:::::
:::::::
::::::::::

::::: {#footer role="contentinfo"}
:::: {.section .footer-body}
Document generated by Confluence on May 22, 2024 14:04

::: {#footer-logo}
[Atlassian](https://www.atlassian.com/)
:::
::::
:::::
::::::::::::::
