## 2020-07-26 Version 0.2.8

### New features

* Add `setup-dev-env` target in Makefile
* Add `get(MatchSuccess, Int, dflt)`
* Add `Null()` constructor
* Add experimental `AllOf()`
* Add experimental `Bool(AllOf)`
* Add experimental `Bool(AnyOf)`
* Add `linux` and `windows` parameter to `ec2din.ngs`
* More concise tests with `THROWS` pseudo-operator
* Add `NamedInstances` (aka enums)
* Add experimental `Arg(Program)`

### Fixes and improvements

* Function with only guard statement no more crashes NGS
* Add missing `Bool()` call in `for(..., HERE, ...)`
* Functions that take predicates - default the predicates to `identity()` and improve documentation
* `filter(HashLike, predicate)` - the `predicate` doesn't have to be `Fun` anymore
* `filter(hl:HashLike, predicate)` now returns value of the same type as `hl`
* `escape_bash()` - fixed and it's not experimental anymore
* `section` inside `ns` now working properly
* Top-level/commands syntax now treats as expression syntax any expression that starts with identifier followed by "." or "::".
* Support `block.return()` to return `null`
* `each(MultiMethod, Fun)` now returns the first parameter
* Improve readability of `partition(Eachable1, predicate)`
* `Int(Str)` now throws if the string contains garbage in the end
* `IPAddr()` and `IPNet()` now validate input
* `+(Hash, Hash)` now handles subtypes properly (does not return a `Hash` but the subtype)
* `ns { ... }` syntax now allows empty body - `ns { }`.
* Native `attrs()` methods - not crash on invalid argument
* `attrs()` now defaults to empty `Hash`, not to `null`.
* Improve `Str(Type)`
* `echo(INVALID_FILE_DESCRIPTOR, ...)` now throws `WriteFail`
* `write(INVALID_FILE_DESCRIPTOR, ...)` now throws `WriteFail`

### Breaking changes

* Experimental `OneOf` renamed to `AnyOf` for naming consistency
* Remove unused literal syntax for table
* Syntax: `F` function definition must be followed by a space when the function is named
* Remove deprecated `table(Arr)`
* Remove deprecated `filter(Eachable1, Str, Any)`
* Remove experimental `min(Eachable1, Fun)` and `max(Eachable1, Fun)`

### Deprecated

* Deprecated `ensure_array(x)` in favor of the newer `ensure(x, Arr)`

## 2019-11-10 Version 0.2.7

### New features

* Concurrency
	* Add `c_pthread_cond_*` methods
	* Add `Cond` type that exposes `pthread_cond_*` functionality
	* Add `BlockingList` linked list type with bocking `shift()` and `pop()` methods
	* Add `Executor` type
	* Add `ThreadedExecutor` type for simplified processing in threads
	* Add `pmap(Eachable1, Int, Fun)` - parallel map using limited numbers of threads
	* Add `pfilter(Eachable1, predicate)` - parallel filter
	* Add `pfilter(Eachable1, Int, predicate)` - parallel filter using limited numbers of threads
* Add `block IDENTIFIER BODY` syntax
* Add `section NAME BODY` syntax
* Add experimental `TmpFile` - temporary file that is deleted automatically on exit
* Add experimental `Program` (allows `assert(Program("dd"))`)
* Add experimental `sortv()`
* Add experimental `OneOf` type (allows `"abc" ~ OneOf(Int, Str)`)
* Add `List` linked list type
* Add `JsonData` method which converts to JSON-compatible data structures; used by `encode_json` now.
* Add `has_no(container, element)`
* Add `echo(ProcessesPipeline, Str)`
* Automate updating `ngs.vim` syntax file
* Add `lines()` - returns all standard input as array of lines
* Add `words(Str)`
* Add `Results` type, array-like with `Result` items
* Add `ResultsException` type (with `.results` being `Results` type)
* Add `List` type
* Add `inspect(Eachable1)`
* Add `len(Box)`
* Add `len(NumRange)` for `Int` ranges
* Add `RetryBodyMissing` exception
* Add `Namespace` type. It is now returned by `ns` block.
* Add `Pred(NumRange)`.
* Default code to run when not given on command line is `F default_argv_code() { fetch() }()` .
* Add Vagrantfile
* Add `~(x, t:Type)`
* Add `has_index(Eachable1, Int)`
* Add `len(Int)`
* Add experimental `Pred(OneOf)`
* Add experimental `TODO(Str)`
* `ec2din.ngs` now takes expirimental `-c COMMAND`

### Fixes and improvements

