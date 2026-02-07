# Runtime Environment

## Distributing Agents

Agents should be distributed as OCI container images.

## Running Agents

Tools should execute agents in a runtime environment compatible with the OCI runtime specification.
Tools must set the following environment variables when executing the agent process:

```sh
# The base URL under which the HTTP interface is served by the tool
MINION_API_BASE_URL
# A token that agents can use to authorize themselves against the HTTP interface
MINION_API_TOKEN
```
