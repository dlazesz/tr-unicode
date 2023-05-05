# tr-unicode
The Unix tr command reimplemented in Perl, augmented with (minimal) Unicode support

[GNU tr has no support for Unicode.](https://lists.gnu.org/archive/html/bug-coreutils/2009-02/msg00028.html) [It works on bytes and its 8-bit clean.](https://git.savannah.gnu.org/cgit/coreutils.git/commit/src/tr.c?id=8d3dce9861c15f06a014c91fa29c15143fd27127)

Therefore on UTF-8 text *strange* errors occur *randomly* as a *feature*.
For example:

    $ echo "°" | tr "Ű" "ű"
    ±
This implementation is trying to provide a fix for this issue with minimal Unicode support.

**Bug reports and patch requests are welcome on the project site!**

Note: [The tr implementation of *uutils coreutils* written in Rust](https://github.com/uutils/coreutils) [has Unicode support](https://github.com/uutils/coreutils/blob/6988eb7ec64eff10d2ebf001c7fef845c04336d5/tests/by-util/test_tr.rs)

## Invocation

One can make an alias to tr or place in ~/bin to have the basic functionality of tr.

### Excerpt form man tr with some addition:

Usage: tr [OPTION]... SET1 [SET2]

(OPTION must (if there are any) present in the first argument, only one at a
    time (TODO: bundling)!)

This is a tr-like utility in Perl with minimal Unicode support.
(Aid for tr's 'feature': echo "°" | tr "Ű" "ű" -> ±)

Translate, squeeze, and/or delete characters from standard input,
writing to standard output.

    -c, -C, --complement    use the complement of SET1
    -d, --delete            delete characters in SET1, do not translate
    -s, --squeeze-repeats   replace each input sequence of a repeated character
                              that is listed in SET1 with a single occurrence
                              of that character
    -t, --truncate-set1     (NOT IMPLEMENTED) first truncate SET1 to length of
                              SET2
        --help     display this help and exit (This text)
        --version  output version information and exit (NOT IMPLEMENTED)

SETs are specified as strings of characters.  Most represent themselves.
Interpreted sequences are:

    \NNN            character with octal value NNN (1 to 3 octal digits)
    \\              backslash
    \a              audible BEL
    \b              backspace
    \f              form feed
    \n              new line
    \r              return
    \t              horizontal tab
    \v              vertical tab
    CHAR1-CHAR2     all characters from CHAR1 to CHAR2 in ascending order
                    (as in ASCII, TODO: MAKE LOCALE DEPENDENT)
  
    NOT IMPLEMENTED, TODO:
    [CHAR*]         in SET2, copies of CHAR until length of SET1
    [CHAR*REPEAT]   REPEAT copies of CHAR, REPEAT octal if starting with 0
    [:alnum:]       all letters and digits
    [:alpha:]       all letters
    [:blank:]       all horizontal whitespace
    [:cntrl:]       all control characters
    [:digit:]       all digits
    [:graph:]       all printable characters, not including space
    [:lower:]       all lower case letters
    [:print:]       all printable characters, including space
    [:punct:]       all punctuation characters
    [:space:]       all horizontal or vertical whitespace
    [:upper:]       all upper case letters
    [:xdigit:]      all hexadecimal digits
    [=CHAR=]        all characters which are equivalent to CHAR

Bug reports and patch requests are welcome on the project site!

Excerpt from man tr:

Translation occurs if -d is not given and both SET1 and SET2 appear.
-t may be used only when translating.  SET2 is extended to length of
SET1 by repeating its last character as necessary.  Excess characters
of SET2 are ignored.  Only [:lower:] and [:upper:] are guaranteed to
expand in ascending order; used in SET2 while translating, they may
only be used in pairs to specify case conversion.  -s uses SET1 if not
translating nor deleting; else squeezing uses SET2 and occurs after
translation or deletion.

GNU coreutils home page: <http://www.gnu.org/software/coreutils/>
General help using GNU software: <http://www.gnu.org/gethelp/>
For complete documentation, run: info coreutils 'tr invocation'