* Rare crashes due to GC fixed
* Multi-stage builds for the docker image (thanks, @organom)
* `Hash(Arr, Str)` is now `Hash(Eachable1, Str)` (to support `ArrLike`)
* `Hash(Arr, Fun)` is now `Hash(Eachable1, Fun)` (to support `ArrLike`)
* Add missing brew dependencies to `install-mac.sh` (thanks for reporting, @zzamboni)
* Add `executable` to `inspect(Process)` and to exception reporting.
* `basename()` is now implemented in `stdlib.ngs` as opposed to previously using `BASENAME(1)`.
* `finished_ok()` now checks for known programs by their basename and not absolute paths (@zzamboni reported failing tests; this changes should fix it)
* `echo(Int, Lines)` is now `echo(Any, Lines)`
* Fix `wait(Process)` to behave correctly when `exit_code` is not set but `exit_signal` is set.
* Fix `$(blah >${true})`
* `print_exception()` - add optional parameter `echo`
* `exception_specific_message()` - refactored for reusability
* Improve error messages for syntax errors
* `push(e:Enum, name:Str)` now returns `e`
* `stat()` methods now return timestamps too (access/modify/change)
* `pmap()` now throws `ResultsException` if any of the threads fail 
* `Stats()` now works with `Eachable1` allowing counting characters in a string for examples
* Fix Pred(SubSeq) and Pred(RegExp) to prevent endless recursion
* Fix `$(... >${false})` syntax
* Fix `c_strptime()`
* Improve `rand_uniq` implementation
* Improve `MatchSuccess` - include pattern
* Improve `MatchFailure` - include data and pattern
* Move `get(Arr, Int, Any)` from C to stdlib and support negative indexes.
* Merged two `first()` methods into one
* Fix `Hash` comparison for `null`
* `+` character is now valid "word" symbol - `$(dig +short yahoo.com)` is now valid syntax

### Breaking changes

* Remove `Return` type and associated machinery
* Remove deprecated `filter(Eachable1)`
* Remove deprecated `to_exit_code()`

### Work in progress

* Brew packaging for MacOS

## 2019-04-13 Version 0.2.6

### Breaking changes

* `$()` now returns `ProcessesPipeline` (a new type). You should check all the places in your code that use `CommandsPipeline`. Code that handles result of `$(...)` should be changed to expect `ProcessesPipeline`, not `CommandsPipeline`.
* `$(nofail my_command)` was removed. Use `$(ok: my_command)`. Deprecated `$(nofail: my_command)` still works.

### New features

* Add `chdir()`.
* Add `set(obj, **kwargs)`. Sets given fields.
* Add `copy(nti:NormalTypeInstance)`. Make shallow copy of an object.
* `Lines` type. Array of strings to be processed as a unit for output purposes (`echo`, `warn`, `error`).
* `inspect()` now returns `Lines`.
* Add `-pil` command line switch for "Print Inspect()ed Lines"
* Add `collector` for `ArrLike`. Enables `collector/ArrLike() ...`.
* Add `mapo(Str, Fun)` (**map** to **o**riginal data type) - map a string to a string, character by character.
* Add `assert_bool(Any, Str)` to `tests.ngs`
* Add `+(a:Eachable1, b:Eachable1)` which is only defined for same types of `a` and `b`.
* Add `-(Str, Ifx)`
* Add experimental `is_subtype(t:Type, maybe_supertype:Type)`.
* Add experimental `ensure(x, t:Type)` for `Eachable1` subtypes. Returns either `x` if it's already of type `t` or new object of type `t` with single item `x`.
* Add `ProcessesPipeline` type
* Add `inspect(pp:ProcessesPipeline)`
* Add `ProcessRedir` type; `Redir` type was renamed to `CommandRedir`. `CommandsPipeline` has `CommandRedir` while `ProcessesPipeline` has `ProcessRedir`.
* Add `Threads(ArrLike)` type
* Add `join(ArrLike, Str)`
* Add `.(ArrLike, Str)`
* Add `collector(Set, Fun)`, allowing `collector/Set() ...`
* Add `MethodParams` type
* Add `MethodParam` type (and subtypes `RequiredMethodParam`, `OptionalMethodParam`, `SplatMethodParam`, `ArrSplatMethodParam`, `HashSplatMethodParam`)
* Add `NGS_EXIT_BACKTRACE` environment variable. If true, prints backtrace on `exit()`.
* Add `NGS_WARN_BACKTRACE` environment variable. If true, prints backtrace on `warn()`.
* Add `NGS_ERROR_BACKTRACE` environment variable. If true, prints backtrace on `error()`.
* Add `$(ok_sig: ...)` option. If external program terminates by specified signal, NGS will not throw exception.
* Add `$(cd: ...)` option. Runs the external program in the specified directory.
* Add `$(log: ...)` option. Logs the program to be executed first and then executes the program.

