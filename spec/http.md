# HTTP Interface

This page describes the HTTP interface that agents and developer tools communicate over.
In line with the rest of this specification, it is a composite of existing standards and well-known interfaces.

## Routes

Tools must provide a single HTTP interface which provides the following set of routes:

* `/chat/completions` - OpenAI-compatible LLM chat completions API
    * `POST /chat/completions`
* `/agent/task` - Task management API
    * `GET /agent/task`
    * `POST /agent/task/complete`
    * `POST /agent/task/fail`

When, as part of the minionrt interface, a tool provides a git repository URL, this URL must use the `http` or `https` scheme.
Let `URL` stand for such a URL.
The tool must then serve the Git v2 wire protocol via the smart HTTP transfer protocol under `URL` with the following routes:

* `GET URL/info/refs`
* `POST URL/git-receive-pack`
* `POST URL/git-upload-pack`

## Chat Completions API

The tool must accept `POST` requests to the `/chat/completions` endpoint.

> [!WARNING]
> This API has yet to be fully specified.
> For now, the tool should follow the schema implemented by [OpenRouter](https://openrouter.ai/docs/api-reference/overview), another OpenAI-compatible API.

## Task Management API

### `GET /agent/task`

Agents should use this endpoint to fetch the task they should solve.

**Request body:** No body.

**Response body:** JSON.

```ts
type Response = {
  /** The status of the task */
  status: TaskStatus;
  /** A natural-language description of the task */
  description: string;
  /** The git username the agent should use for commits
   * The tool should not provide use a human user name here.
   * Instead, it should provide a name that identifies the tool to git services relevant to the tool.
  */
  git_user_name: string;
  /** The git email address the agent should use for commits
   * The tool should not provide a human email address here.
   * Instead, it should provide an address that identifies the tool to git services relevant to the tool.
  */
  git_user_email: string;
  /** The URL of the git repository, served or proxied by the tool */
  git_repo_url: string;
  /** The git branch the agent should checkout */
  git_branch: string;
};

type TaskStatus = "Queued" | "Running" | "Completed" | "Failed";
```

> [!NOTE]
> This API will be changed to support multiple repositories (and potentially other resources in the future).

### `POST /agent/task/complete`

Agents should use this endpoint once a task has been completed successfully.

**Request body:** JSON.

```ts
type Request = {
    /** A natural-language description of the result */
    description: string;
}
```

**Response body:** No body.

### `POST /agent/task/fail`

Agents should use this endpoint once they determined that solving a task has failed.

**Request body:**

```ts
type Request = {
  /** Optional: The reason for the task failure */
  reason?: TaskFailureReason;
  /** A natural-language description of the task failure */
  description: string;
};

enum TaskFailureReason {
  /**
   * The agent failed to complete the task due to technical problems unrelated to the task itself.
   */
  TechnicalIssues = "TechnicalIssues",
  /**
   * The agent failed to complete the task due to a problem with the task itself, e.g. because
   * the task is unclear or impossible to complete.
   */
  TaskIssues = "TaskIssues",
  /**
   * There were no fundamental technical issues and the task was valid, but the agent still failed
   * to complete the task because it did not succeed at task-specific problem-solving.
   */
  ProblemSolving = "ProblemSolving",
}
```

**Response body:** No body.

## Git Protocol

When, as part of the minionrt interface, a tool provides a git repository URL, this URL must use the `http` or `https` scheme.
Let `URL` stand for such a URL.
The tool must then serve the Git v2 wire protocol via the smart HTTP transfer protocol under `URL` with the following routes:

* `GET URL/info/refs`
* `POST URL/git-receive-pack`
* `POST URL/git-upload-pack`

For more details on the Git HTTP protocol and the wire protocol v2, see:

* [Git HTTP protocol documentation](https://git-scm.com/docs/http-protocol)
* [Git wire protocol v2 documentation](https://git-scm.com/docs/protocol-v2)

If agents make git requests to the provided URLs, they too must use the Git v2 wire protocol via the smart HTTP transfer protocol.
