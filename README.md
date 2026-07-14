# get_next_line

`get_next_line` is a reusable line reader that returns one line at a time from a file descriptor. It is designed as a compact C module for the 42 curriculum and uses a static stash to preserve unread data between calls.

## Overview

The function reads from a file descriptor until it finds a newline or reaches EOF, then returns the next line as a newly allocated string.

This implementation includes:

- persistent internal buffer management
- newline detection across multiple reads
- clean handling of the last line without a trailing newline
- helper functions for string duplication and joining

## Public API

```c
char *get_next_line(int fd);
```

The header also provides the helper declarations used internally by the implementation.

## Behavior

- Returns a heap-allocated line including the trailing `\n` when present
- Returns the final line even if it does not end with `\n`
- Returns `NULL` on EOF or on read/allocation failure
- Keeps unread data for the next call

## Buffer size

`BUFFER_SIZE` defaults to `42` if it is not defined at compile time.

Example:

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c
```

## Project structure

```text
gnl/
├── get_next_line.c
├── get_next_line.h
└── get_next_line_utils.c
```

## Implementation notes

The function works in three steps:

1. Read chunks into a buffer until a newline appears or the read ends.
2. Extract the next line from the stash.
3. Remove the consumed part and keep the remainder for the next call.

The internal stash is static, so one descriptor can keep its own progress across repeated calls.

## Integration

This module is usually copied into another project or compiled together with it. It is especially useful for projects that need line-by-line file parsing.
