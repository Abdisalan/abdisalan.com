---
title: "Haskell Language Server on Doom Emacs"
date: "2024-11-26T14:27:22.169Z"
template: "post"
draft: false 
slug: "/posts/haskell-lsp-doom-emacs"
description: "Quick setup for Haskell on emacs that I use."
category: "Dev Setup"
tags:
  - "Haskell"
  - "Emacs"
socialImage: "media/20241126-DoomHaskellLsp.png"
---

![Emacs with haskell code using the lsp to run autocomplete](media/20241126-DoomHaskellLsp.png)

> Doom Emacs with the doom-one-light theme

# Quick summary

  - This assumes you already have Doom Emacs installed.
  - We'll need to install ghcup which is a Haskell toolchain.
  - And lastly we'll make small changes to your doom config file

# Install GHCup

Follow the install instructions from the official source <https://www.haskell.org/ghcup/>

Run the terminal user interface.
```
ghcup tui
```

Use the up and down arrows to pick your desired version and install by pressing `i`, then make sure to also press `s` after installing to &ldquo;set&rdquo; that version.

![GHCup running in terminal ui mode](/media/20241126-ghcup_tui.png)


# Doom init.el

Within your doom folder (usually located in `~/.doom.d/`) open your `init.el` file.
Make sure to uncomment this line and then run `doom sync` in your terminal to install the packages.

```elisp
(haskell +lsp)    ; a language that's lazier than I am
```


## eglot vs lsp-mode

I am familiar with eglot especially since it's been added to emacs' core in version 29. I'm sure the haskell language server will work with it but I haven't put in the time to set that up and I've got it working with lsp-mode anyway 🤷🏾‍♂️


# Doom config.el

Next open the `config.el` file, also located in your doom folder. We'll be adding two snippets.

The first is setting up hooks to turn on the language server when emacs detects `haskel-mode`.

```elisp
;; HASKELL
;; Hooks so haskell and literate haskell major modes trigger LSP setup
(add-hook 'haskell-mode-hook #'lsp)
(add-hook 'haskell-literate-mode-hook #'lsp)
```


## Supply environment variables

If you start emacs through a GUI like me, your emacs will likely not have the same environment as your terminal. This will cause an issue where emacs cannot find the `haskell-language-server` program you installed with `ghcup`.

For example:

```
Failed to find a HLS version for GHC 9.4.5
Executable names we failed to find: haskell-language-server-9.4.5,haskell-language-server
```

This second snippet will ensure that the language server binaries will be available to emacs. It adds the ghcup bin folder to the `exec-path` list and the `PATH` environment variable. Put this code block in your config.el as well!

```elisp
;; Put this in your emacs config
;; Add haskell lsp to path for emacs subprocesses
(add-to-list 'exec-path (expand-file-name "~/.ghcup/bin"))
;; Add haskell tools to path for emacs environment
(setenv "PATH" (concat (expand-file-name "~/.ghcup/bin") ":" (getenv "PATH")))
```

Hope this helps!
