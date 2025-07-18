# Faster `clear` command for the CLI

Replace the default clear command with an alias that directly sends ANSI escape codes to clear the terminal screen faster:

```sh
alias clear='printf "\033[H\033[2J"'
alias c='printf "\033[H\033[2J"'
```

Explanation:

* `\033[H` – moves the cursor to the top-left corner.
* `\033[2J` – clears the entire screen.

The regular clear command is an actual executable, which adds a small delay. Using printf with escape codes is faster because it avoids that overhead.
