# The minionrt Spec

## Introduction

This is the specification of minionrt, a common interface between software engineering tools and AI agents.
This includes a set of network protocols, but also specifies the runtime environment and distribution format for agentic programs.
The overall goal of this project is to minimize the amount of reinventing the wheel by using existing protocols and standards where possible.
For the most part, this specification can be seen as a composite of existing standards.

> [!WARNING]
> The specification is early work in progress and may change significantly.

## Conformance

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in the normative portions of this document are to be interpreted as described in [IETF RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119).
These key words may appear in lowercase and still retain their meaning unless explicitly declared as non-normative.

## License

The specification is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

## Contents

1. [Overview](overview.md)
1. [HTTP Interface](http.md)
1. [Runtime Environment](runtime.md)
