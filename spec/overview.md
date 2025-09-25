# Overview

minionrt connects two kinds of software: agents and developer tools.
With "tools" or "developer tools" we refer to any interfaces or programs that human developers interact with; a tool could be an editor, a command line interface (CLI), a programming language or even a platform such as GitHub.
With "agents" we refer to programs that automate certain software-engineering tasks, usually using large language models (LLMs) in the process.

There have been various attempts to connect agents and developer tools, but most other proposals we are aware of have had at least one of two key shortcomings.
The first shortcoming is a narrow focus on a particular developer tool, thereby often coupling the agentic code to a particular developer interface.
While such a tight coupling makes it easy to provide a good user experience by matching the agentic code to the capabilities of the tool, it is not very efficient:
A separate implementation is needed for each combination of developer tool and agent.
The second shortcoming of other proposals is restricting agentic frameworks to a particular programming language, which immediately limits possible applications.

We propose a different approach that avoids coupling agents to tools or any particular programming language.
Let us look at what that means in practice:

## What does this mean for agents?

* Agents are arbitrary programs written in any language, distributed as OCI container images.
* They access an OpenAI-compatible LLM API proxy, forwarding to a configured provider (e.g. OpenRouter).
* They access a Git proxy that restricts pushing to selected repositories and branches.
* They access a minimal additional HTTP API to retrieve their task and interact with the user.

Note that only the last bullet point is minionrt-specific, the remainder of the interface is a composite of well-known standards and widely used interfaces.

## What does this mean for tools?

Tools do not have to integrate with any particular daemon or separate application.
It is their choice on how to provide a compatible interface and runtime.
We do not even mandate any particular runtime technology; tools can use virtualization, containerization or another form of sandboxing as long as it is compatible with the [OCI standards](https://opencontainers.org/).

Of course, tools and agents *can* use our reference implementation, which is written in Rust with the hope that implementations for other languages can be easily created.
But by no means are they restricted to go that route.
