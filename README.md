<!-- This file was automatically generated by the `build-harness`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->


[![Cloud Posse](https://cloudposse.com/logo-300x69.svg)](https://cloudposse.com)

# Packages [![TravisCI Build Status](https://travis-ci.org/cloudposse/packages.svg?branch=master)](https://travis-ci.org/cloudposse/packages) [![Codefresh Build Status](https://g.codefresh.io/api/badges/build?repoOwner=cloudposse&repoName=packages&branch=master&pipelineName=packages&accountName=cloudposse&type=cf-1)](https://g.codefresh.io/repositories/cloudposse/packages/builds?filter=trigger:build;branch:master;service:5b234974667ab79287990636~packages) [![Latest Release](https://img.shields.io/github/release/cloudposse/packages.svg)](https://github.com/cloudposse/packages/releases/latest) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Cloud Posse distribution of awesome apps.


---

This project is part of our comprehensive ["SweetOps"](https://docs.cloudposse.com) approach towards DevOps. 


It's 100% Open Source and licensed under the [APACHE2](LICENSE).









## Introduction


Use this repo to easily install releases of popular Open Source apps. We provide a few ways to use it.

1. `Makefile` based installer which downloads packages directly from their source and installs them
2. Alpine Linux package repository (`apk.cloudposse.com`) which installs prebuilt packages using original source binary (where possible)
3. Docker image (`cloudposse/packages`) which distributes an image with all linux binaries for `x86_64`

See examples below for usage.

## Usage


### Alpine Repository (recommended)

Configure the alpine repository

```
curl -sSL https://apk.cloudposse.com/install.sh | sudo bash
```

Simply install any package as normal:
```
apk add gomplate
```

You can use version pinning:
```
apk add gomplate==3.0.0-r0
```

And even repository pinning:
```
apk add gomplate@cloudposse==3.0.0-r0
```

### Makefile Interface

The `Makefile` interface works on OSX and Linux. It's a great way to distribute binaries in an OS-agnostic way which does not depend on a package manager.

See all available packages:
```
make -C install help
```

Install everything...
```
make -C install all
```

Install specific packages:
```
make -C install aws-vault chamber
```

Install to a specific folder:
```
make -C install aws-vault INSTALL_PATH=/usr/bin
```

Uninstall a specific package
```
make -C uninstall yq
```




## Examples

### Docker Multi-stage Build

Add this to a `Dockerfile` to install packages using a multi-stage build process:
```
FROM cloudposse/packages:latest AS packages

COPY --from=packages /packages/bin/kubectl /usr/local/bin/
```

### Docker with Git Clone

Or... add this to a `Dockerfile` to easily install packages on-demand:
```
RUN git clone --depth=1 -b master https://github.com/cloudposse/packages.git /packages && \
    rm -rf /packages/.git && \
    make -C /packages/install kubectl
```

### Makefile Inclusion

Sometimes it's necessary to install some binary dependencies when building projects. For example, we frequently 
rely on `gomplate` or `helm` to build chart packages.

Here's a stub you can include into a `Makefile` to make it easier to install binary dependencies.

```
export PACKAGES_VERSION ?= master
export PACKAGES_PATH ?= packages/
export INSTALL_PATH ?= $(PACKAGES_PATH)/vendor

## Install packages
packages/install:
        @if [ ! -d $(PACKAGES_PATH) ]; then \
          echo "Installing packages $(PACKAGES_VERSION)..."; \
          rm -rf $(PACKAGES_PATH); \
          git clone --depth=1 -b $(PACKAGES_VERSION) https://github.com/cloudposse/packages.git $(PACKAGES_PATH); \
          rm -rf $(PACKAGES_PATH)/.git; \
        fi

## Install package (e.g. helm, helmfile, kubectl)
packages/install/%: packages/install
        @make -C $(PACKAGES_PATH)/install $(subst packages/install/,,$@)

## Uninstall package (e.g. helm, helmfile, kubectl)
packages/uninstall/%:
        @make -C $(PACKAGES_PATH)/uninstall $(subst packages/uninstall/,,$@)
```



## Makefile Targets
```
cli53                     0.8.12     Command line tool for Amazon Route 53
kubens                    0.6.1      Fast way to switch between clusters and namespaces in kubectl – [✩Star] if you're using it!
gosu                      1.10       Simple Go-based setuid+setgid+setgroups+exec
slack-notifier            0.1.3      Command line utility to send messages with attachments to Slack channels via Incoming Webhooks
terragrunt                0.17.0     Terragrunt is a thin wrapper for Terraform that provides extra tools for working with multiple Terraform modules.
chamber                   2.2.0      CLI for managing secrets
gitleaks                  1.2.0      Audit git repos for secrets 🔑
lectl                     0.17       Script to check issued certificates by Let's Encrypt on CTL (Certificate Transparency Log) using https://crt.sh
hugo                      0.49.2     The world’s fastest framework for building websites.
emailcli                  1.0.3      Command line email sending client written in Go.
github-commenter          0.1.2      Command line utility for creating GitHub comments on Commits, Pull Request Reviews or Issues
goofys                    0.19.0     a high-performance, POSIX-ish Amazon S3 file system written in Go
gomplate                  3.0.0      A flexible commandline tool for template rendering. Supports lots of local and remote datasources.
k6                        0.22.1     A modern load testing tool, using Go and JavaScript - https://k6.io
helmfile                  0.40.1     Deploy Kubernetes Helm Charts
sops                      3.1.1      Secrets management stinks, use some sops!
shfmt                     2.5.1      A shell parser, formatter and interpreter (POSIX/Bash/mksh)
gotop                     1.5.0      A terminal based graphical activity monitor inspired by gtop and vtop
helm                      2.10.0     The Kubernetes Package Manager
gometalinter              2.0.11     Concurrently run Go lint tools and normalise their output
misspell                  0.3.4      Correct commonly misspelled English words in source files
cloudflared               2018.8.0   Argo Tunnel client
terraform-docs            0.4.5      Generate docs from terraform modules
fargate                   0.2.3      CLI for AWS Fargate
figurine                  0.2.2      Print your name in style
terraform                 0.11.8     Terraform is a tool for building, changing, and combining infrastructure safely and efficiently.
aws-vault                 4.4.1      A vault for securely storing and accessing AWS credentials in development environments
packer                    1.3.1      Packer is a tool for creating identical machine images for multiple platforms from a single source configuration.
json2hcl                  0.0.6      Convert JSON to HCL, and vice versa
teleport                  3.0.0      Privileged access management for elastic infrastructure.
kubectl                   1.12.1     Issue tracker and mirror of kubectl code
kops                      1.10.0     Kubernetes Operations (kops) - Production Grade K8s Installation, Upgrades, and Management
stern                     1.8.0      ⎈ Multi pod and container log tailing for Kubernetes
shellcheck                0.5.0      ShellCheck, a static analysis tool for shell scripts
kubectx                   0.6.1      Fast way to switch between clusters and namespaces in kubectl – [✩Star] if you're using it!
awless                    0.1.11     A Mighty CLI for AWS
github-release            0.7.2      Commandline app to create and edit releases on Github (and upload artifacts)
yq                        2.1.1      yq is a portable command-line YAML processor
ctop                      0.7.1      Top-like interface for container metrics
ghr                       0.12.0     Upload multiple artifacts to GitHub Release in parallel
aws-iam-authenticator     0.3.0      A tool to use AWS IAM credentials to authenticate to a Kubernetes cluster
htmltest                  0.10.1     :white_check_mark: Test generated HTML for problems
fetch                     0.3.1      fetch makes it easy to download files, folders, and release assets from a specific git commit, branch, or tag of public andssss
atlantis                  0.4.10     Terraform For Teams
```



## Related Projects

Check out these related projects.

- [build-harness](https://github.com/cloudposse/build-harness) - Collection of Makefiles to facilitate building Golang projects, Dockerfiles, Helm charts, and more
- [geodesic](https://github.com/cloudposse/geodesic) - Geodesic is the fastest way to get up and running with a rock solid, production grade cloud platform built on strictly Open Source tools.



## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/packages/issues), send us an [email][email] or join our [Slack Community][slack].

## Commercial Support

Work directly with our team of DevOps experts via email, slack, and video conferencing. 

We provide [*commercial support*][commercial_support] for all of our [Open Source][github] projects. As a *Dedicated Support* customer, you have access to our team of subject matter experts at a fraction of the cost of a full-time engineer. 

[![E-Mail](https://img.shields.io/badge/email-hello@cloudposse.com-blue.svg)](mailto:hello@cloudposse.com)

- **Questions.** We'll use a Shared Slack channel between your team and ours.
- **Troubleshooting.** We'll help you triage why things aren't working.
- **Code Reviews.** We'll review your Pull Requests and provide constructive feedback.
- **Bug Fixes.** We'll rapidly work to fix any bugs in our projects.
- **Build New Terraform Modules.** We'll develop original modules to provision infrastructure.
- **Cloud Architecture.** We'll assist with your cloud strategy and design.
- **Implementation.** We'll provide hands-on support to implement our reference architectures. 


## Community Forum

Get access to our [Open Source Community Forum][slack] on Slack. It's **FREE** to join for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build *sweet* infrastructure.

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/packages/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://github.com/orgs/cloudposse/projects/3) with our other projects, we would love to hear from you! Shoot us an [email](mailto:hello@cloudposse.com).

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2018 [Cloud Posse, LLC](https://cloudposse.com)



## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know at <hello@cloudposse.com>

[![Cloud Posse](https://cloudposse.com/logo-300x69.svg)](https://cloudposse.com)

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We love [Open Source Software](https://github.com/cloudposse/)!

We offer paid support on all of our projects.  

Check out [our other projects][github], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.

  [docs]: https://docs.cloudposse.com/
  [website]: https://cloudposse.com/
  [github]: https://github.com/cloudposse/
  [commercial_support]: https://github.com/orgs/cloudposse/projects
  [jobs]: https://cloudposse.com/jobs/
  [hire]: https://cloudposse.com/contact/
  [slack]: https://slack.cloudposse.com/
  [linkedin]: https://www.linkedin.com/company/cloudposse
  [twitter]: https://twitter.com/cloudposse/
  [email]: mailto:hello@cloudposse.com


### Contributors

|  [![Erik Osterman][osterman_avatar]][osterman_homepage]<br/>[Erik Osterman][osterman_homepage] | [![Igor Rodionov][goruha_avatar]][goruha_homepage]<br/>[Igor Rodionov][goruha_homepage] | [![Andriy Knysh][aknysh_avatar]][aknysh_homepage]<br/>[Andriy Knysh][aknysh_homepage] |
|---|---|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: https://github.com/osterman.png?size=150
  [goruha_homepage]: https://github.com/goruha
  [goruha_avatar]: https://github.com/goruha.png?size=150
  [aknysh_homepage]: https://github.com/aknysh
  [aknysh_avatar]: https://github.com/aknysh.png?size=150


