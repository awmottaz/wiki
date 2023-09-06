# Recipes for `find`

## Find all top-level node_modules folders in a monorepo

```sh
find -path '*node_modules' -type d ! -path '*node_modules*node_modules*'
```

## Execute a command with everything you found, appending the matched file to the end of the command

```sh
find <query> -exec <command> {} +
```
