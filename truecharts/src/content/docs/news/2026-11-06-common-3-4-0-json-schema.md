---
slug: "news/truecharts/common-3-4-0-json-schema"
title: "Common 30.4.0: JSON Schema for values.yaml"
authors: [crowcontrol]
date: 2026-11-06
tags:
  - "2026"
  - "announcement"
---

## Common 30.4.0 Released: values.yaml Now Has a JSON Schema

We are happy to announce the release of **common 30.4.0**, the shared Helm library behind every TrueCharts chart.
The headline feature of this release is a complete **JSON schema for `values.yaml`**.

## What Is a JSON Schema?

A JSON schema is a formal, machine-readable description of what a configuration file is allowed to look like.
It lists every valid key, the type of each value, which values are allowed, and which fields are required.
Helm checks your values against this schema whenever you install, upgrade, lint or template a chart.

## Why This Matters

- **Mistakes are caught early.** Typos, wrong nesting and wrong types are reported before anything reaches your cluster.
- **Clear error messages.** Helm tells you which key is wrong and why, instead of leaving you to guess why something does not behave as expected.
- **Better editor support.** Editors with YAML language server support (such as VS Code) can offer autocompletion,
  inline documentation and live validation while you write your values.
- **One source of truth.** Users, contributors and CI tooling all validate against the same definition of what a valid `values.yaml` looks like.

## Easier for AI and Other Tools

The structure and requirements of TrueCharts `values.yaml` files are now described in a formal, machine-readable way.
That makes it much easier for AI assistants, generators and other automation to understand our charts.
Instead of guessing from examples, they can read the schema, produce valid configuration and check their own output.
This means fewer invented keys and more reliable AI-assisted configuration.

## Documentation Generated From the Schema

The complete **common-2026 documentation is now generated directly from the schema**.
Because the docs and the schema come from the same source, they can no longer drift apart.
That means no more stale common-2026 docs: when an option changes, the documentation changes with it.

## Much Stronger Coverage Checking

We also significantly improved coverage checking.
Documentation, unit tests and the schema itself now all have extensive coverage tests.
These checks make sure every option is defined in the schema, documented and tested.
This greatly reduces the chance of holes in our testing and documentation,
and helps us catch missing pieces before they reach a release.

## Improved Security

Configuration mistakes are a real source of security problems.
Before, a mistyped key for something like a security context, a resource limit or a network policy could be silently ignored,
leaving you with a default you never intended.
With the schema, unknown keys are rejected, and types and allowed values are enforced.
Malformed input is stopped before it is ever templated, so the settings you write are the settings that actually apply.

## A Large Amount of Work

Creating this schema was a big undertaking.
Every option in the common library had to be described, typed and documented,
and then checked against the existing templates and tests.
Along the way we found and fixed many inconsistencies.
Thank you to everyone who helped get this over the line.

## Preparing for common-2026

This release is also preparation for the big **common-2026** release, which is coming soon.
The schema gives that release a stable, validated contract to build on.

## ⚠️ Important: Upgrade Notice

Technically, this release is **non-breaking**: valid configuration keeps working exactly as before.

However, the schema changes how mistakes are handled.
Many errors in `values.yaml` that used to be **silently ignored** will now cause a **hard failure**.
If your values contain mistakes, such as typos, wrong nesting, wrong types or leftover keys that no longer exist,
you may be confronted with failing upgrades.

**How to migrate:** check the correctness of your `values.yaml` manually **before** upgrading.
Look for typos and outdated keys, and run `helm lint` or `helm template` with your values to see any errors up front.
If an upgrade does fail, the error message names the offending key. Fix it and try again.

## Thank You

Thank you to all contributors who help build and maintain TrueCharts, and to all users for the feedback and support that drive the project forward.

For more information or to get involved, visit: [https://trueforge.org](https://trueforge.org)
