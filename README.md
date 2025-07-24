# Craft Examples

This repository is home to examples of software and craft projects used in documentation
and tests for Starcraft tools.

All examples are organized by branch. Nothing is merged into `main`.

When adding a new example, use one of the two branch templates.

## Add an example craft project

Craft projects are project files and configurations, like `snapcraft.yaml` and
`rockcraft.yaml`, that work as-is.

Use this branch name format:

```
project/<source-language-or-framework>/<name>
```

Examples:

Branch name | Contents
-|-
`project/rust/oxidizr-snap` | The Snapcraft project files for the oxidizer snap.
`project/go/kubernetes-charm` | The Charmcraft project files for the Kubernetes charm.
`project/cpp/tensorflow-rock` | The Rockcraft project files for the Tensorflow rock.

## Add an example software source

These are the source tree of entire software projects. These examples should not
be specific to any tool, though it is fine if only one tool uses the example.

Use this branch name format:

```
src/<source-language-or-framework>/<name>
```

Examples:

Branch name | Contents
-|-
`src/python/pyfiglet` | The source code for pyfiglet.
`src/java/maven-hello` | The source code for a "Hello, world!" Java app using Maven.
`src/cpp/moon-buggy` | The source code for the Moon Buggy app.
