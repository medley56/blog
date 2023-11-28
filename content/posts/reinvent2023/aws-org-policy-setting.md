---
title: "SEC218 - Setting Up AWS Org Policies"
date: 2023-11-28T12:30:00-07:00
draft: false
---

Managing AWS IAM permissions centrally using AWS Orgs.

## Background

* Orgs allow centralized control of accounts from a top level account.
* Customize your environment by applying policies from the Org account.
* Centralizes billing.
* Organization > Organization root > Organizational unit > Account
* Policies attach to the whole Org, OUs, or individual accounts
* Policies work on an inheritance model. Higher level policies seem to trump lower level policies.

## Best Practices for Orgs

* Use many accounts (they are free, duh).
* Structure the Org for ease of management, _not_ like your org chart
* Don't run _anything_ workload-related in the management account. Also, lock it down hard.

## Organization Policy Types

### Authorization Policies

People are using these. Lots of hands.

* SCPs
* Plan to add others

### Management Policies

Less commonly used because they are newe. _Very_ few hands went up.

* Tag policy
    * Enforce tagging policy to help standardize
    * Note: current error message when accounts fail the policy are cryptic. "Getting better"
    * Only supports a handful of AWS resources. Does not even include S3.
    * These do not force users to use tags. The only way to force resource creation to use tags is to apply a SCP.
    * There is no visibility into which values are allowed. *facepalm*
    * If you already have existing resources that are not tagged and a policy is instituted, even cloudformation rollbacks will fail due to the incorrect (existing) tags.
    * This seems terrible. 
* Backup policy - Centralized backup plan for all accounts under the policy
    * Set backup vault (exists in a region but can be used across regions)
    * Filter by tags
    * Set maintenance window
    * Allows 10 separate backup policies per attach point (Org, OU, Account)
* AI opt-out policy

## Service Control Policy Best Practices

* Sets the "maximum allowable actions" in an AWS Org, OU, or Account.
* Limit of 5 SCPs per attachment point
* Apply to all IAM Roles, IAM Users, and root users in Org member accounts.
* Applies immediately to all IAM auth decisions once it is applied.
* SCPs require explicit ALLOW policies (comes with a default allow all). Effectively the evaluation does an AND between existing IAM policies and SCP policy.
* Allow-list approved AWS services. Keep it high level.
* Deny-list undesirable actions. Be specific, lest you prevent people from doing actual work.
* Use OUs to manage policy application to accounts. But remember an account can only belong to single OU.

### Example Usecase

* Block usage of regions you don't want to use
    * But never block regions that are required for specific services (e.g. us-east-1 is used for lots of things)
* Block root account for doing things
* Prevent backup deletion (may affect backup lifecycle manager). Does not apply to actions taken by Service Principals.

## Questions

1. Can the management account be deployed in an OU? No. It is always the root of the Organization. In fact, you cannot even apply policies to the management account. "All principals in the management account are not subject to Service Control Policies (SCP)."