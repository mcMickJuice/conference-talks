# Shell Scripting

## Some Scripts

### Transform ESLint Output into CSV

ESLint output is disgusting. I want to transform it into a .csv where each lint violation is tied to the file in which it occurs.

```sh
#!/bin/bash

## remove leading stdout prefix from lint
# group deprecation elements by file
sed 's/\[lint:code:parallel\] //' <lint-raw.txt |
  sed 's/\[[0-9]\] //' |
  awk '{if($0 ~ "/Users") {s=$0} else if($0 ~ "deprecation/deprecation") {printf "%s,%s\n", s, $3}}' |
  sed "s/'//g"
```

Shell commands used:
- `cat` or output redirect via `<` to feed the file contents into the pipe
- `sed` to find and replace line prefixes that we don't want
- `awk` to filter, reduce and concat the filename with the lint violation
- `sed` to remove single quotes, cleaning up our data

### Automate Running of Failing CircleCI Cypress Tests Locally

[Script](https://github.com/shipt/segway-next/blob/ab52e404d192c654b5231ef513418f35d0600238/scripts/run-failed-circle-cy-tests-locally.sh#L31)

### Run Code Analysis for Specific Hooks

## Resources
- [How to read man pages](https://www.geeksforgeeks.org/man-command-in-linux-with-examples/)
- [DistroTube's Shell Commands Playlist](https://www.youtube.com/playlist?list=PL5--8gKSku174EnRTbP4DzU2W80Q1vqtm)
- [tldr utility](https://github.com/tldr-pages/tldr)
- [jq](https://jqlang.github.io/jq/)
