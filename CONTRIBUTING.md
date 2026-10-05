# Contributing to SoulFireBypass

SoulFireBypass integrates with Velocity and BungeeCord to permit configured test clients to bypass online authentication.
This guide covers local development, validation, and pull requests.

## Before you start

Search [open and closed issues](https://github.com/soulfiremc-com/SoulFireBypass/issues) before reporting a problem or proposing a feature.
Small fixes can go directly to a pull request. Discuss substantial changes in an issue before implementation.
For usage questions and issue routing, read [SUPPORT.md](SUPPORT.md).
Follow the [community code of conduct](https://github.com/soulfiremc-com/.github/blob/main/CODE_OF_CONDUCT.md).
Report vulnerabilities privately through the [security policy](https://github.com/soulfiremc-com/.github/blob/main/SECURITY.md).

## Prepare and build

Install Git and a JDK 25 to match the Gradle toolchain and CI.
The pinned [Velocity 4 API requires Java 25](https://docs.papermc.io/velocity/faq/). The build compiles with `--release 25`.
Run test proxies on Java 25 as well.
Use the checked-in wrapper rather than a separate Gradle installation:

```bash
./gradlew build test
```

On Windows, use `gradlew.bat`.
The JAR appears under `build/libs/`. Install it in an owned test proxy's plugin directory and restart the proxy.
Set a private test key in the generated plugin configuration before checking bypass behavior.
The `ConfigureMe` placeholder is excluded by the plugin.

## Source layout

- `src/main/java/com/soulfiremc/soulfirebypass/velocity/`: Velocity initialization and login handling.
- `src/main/java/com/soulfiremc/soulfirebypass/bungee/`: BungeeCord initialization and handshake handling.
- `src/main/java/com/soulfiremc/soulfirebypass/SFBypassHelpers.java`: shared handshake helpers.
- `src/main/resources/config.yml`: configuration defaults.
- `src/main/templates/`: generated build constants. Edit the template rather than its generated output.

## Change and verify authentication behavior

Keep Velocity and BungeeCord behavior aligned unless you explain a platform-specific difference.
The integrations access proxy internals through reflection. Include the exact proxy build in compatibility reports.
Use `.editorconfig`, existing Java naming, and the Java 25 API target.
Add focused unit tests for complex helper or validation logic where practical.
There is currently no dedicated unit test suite.

Run `./gradlew build test` before submission.
For login or key changes, use a local proxy and test server to verify:

- A configured valid key permits the intended test connection.
- Missing and invalid keys retain ordinary online authentication.
- Duplicate key-prefix handling does not accidentally permit bypass.
- An ordinary authenticated player can still connect.
- Both Velocity and BungeeCord work, or the report explicitly states the platform not tested.

Keep the test network isolated from public players.
Keys grant authentication bypass. Never put real keys in public issues, screenshots, logs, or examples.
Report unintended authentication bypass through the private security channel.

## Submit a pull request

Keep the change focused on one problem. Avoid unrelated formatting and dependency updates.
Use Conventional Commit subjects such as `docs(contributing): clarify local setup` or `fix(build): correct packaging`.
Use a meaningful scope, imperative wording, and a subject under 72 characters.
For non-trivial changes, add a body that explains the motivation and important tradeoffs.
For breaking changes, include a `BREAKING CHANGE:` footer and migration instructions.
Do not bypass Git hooks. Let all configured checks finish.

Complete the pull request template with the problem, resulting behavior, and affected files.
If a related issue exists, link it.
Use `Closes #123` only if the change fully resolves that issue.
Record build results, proxy versions, and authentication checks on each affected platform.
Explain any checks that you could not perform.
For visible changes, include screenshots and the environment used to capture them.
Open a draft for early feedback on substantial changes.
Respond to review comments and rerun affected checks after revisions.

Update documentation and examples with behavior changes. Remove obsolete code rather than leaving placeholders or shims.
Do not commit credentials, private logs, dependency directories, or generated build artifacts.
Respect existing license notices and submit only material that you have the right to contribute.
