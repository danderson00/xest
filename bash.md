# `bash` Tricks

## Snippets

Snippet|Description
---|---
`$(dirname $0)/script`|Execute `script` from the same directory as the executing script
`readlink -f $(dirname $0)/..`|Obtain parent directory relative to executing script
`ls -t | head -n1`|Get most recently modified file from directory

## Commands

### Clean up `node_modules`

```shell script
find . -name "node_modules" -type d -prune -exec rm -rf '{}' +
```

### Global search and replace

Replace all instances of `find` with `replace` in all files with a file extension of `.txt` in the current directory
or the directory below (`-maxdepth 2`). 

```shell script
find . -maxdepth 2 -name *.txt -print -exec sed -i 's/find/replace/g' {} \;
```