### Fixes and improvements

* Fixed `catch` bug that was resulting `dump Uncaught exception` messages when `catch(e:MyType)` was handling `e` of non-`MyType`.
* `indexes(arr:Arr, predicate)` upgraded to handle eachable: `indexes(e:Eachable1, predicate)`
* `indexes(arr:Arr, r:PredRange)` - now takes optional `dflt` parameter: `indexes(arr:Arr, r:PredRange, dflt=...)`
* `Box()` now returns an `EmptyBox` (thanks @organom !)
* `Pred` improved so that `.filter({"a": 1})` will not throw exception if there is no `a` field.
* Better `c_dlopen()` error message
* `T.user = x` for native types now returns `x` and not `T`, consistent with other assignments
* `Return` is now a subtype of `Exception`. This fixes failing `Failure(Return)`, which only works on `Exception`.
* Remove `any()` and `none()` for `Box` - they work anyway because `Box` is `Eachable1`. `any()` and `none()` are defined for `Eachable1`.
* Faster `==(Hash, Hash)`
* Faster `init(Hash)`
* Fix `C_WEXITSTATUS` - return null when `WIFEXITED` returns false.
* Fix `C_WTERMSIG` - return null when `WIFSIGNALED` returns false.
* Fix `decode_hex()` - now handles inputs of even length, not specifically 2. This makes `decode_hex()` exact reverse of `encode_hex()`.
* Work around strange error (Issue #180) after `execve()` was failing on MacOS. Now executing minimal amount of code after failing `execve()`.
* `+(s:Str, a:Eachable1)` and +`(a:Eachable1, s:Str)` now return result of exact the same type as `a`, not `Arr`.
* Improve `print_exception()`
* Improved exception message for environment variable access, when the environment variable is not set.
* Remove MacOS-specific "stupid" malloc. GC library appears to work OK on MacOS now.
* Got rid of uthash dependency
* Finished moving `todo.txt` to GitHub issues
* Improve `ArrLike` implementation
* `bin/elb-describe-lbs.ngs` - Instances are now sorted by LaunchTime
* Improve `finished_ok` implementation: easier handling of "known programs", refactor to support `ok_sig` option.
* Warning on usage of `` `line: ` `` option on non-last command.
* `CommandsPipeline` related changes which have low probability of breaking anything.
	* `wait(cp:CommandsPipeline)` is now `wait(pp:ProcessesPipeline)` and returns `pp`.
	* `Str(cp:CommandsPipeline)` is now `Str(pp:ProcessesPipeline)`.
	* `kill(cp:CommandsPipeline, sig:Int=SIGNALS.TERM)` is now `kill(pp:ProcessesPipeline, sig:Int=SIGNALS.TERM)`
	* `lines(cp:CommandsPipeline)` is now `lines(pp:ProcessesPipeline)`
	* `lines(cp:CommandsPipeline, cb:Fun)` is now `lines(pp:ProcessesPipeline, cb:Fun)`
	* `assert_...(cp:CommandsPipeline, ...)` are now `assert_...(pp:ProcessesPipeline, ...)`
* `Argv()` - support for arguments that need repeated argument name

### Deprecated

* Deprecated `Pred(BasicTypeInstance)`

### Removed

* Removed deprecated `nuke_null()`

### Work in progress

* Terminal
* Improving error messages shown when exceptions occur.
* Add experimental `escape_bash(Str)` - quote argument for bash

## 2018-09-20 Version 0.2.5

### New features

* Running NGS without arguments no more produces an error but a friendly usage message
* Docker image for each commit; built by Travis
* Threads-related
	* Add `Thread.local` - thread local storage (a Hash)
		* `Thread.local.thread` - current thread object
	* Threads are now named:
		* Add `init(t:Thread, name:Str, f:Fun)`
		* Add `init(t:Thread, name:Str, f:Fun, arg)`
* `aws` command output parsing - `.Reservation` renamed to `._Reservation`
* Add `eachk()` and `eachv()`
* `Str(s:Str, target_width:Int)` extended to `Str(s:Str, target_width:Int, ch=' ')`, allowing padding with given character
* Exiting
	* Add `ExitException` - parent of all exceptions with `exit_code` field
		* `FatalError` is now a child of `ExitException`
		* Add `NormalExit` (child of `ExitException`)
	* Add `exit(exit_code:Int=1)`
	* Add `exit(s:Str, exit_code:Int=1)`
* Add `dollar \$ escaping syntax`
* Add `map_idx_val(e:Eachable1, mapper:Fun)`
* Add `myip()` - returns your IP as observable from the Internet
* Add `each()` for `Success` and `Failure`
* Add experimental `each_chunk(e:Eachable1, n:Int, cb:Fun)`
* Add experimental `decode(s:Str, t:Type)`
* Add experimental `nd` (Ngs Data) command line tool

### Fixes and improvements

* Fix incorrect parsing of immediate hashes: `%{k1 v1 k2 v2 ...}`
* Improve `Pred(r:RegExp)`
* Fix `join(Arr)` for array of `Thread`s
* `Bool(Path)` now checks that underlying file system object is of the correct type
* `dir()` now skips `.` and `..` entries
* `dir()` now has "raw" parameter
* `decode()` now parses "locate" output
* `nop()` now takes any arguments
* `retry()` - removed "catch_exceptions" parameter
* `assert_path_exists(`) - improve implementation
* `E_...` constants from C now include all constants available during compilation
* `init(Failure, ...)` - only accept Exception as second argument
* `decode_json()` - improve exception

