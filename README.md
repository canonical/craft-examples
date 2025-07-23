# Craft Examples

This repository is home to code examples used in documentation and tests for the
Starcraft tools.

## Creating an example

All examples should be categorized using branch names. That is, nothing should get
merged into `main`! When creating a new example, use one of the two branch templates.

#### `project/<lang>/<name>`
This should be used for any project files that should work as-is. Multiple project
files for different tools can co-exist in these branches, but if you find yourself
adding multiple project files for a single tool into one branch then you likely
would be better off adding it into another branch.

Examples:
* `project/python/craft-cli`
* `project/golang/chisel`
* `project/rust/oxidizr`

#### `src/<lang>/<name>`
This should be used for full source trees for projects. These examples should not
be specific to any tool, though it is fine if only one tool uses the example.

Examples:
* `src/java/maven-hello`
* `src/cpp/moon-buggy`
* `src/python/pyfiglet`
