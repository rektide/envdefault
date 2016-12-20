# envdefault

> Set **defaults** for **environment variables** not already set.

Utility to run a POSIX shell script & report any variables set in that subshell that were not already set: this utility helps load environment variables that may have been missing, loads with priority lower than those already loaded.

# Usage

envdefault expects either a single script to source, or, stdin which it will source. After sourcing the content in a subshell, it will print out (& source) all new variables exported.

## Invoke as a program

```
export FOO=bar
echo export FOO=baz DOG=shibe > defaults
envdefault defaults
```

Would output `export DOG=shibe`.

After sourcing `defaults` (securely, inside a temporary subshell), envdefault finds:
1. The variable FOO had already been set, so it can be ignored.
1. The variable DOG is new, so print that value (and export it).

One can directly [Source](#Source) envdefault, but one can also imagine using envdefault via eval- `eval $(envdefault defaults)` as a namesake way to load environment variables at a low, "default" priority.

## Stdin

Absent any arguments, envdefault accepts **stdin** as the source for it's subshell. The invoke example might become: 

```
export FOO=bar
echo export FOO=baz DOG=shibe | envdefault
```

To output the same `export DOG=shibe` result. Again, FOO was already set so not reported, but DOG is new, so it's default is echoed.

## Source

Alternatively, in a large variety of shells, one can **source** envdefault: `echo export FOO=baz DOG=shibe > defaults ; . envdefault defaults`.
