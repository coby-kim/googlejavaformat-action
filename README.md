# Google Java Format Action

Automatically format your Java files using [Google Java Style guidelines](https://google.github.io/styleguide/javaguide.html).

This action automatically downloads the latest release of the [Google Java Format](https://github.com/google/google-java-format) program.

This action can format your files and push the changes, or just check the formatting without committing anything.

You must checkout your repository with `actions/checkout` before calling this action (see the example).

## Examples

Format all Java files in the repository and commit the changes:

```yml
# Example workflow
name: Format

on:
  push:
    branches:
      - master

jobs:

  formatting:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2 # v2 minimum required
      - uses: axel-op/googlejavaformat-action@v3
        with:
          args: "--skip-sorting-imports --replace"
          # Recommended if you use MacOS:
          # githubToken: ${{ secrets.GITHUB_TOKEN }}
```

Check if the formatting is correct without pushing anything:

```yml
# Example workflow
name: Format

on: [push, pull_request]

jobs:

  formatting:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2 # v2 minimum required
      - uses: axel-op/googlejavaformat-action@v3
        with:
          args: "--set-exit-if-changed"
```

## Inputs

None of these inputs is required, but you can add them to change the behavior of this action.

### `githubToken`

**Recommended if you execute this action on MacOS**. Due to [this issue](https://github.com/actions/virtual-environments/issues/602), calling the GitHub API from a MacOS machine can result in an error because of a rate limit. To overcome this, provide the [`GITHUB_TOKEN`](https://docs.github.com/en/actions/configuring-and-managing-workflows/authenticating-with-the-github_token) to authenticate these calls. If you provide it, it will also be used to authenticate the commits made by this action.

### `version`

Set this input to use a [specific version of Google Java Format](https://github.com/google/google-java-format/releases). For example: `1.7`, `1.8`...

### `files`

A pattern to match the files to format. The default is `**/*.java`, which means that all Java files in your repository will be formatted.

### `skipCommit`

Set to `true` if you don't want the changes to be committed by this action. Default: `false`.

### `commitMessage`

You can specify a custom commit message. Default: `Google Java Format`.

### `args`

The arguments to pass to the Google Java Format executable.
By default, only `--replace` is used.

```console
-i, -r, -replace, --replace
  Send formatted output back to files, not stdout.

--assume-filename, -assume-filename
  File name to use for diagnostics when formatting standard input (default is <stdin>).

--aosp, -aosp, -a
  Use AOSP style instead of Google Style (4-space indentation).

--fix-imports-only
  Fix import order and remove any unused imports, but do no other formatting.

--skip-sorting-imports
  Do not fix the import order. Unused imports will still be removed.

--skip-removing-unused-imports
  Do not remove unused imports. Imports will still be sorted.

--skip-reflowing-long-strings
  (JDK 11+) Do not reflow string literals that exceed the column limit.

--skip-javadoc-formatting
  (JDK 11+) Do not reformat javadoc.

--dry-run, -n
  Prints the paths of the files whose contents would change if the formatter were run normally.

--set-exit-if-changed
  Return exit code 1 if there are any formatting changes.

--length, -length
  Character length to format.

--lines, -lines, --line, -line
  Line range(s) to format, like 5:10 (1-based; default is all).

--offset, -offset
  Character offset to format (0-based; default is all).
```

Note:

- If you add `--dry-run` or `-n`, no commit will be made.
- The argument `--set-exit-if-changed` will work as expected and this action will fail if some files need to be formatted.
