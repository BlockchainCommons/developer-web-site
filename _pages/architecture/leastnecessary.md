---
cover: false
header:
  onnverlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: "Design Principles: Least & Neccessary"
tagline: "Minimization to Improve Security"
hide_description: true
classes:
  - wide
permalink: /architecture/least-necessary"
redirect_from:
  - /least-necessary/
  - /leastnecessary/
  - /architecture/leastnecessary/
sidebar:
  nav:
    - archdesign
    - architecture
---

_Least & Necessary are a set of design principles that were developed
from core computer security principles, which themselves were founded
long ago with the decision to replace "root" (or admin) accounts with
user-level accounts that have lesser permissions. They improve
security by lowering risk of exploit and reducing the damage of
penetration._

## Last Privilege, Authority & Access

The original principle of "Least Privilege" says that you minimize
permissions given to a user. That was extended to the idea of "Least
Authority," which looks at things transitively: you minimize
permissions that the user might accrue through other means, such as
programs that they can run that might themselves have certain
privileges.

Those are primarily system/security-based design patterns, but they
lead us to "Least Access", where you similarly limit access to
content, following the policy of [Data
Minimization](/architecture/data-minimization/).

* **Least Privilege.** Minimize permissions to what’s necessary for a task.
* **Least Authority.** Minimize permissions, but consider transitive authority.
* **Least Access.** Minimize data access to what’s necessary for a task, considering transitive authority.

## Necessary Privilege, Authority & Access

An alternative principle says that instead of minimizing permissions
and access you instead start from zero and provide exactly what's
necessary. This is a bottom-up pattern rather than a top-down pattern.

* **Necessary Privilege.** Provide permissions necessary for a task.
* **Necessary Authority.** Provide permissions and consider transitive authority.
* **Necessary Access.** Provide data access necessary for a task, considering transitive authority.

Choosing between Least and Necessary as principles depends on the
scope of your design. If it has very specific needs, then you can
probably use the Necessary principles, but if it's more generalist,
where users might need to do a large variety of things, then you
instead probably need to design with the Least principles.

## The Purpose of Least & Necessary

With any system, you must presume that at some point you will be
compromised. The goal of the Least & Necessary design principles is to
minimize the damage of such compromises.

It's particular important to protect the most fundamental systems,
such as the ones that allow you to change access permissions. If those
are only used in very limited situations, then a comrpromise can
usually be reversed.

## Least & Necessary Links

**Articles:**

* [**Least & Necessary Design Patterns**](https://www.blockchaincommons.com/musings/Least-Necessary/) (Musings)

**Related Principles:**

* [**Data Minimization**](/architecture/data-minimization/)
* [**Homoegenity**](/architecture/homogeneity/)
