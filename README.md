<!-- This file was automatically generated by the `build-harness`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

[![Cloud Posse](https://cloudposse.com/logo-300x69.png)](https://cloudposse.com)

# Packages  [![TravisCI Build Status](https://travis-ci.org/cloudposse/packages.svg?branch=master)](https://travis-ci.org/cloudposse/packages) [![Codefresh Build Status](https://g.codefresh.io/api/badges/build?repoOwner=cloudposse&repoName=packages&branch=master&pipelineName=packages&accountName=cloudposse&type=cf-1)](https://g.codefresh.io/repositories/cloudposse/packages/builds?filter=trigger:build;branch:master;service:5b234974667ab79287990636~packages) [![Latest Release](https://img.shields.io/github/release/cloudposse/packages.svg)](https://github.com/cloudposse/packages/releases) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Cloud Posse distribution of native apps.

Use this repo to easily install binary releases of popular apps. This is useful for inclusion into a `Dockerfile` to install dependencies.


---

This project is part of our comprehensive ["SweetOps"](https://docs.cloudposse.com) approach towards DevOps. 


It's 100% Open Source and licensed under the [APACHE2](LICENSE).




## Usage

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
Available targets:

  aws-vault                           Install aws-vault to easily assume roles (not related to HashiCorp Vault)
  chamber                             Install Chamber to manage secrets with SSM+KMS
  fetch                               Install fetch to easily download files, folders, and release assets from a specific git commit, branch, or tag
  github-commenter                    Install github-commenter
  gomplate                            Install gomplate
  goofys                              Install goofys
  helm                                Install helm
  helmfile                            Install helmfile to easily deploy collections of helm charts
  htmltest                            Install htmltest
  htmltest-darwin                     Install htmltest (darwin)
  htmltest-linux                      Install htmltest (linux)
  hugo                                Install hugo framework for building static websites.
  hugo-darwin                         Install hugo framework for building static websites (darwin)
  hugo-linux                          Install hugo framework for building static websites (linux)
  kops                                Install kops
  kubectl                             Install kubectl
  kubectx                             Install kubectx
  kubens                              Install kubens
  sops                                Install sops (required by `helm-secrets`)
  stern                               Install stern multi pod and container log tailing for Kubernetes
  terraform                           Install Terraform
  terraform-docs                      Install terraform-docs to generate docs from terraform modules
  terragrunt                          Install terragrunt for tools that make it easier to work with multiple Terraform modules
  yq                                  Install YQ to manipulte YAML

```



## Related Projects

Check out these related projects.

- [build-harness](https://github.com/cloudposse/build-harness) - Collection of Makefiles to facilitate building Golang projects, Dockerfiles, Helm charts, and more
- [geodesic](https://github.com/cloudposse/geodesic) - Geodesic is the fastest way to get up and running with a rock solid, production grade cloud platform built on strictly Open Source tools.


## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/packages/issues), send us an [email][email] or join our [Slack Community][slack].

## Commerical Support

Work directly with our team of DevOps experts via email, slack, and video conferencing. 

We provide *commercial support* for all of our [Open Source][github] projects. As a *Dedicated Support* customer, you have access to our team of subject matter experts at a fraction of the cost of a fulltime engineer. 

- **Questions.** We'll use a Shared Slack channel between your team and ours.
- **Troubleshooting.** We'll help you triage why things aren't working.
- **Code Reviews.** We'll review your Pull Requests and provide constructive feedback.
- **Bug Fixes.** We'll rapidly work to fix any bugs in our projects.
- **Build New Terraform Modules.** We'll develop original modules to provision infrastructure.
- **Cloud Architecture.** We'll assist with your cloud strategy and design.
- **Implementation.** We'll provide hands on support to implement our reference architectures. 

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

[![Cloud Posse](https://cloudposse.com/logo-300x69.png)](https://cloudposse.com)

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We love [Open Source Software](https://github.com/cloudposse/)!

We offer paid support on all of our projects.  

Check out [our other projects][github], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.

  [docs]: https://docs.cloudposse.com/
  [website]: https://cloudposse.com/
  [github]: https://github.com/cloudposse/
  [jobs]: https://cloudposse.com/jobs/
  [hire]: https://cloudposse.com/contact/
  [slack]: https://slack.cloudposse.com/
  [linkedin]: https://www.linkedin.com/company/cloudposse
  [twitter]: https://twitter.com/cloudposse/
  [email]: mailto:hello@cloudposse.com


### Contributors

|  [![Erik Osterman][osterman_avatar]](osterman_homepage)<br/>[Erik Osterman][osterman_homepage] | [![Igor Rodionov][goruha_avatar]](goruha_homepage)<br/>[Igor Rodionov][goruha_homepage] | [![Andriy Knysh][aknysh_avatar]](aknysh_homepage)<br/>[Andriy Knysh][aknysh_homepage] |
|---|---|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: http://s.gravatar.com/avatar/88c480d4f73b813904e00a5695a454cb?s=144
  [goruha_homepage]: https://github.com/goruha/
  [goruha_avatar]: http://s.gravatar.com/avatar/bc70834d32ed4517568a1feb0b9be7e2?s=144
  [aknysh_homepage]: https://github.com/aknysh/
  [aknysh_avatar]: https://avatars0.githubusercontent.com/u/7356997?v=4&u=ed9ce1c9151d552d985bdf5546772e14ef7ab617&s=144


