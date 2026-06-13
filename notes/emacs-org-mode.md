## Emacs/Org-Mode

[GNU Emacs](https://www.gnu.org/software/emacs/) is a free and open-source text editor known for its customizability and extensibility. 

[Org-Mode](https://orgmode.org/) is one of the major modes in GNU Emacs. In addition to basic note-taking, Org-Mode also supports to-do lists, computational notebooks, and literate programming, making it ideal for electronic laboratory notebooks.

### Usage
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