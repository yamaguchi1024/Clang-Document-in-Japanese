### ClangFormat
ClangFormatはLibFormatの最上位に位置するツール群です。独立したツールとしても、またはエディタと統合することによっても開発者のワークフローをサポートします。

#### 独立したツールとして
* clang-formatは clang/tools/clang-formatにあり、C/C++/Obj-Cのコードをフォーマットするのに使えます。

```
$ clang-format -help
OVERVIEW: A tool to format C/C++/Java/JavaScript/Objective-C/Protobuf code.

If no arguments are specified, it formats the code from standard input
and writes the result to the standard output.
If <file>s are given, it reformats the files. If -i is specified
together with <file>s, the files are edited in-place. Otherwise, the
result is written to the standard output.

USAGE: clang-format [options] [<file> ...]

OPTIONS:

Clang-format options:

  -assume-filename=<string> - When reading from stdin, clang-format assumes this
                              filename to look for a style config file (with
                              -style=file) and to determine the language.
  -cursor=<uint>            - The position of the cursor when invoking
                              clang-format from an editor integration
  -dump-config              - Dump configuration options to stdout and exit.
                              Can be used with -style option.
  -fallback-style=<string>  - The name of the predefined style used as a
                              fallback in case clang-format is invoked with
                              -style=file, but can not find the .clang-format
                              file to use.
                              Use -fallback-style=none to skip formatting.
  -i                        - Inplace edit <file>s, if specified.
  -length=<uint>            - Format a range of this length (in bytes).
                              Multiple ranges can be formatted by specifying
                              several -offset and -length pairs.
                              When only a single -offset is specified without
                              -length, clang-format will format up to the end
                              of the file.
                              Can only be used with one input file.
  -lines=<string>           - <start line>:<end line> - format a range of
                              lines (both 1-based).
                              Multiple ranges can be formatted by specifying
                              several -lines arguments.
                              Can't be used with -offset and -length.
                              Can only be used with one input file.
  -offset=<uint>            - Format a range starting at this byte offset.
                              Multiple ranges can be formatted by specifying
                              several -offset and -length pairs.
                              Can only be used with one input file.
  -output-replacements-xml  - Output replacements as XML.
  -sort-includes            - Sort touched include lines
  -style=<string>           - Coding style, currently supports:
                                LLVM, Google, Chromium, Mozilla, WebKit.
                              Use -style=file to load style configuration from
                              .clang-format file located in one of the parent
                              directories of the source file (or current
                              directory for stdin).
                              Use -style="{key: value, ...}" to set specific
                              parameters, e.g.:
                                -style="{BasedOnStyle: llvm, IndentWidth: 8}"

Generic Options:

  -help                     - Display available options (-help-hidden for more)
  -help-list                - Display list of available options (-help-list-hidden for more)
  -version                  - Display the version of this program
```
