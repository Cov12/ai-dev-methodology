# Principle 11: Environment Portability

## Principle

Version control carries code state. It does not carry environment state. Identify the machine-bound state your project depends on, and commit an explicit reproduction path for it alongside the code.

## Why this matters

When environment state is undocumented, every new machine rediscovers the same gaps at the worst possible time: when trying to run a real deliverable.

## Where this came from

This principle came from a cross-machine provisioning failure. One machine had the necessary runtimes, CLIs, and authenticated state. The second machine had the code, but not the environment required to run it. The repo was intact. The environment was not.

The lesson is not “remember to check your setup.” The lesson is: if the project does not have a committed reproduction path, environment checks will always be improvised.

## What belongs in a reproduction path

At minimum, commit a setup runbook that covers:

- runtimes and package managers
- required CLI tools
- how to acquire credentials without storing them in git
- required environment variables and where to set them
- a smoke command that proves the environment works
