# Emacs/Org-Mode

[GNU Emacs](https://www.gnu.org/software/emacs/) is a free and open-source text editor known for its customizability and extensibility. 

[Org-Mode](https://orgmode.org/) is one of the major modes in GNU Emacs. In addition to basic note-taking, Org-Mode also supports to-do lists, computational notebooks, and literate programming, making it ideal for electronic laboratory notebooks.

## Usage
- Launch Emacs
    ```bash
    emacs
    ```
    - [OPTIONS]
        - `-q`: no initial configuration file
        - `-l init.el`: configure with `init.el`
        - `-nw`: no window
- Configure Emacs
    - `~/.emacs.d/init.el`: the configuration file
    - `~/.emacs.d/elpa`: installed packages
        - https://melpa.org/
        - https://stable.melpa.org/
        - https://elpa.gnu.org/packages/
        - https://elpa.nongnu.org/nongnu/
        - https://orgmode.org/worg/org-contrib/
- Quick-start
    - Key abbreviations & keybindings
        - `C` (Ctrl), `M` (Alt), `Spc` (space), `Ret` (Enter), `S` (Shift)
        - `C-x` refers to pressing and holding the Ctrl key, then pressing the x key.
        -  `C-x C-y` refers to pressing and holding the Ctrl key, then pressing the x key and releasing it, then pressing the y key.
    - Fundamental mode
        - `C-x C-f`: open files
        - `C-x C-c`: quit
        - `C-x C-e`: evaluate last symbolic expression
        - `C-x C-0`: rescale to default font size
        - `C-x 1`: close all other buffers
        - `C-x o`: switch to another buffer
        - `C-x b`: switch buffer
            - Current working files
            - `*Message*`: check all buffer messages (`q`: leave buffer)
        - `C-x s`: save buffer
        - `C-Spc`: set the marker
        - `M-w`: copy
        - `C-w`: cut
        - `C-y`: paste
        - `C-/`, `C-_`: undo
        - `C-h`: help menu
            - +`v`: describe variables
            - +`f`: describe functions
            - +`o`: open documentation
        - `M-!`: shell command
        - `M-:`: eval command
        - `M-x`: execute commands (with Tab completion)
            - `eval-buffer`: reload configuration
            - `list-packages`: list all packages
            - `kill-this-buffer` (`C-x k`)
            - `comment-line` (`C-x C-;`): comment or uncomment
            - `imenu` (`M-g i`): menu items
            - `M-o`: command source
            - `check-parens`: check parentheses
            - `quoted-insert` (`C-q`)
            - `term`: terminal
    - Org-Mode
        - `C-Ret`: start a new bullet point
        - `M-arrows`: rearrange items
        - `M-x`
            - `org-insert-link` (`C-c C-l`): add links
            - `org-open-at-point` (`C-c C-o`): open links
            - `org-toggle-checkbox` (`C-c C-x C-b`): modify checkbox status
            - `org-todo` (`C-c C-t` or `S-arrows`): modify todo status, priority
            - `org-schedule` (`C-c C-s`): set schedule
            - `org-set-tags-command`, `counsel-org-tag` (`C-c C-q`): add or remove tags
            - `org-set-property` (`C-c C-x p`): set properties
            - `org-refile` (`C-c C-w`): refile to archive
            - `org-archive-subtree-default` (`C-c C-x C-a`): directly refile to archive
            - `org-capture`
                - `C-c C-c`: finish
                - `C-c C-w`: refile
                - `C-c C-k`: abort
            - `org-toggle-inline-images` (`C-c C-x C-v`): display/hide images
            - `org-mode-restart`: restart Org-Mode
        - [Org-Tempo](https://orgmode.org/manual/Structure-Templates.html#FOOT161) (`org-tempo`)
            - `<-*-Tab`: call template
            - `C-c '` (org-edit-special): code editing buffer
        - [Org-Babel](https://orgmode.org/worg/org-contrib/babel/intro.html)
            - `C-c C-c`: execute code block
            - `,*`: escape headlines
            - `org-babel-tangle` (`C-c C-v t`): tangle code blocks to the target file, e.g. `emacs-lisp` to `:tangle init.el`
            - `C-u C-c C-v t`: tangle only the selected code block