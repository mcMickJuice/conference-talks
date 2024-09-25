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
- `cat` or [output redirect](https://stackoverflow.com/questions/11710552/useless-use-of-cat) via `<` to feed the file contents into the pipe
- `sed` to find and replace line prefixes that we don't want
- `awk` to filter, reduce and concat the filename with the lint violation
- `sed` to remove single quotes, cleaning up our data

### Automate Running of Failing CircleCI Cypress Tests Locally

[Script](https://github.com/shipt/segway-next/blob/ab52e404d192c654b5231ef513418f35d0600238/scripts/run-failed-circle-cy-tests-locally.sh#L31)

Shell commands used:
- `curl` for the initial API call to Circle CI to get the jobs for a workflow
- `jq` to filter and transform the api response to a single value
- `xargs` to build a command interpolating the value from the input. Kind of like string interpolation
- `curl` to make another API call to Circle CI to get the results of a specific job
- `jq`to filter and map API response to get a url for the cypress/logs artifact
- `xargs` to take the above url and fetch the artifact file which contains the cypress error logs
- `jq` to map just the `specName` of the failing tests
- `awk` to turn these multiple lines into a single string, kind of like reduce on an Array in Javascript
- `xargs` to run `yarn cypress run` with a comma separated list of failing spec files

### Run Code Analysis for Specific Hooks

## Resources
- [How to read man pages](https://www.geeksforgeeks.org/man-command-in-linux-with-examples/)
- [DistroTube's Shell Commands Playlist](https://www.youtube.com/playlist?list=PL5--8gKSku174EnRTbP4DzU2W80Q1vqtm)
- [tldr utility](https://github.com/tldr-pages/tldr)
- [jq](https://jqlang.github.io/jq/)