### Work in progress

* `AWS2` -- second version of AWS (among other: `.Region` renamed to `._Region`)
* `bin/na` ("NGS AWS") -- CLI for AWS wrapper around declarative primitives library
* `bin/nd` ("NGS Data") -- Data manipulation utility
* documentation
* Moving `todo.txt` to [GitHub issues](https://github.com/ngs-lang/ngs/issues)

## 2018-04-21 Version 0.2.4

### New features

* Add `Time`, built on exposed c primitives - gettimeofday, strptime, mktime
* Add `Set` type
* Add `.user` field for arbitrary data on types
* Add `bin/hn.ngs` -- "tail -f" like script for Hacker News
* status() will override previous one on tty using escape codes
if no other output was sent between the calls
* status() - use iTerm2 badges

### Fixes and improvements

* `join(Thread)` - throw exception if the other thread had exception
* Improve AWS declarative primitives library
* Implement file descriptors redirection: `$(my_prog 2>${1})`
* Better `Hash` iteration in stdlib due to additional exposed `ll_hash_*` primitives and `LLHashEntry` type.
* Fix Linux installation instructions
* Update Mac installation instructions
* Automatically set reference to namespace in methods defined in `ns { ... }`
* Improve memory allocation - use "atomic" memory in few additional places
* Rename "Method" to "Multimethod" and "Method imlementation" to "Method" all over
* Rename "attribute" to "field" all over
* Deprecate KV
* Rename `to_exit_code` to `ExitCode` for consistency (deprecate `to_exit_code`)

### Work in progress

* AWS2 -- second version of AWS
* `bin/na` ("NGS AWS") -- CLI for AWS wrapper around declarative primitives library
* `Table2` -- the more advanced tabular data holder
* documentation
* Experimental syntax for building documents
* Add experimental `MapIter`, `FilterIter`, `FunIter`

## 2017-07-09 Version 0.2.3

* Fix missing `version.h` and `ChageLog` update in 0.2.2

## 2017-07-09 Version 0.2.2

* Fix `assert_*` functions after pipes support in 0.2.0

## 2017-06-07 Version 0.2.1

* Add `lines(cp:CommandsPipeline)` to fix broken `$(command).lines()`
after pipes support in 0.2.0
* Add `to_exit_code(cp:CommandsPipeline)` to fix broken exit codes
after pipes support in 0.2.0
* Fix tests broken with `RegExp` syntax change in 0.2.0
* Fix `Str(t:Time, format:Str=TIME_FORMAT, gmt:Bool=false)`
that was not using `format`
* Add experimental `Pred(h:Hash)`

## 2017-06-02 Version 0.2.0

### Breaking changes

* BREAKING: Swap write() arguments from write(what, file) to write(file, what)
Many methods with old arguments order will still work but will give a
deprecation warning.

###  New Features
* Pipes support
* Add optional "count" (bytes) parameter to read(fd:Int)
* Add preliminary `Str(t:c_pthread_t)`
* Add missing `id(rrset:RecordSetRes)`
* Add `abs(i:Int)`
* Add `decode_uri_component(s:Str)`

### Fixes and improvements

* Slightly improved documentation
* Improve stability of `Str(r:Res)`
* Fix error handling in `[](arr:Arr, r:NumRange)`
* Add optional "max_parts" parameter to split(s:Str, delim:Str)
* A bit more readable inspect() output
* Fix backslashes syntax in `/regular expressions/`

## 2017-05-13 Version 0.1.0

* Incorporate Mac OS X support contributed by Zeev Glozman
