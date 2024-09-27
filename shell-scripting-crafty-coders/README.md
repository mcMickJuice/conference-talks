# Shell Scripting

## Common/Useful Commands

### Builtins
grep - finding and filtering, performs text matching
awk - filtering, replacing, logic
find - finding, performs file name matching
cat - print out file contents
echo - output what is input
sed - find and replace
curl - api calls

### Utilities (installable via homebrew)
[jq](https://jqlang.github.io/jq/)
[jqp](https://github.com/noahgorstein/jqp)
[fzf](https://github.com/junegunn/fzf)
[tldr](https://github.com/tldr-pages/tldr)


## Some Use Cases

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

[Script](https://github.com/shipt/segway-next/blob/ab52e404d192c654b5231ef513418f35d0600238/scripts/run-failed-circle-cy-tests-locally.sh)

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

[Script](https://github.com/shipt/segway-scripts/blob/5f06e2511f3e921fa12abc98165a565bc03576da/use_membership_analysis)

Shell/Libraries used:
- `grep` to perform a text search for the `./src` directory where the text `useMembership(` appears, selecting only the filename
- `grep` again to filter out test files
- execute this shell command using the `execSync` node stdlib builtin. This will execute the command in a shell. The outputs of this command is a newline separated string
- split this string to get an array of filenames, and map over each filename, read the file and passing the file contents to a code parser function for analysis

## Resources
- [How to read man pages](https://www.geeksforgeeks.org/man-command-in-linux-with-examples/)
- [DistroTube's Shell Commands Playlist](https://www.youtube.com/playlist?list=PL5--8gKSku174EnRTbP4DzU2W80Q1vqtm)
- [tldr utility](https://github.com/tldr-pages/tldr)
- [jq](https://jqlang.github.io/jq/)
