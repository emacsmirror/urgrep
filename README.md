# Urgrep - Universal Recursive Grep

**Urgrep** is an Emacs package to provide a universal frontend for *any*
grep-like tool, as an alternative to the built-in `M-x rgrep` (and other similar
packages). Currently, [`ugrep`][ugrep], [`ripgrep`][ripgrep], [`ag`][ag],
[`ack`][ack], [`git grep`][git-grep], and [`grep`][grep]/[`find`][find] are
supported.

## Why Urgrep?

#### One package, many tools

No matter which tool you prefer, you can use it with Urgrep. If a new tool comes
along, you won't need to find a new Emacs package for it.

#### Rich minibuffer interface

Easily manipulate per-search options with Isearch-like key bindings within the
Urgrep minibuffer prompt.

#### Seamless support for TRAMP

Each host can use a different set of tools depending on what the system has
installed without any special configuration.

## Using Urgrep

The primary entry point to Urgrep is the interactive function `urgrep`. This
prompts you for a query to search for in, by default, the root directory of the
current project (or the `default-directory` if there is no project). By
prefixing with <kbd>C-u</kbd>, this will always start the search within the
`default-directory`. With <kbd>C-u</kbd> <kbd>C-u</kbd>, Urgrep will allow you
to enter the base directory. Within the search prompt, there are several
Isearch-like key bindings to let you modify the search's behavior:

| Key binding                 | Action                                   |
|:----------------------------|:-----------------------------------------|
| <kbd>M-s</kbd> <kbd>r</kbd> | Toggle regexp search                     |
| <kbd>M-s</kbd> <kbd>c</kbd> | Toggle case sensitivity                  |
| <kbd>M-s</kbd> <kbd>f</kbd> | Set wildcards to filter files¹           |
| <kbd>M-s</kbd> <kbd>C</kbd> | Set number of lines of context²          |
| <kbd>M-s</kbd> <kbd>B</kbd> | Set number of lines of leading context²  |
| <kbd>M-s</kbd> <kbd>A</kbd> | Set number of lines of trailing context² |

> 1. Prompts with a recursive minibuffer<br>
> 2. With a numeric prefix argument, set immediately; otherwise, use a recursive
>    minibuffer

In addition to the above, you can call `urgrep-run-command`, which works like
`urgrep` but allows you to manually edit the command before executing it.

### Configuring the tool to use

By default, Urgrep tries all search tools it's aware of to find the best option.
To improve performance, you can restrict the set of tools to search for by
setting `urgrep-preferred-tools`:

```elisp
(setq urgrep-preferred-tools '(git-grep grep))
```

If a tool is installed in an unusual place on your system, you can specify this
by providing a cons cell as an element in `urgrep-preferred-tools`:

```elisp
(setq urgrep-preferred-tools '((ag . "/home/coco/bin/ag")))
```

This also works with connection-local variables:

```elisp
(connection-local-set-profile-variables 'urgrep-ripgrep
 '((urgrep-preferred-tools . (ripgrep))))

(connection-local-set-profiles
 '(:application tramp :machine "imagewriter") 'urgrep-ripgrep)
```

## Programmatic interface

In addition to interactive use, Urgrep is designed to allow for programmatic
use, returning a command to execute with the specified query and options:
`urgrep-command`. This takes a `query` as well as several optional keyword
arguments. For more information, consult the docstring for `urgrep-command`.

## Contributing

Feedback is welcome, but it's a bit early for code contributions. Thanks for the
thought, though! (Hopefully this will change after not too long.)

[ugrep]: https://github.com/Genivia/ugrep
[ripgrep]: https://github.com/BurntSushi/ripgrep
[ag]: https://github.com/ggreer/the_silver_searcher
[ack]: https://beyondgrep.com/
[git-grep]: https://git-scm.com/docs/git-grep
[grep]: https://www.gnu.org/software/grep/
[find]: https://www.gnu.org/software/findutils/
