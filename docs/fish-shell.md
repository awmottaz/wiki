---
sidebar_position: 2
---

# Fish shell

I like to use the fish shell as my login shell.

## Packages

I use [fisher](https://github.com/jorgebucaran/fisher) to manage packages.

```
$ fisher list
franciscolourenco/done
jorgebucaran/fisher
jethrokuan/z
edc/bass
```

## NVM

To install and integrate NVM...

1. Follow the installation instructions from here: https://github.com/nvm-sh/nvm#installing-and-updating
2. Follow the integration instructions here: https://github.com/nvm-sh/nvm#fish

## Prompt

I've written a fairly minimal but functional custom prompt.

```
$ funced fish_prompt
```

```sh
function fish_prompt
    if test ! $status -eq 0
        printf '%s%s ' (set_color $fish_color_error) (echo '🔴')
    end

    if test -n $CMD_DURATION -a $CMD_DURATION -gt 500
        printf '%s[%s] ' (set_color brblack) (humantime $CMD_DURATION)
    end

    printf '%s%s %s%s\n%s$ ' (set_color $fish_color_cwd) $PWD \
        (set_color cyan) (fish_git_prompt) \
        (set_color $fish_color_normal)
end
```
