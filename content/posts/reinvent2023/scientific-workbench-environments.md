---
title: "LFS302 - Building Scientific Workbench Environments"
date: 2023-11-29T11:30:00-07:00
draft: false
---

## Claims (Hypotheses)

* Scientists need a scalable workbench environment. I'm actually not sure I agree with this.
* Emphasis on biology
* Deploying custom algorithms (analysis algorithms, not processing algos)


### Challenges

* Easy and efficient access to data
* Accessing third party data securely
* Allowing scientists to experiment with their data but selectively deploy working algorithms and workflows
* Catering to scientists who are not experts in coding or ML
* Carrying Jupyter notebooks into production deployments (hmmm...)

## Blocker and Challenges

* What is our biggest challenge? Providing scientists a good prototyping environment that we can transition into the production processing environment. Workspaces might be a good solution. Multi-tenant Jupyter could be another.
* Workspaces: lowest common denominator that works for any needs, provided the scientist knows how to set up their own dev environment.
* Jupyter: Easier to use but limited. Scientists will need to use a separate environment to package their code into Docker.