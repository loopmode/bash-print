# bash-print

Helper for printing messages in colored blocks.

<img src="https://github.com/loopmode/bash-print/raw/master/demo.gif" />

## Usage:

Include the script in your bash script, for example using `source`.
Then, use `print_info`, `print_success` or `print_error`.

- `print_info`: Prints message in a <span style="background: deepskyblue; color: white; padding: 0px 3px 1px;">blue</span> block
- `print_success`: Prints message in a <span style="background: limegreen; color: white; padding: 0px 3px 1px;">blue</span> block
- `print_error`: Prints message in a <span style="background: darkred; color: white; padding: 0px 3px 1px;">red</span> block
- `print_block`: Prints message in a colored block. You __must__ pass an ansi color as __last__ argument 

By default, a block starts and ends with a blank line.
When printing multiple blocks, pass `+` after the message to avoid duplicate linebreaks.


## Example

Using predefined colors:
```
source print.sh
print_error "Invalid target \"$TARGET\"" + 
print_info "Usage: watch <client|server|all>"
```


Using `print_block` and custom ansi color codes:

```
source print.sh
print_block "Invalid target \"$TARGET\"" + "\033[31m"
print_block "Usage: watch <client|server|all>" + "\e[46m"
```
## Known limitations

### $JOIN_PARAM

Even a single quoted string passed to the function is treated as an array of many strings separated by spaces.
Thus, the join mechanism that uses the magic rule "Last argument is a `+`" gets confused and fails when printing messages that themselves end with a `+`.
In that case, the plus is simply not displayed and instead the trailing linebreak is omitted on the block.

```
# both are treated the same way
print_info "foo +"
print_info "foo" +
```

Normally, this is not an issue at all, but you can still change script to something else if needed (Change the `JOIN_PARAM="+"` line at the top).

### Including the script

If `source print.sh` and `source ./print.sh` fail on your system, you can go the more reliable but also verbose way using the  directory of the running script:

```
# Assumes that print.sh is in the same directory as the running script
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
source $DIR/print.sh

print_success "It works!"
```

 