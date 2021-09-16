Table of Contents
=================

   * [Listing Files](#listing-files)
      * [Listing the ten most recently modified files!](#listing-the-ten-most-recently-modified-files)
      * [Listing all files and folders!](#listing-all-files-and-folders)
      * [Listing files WITHOUT ls!](#listing-files-without-ls)
   * [Using cat (no not that cat.)](#using-cat-no-not-that-cat)
      * [Let's concatenate some files!](#lets-concatenate-some-files)
      * [Displaying the file contents](#displaying-the-file-contents)
      * [Writing to a file?](#writing-to-a-file)
   * [Appendix](#appendix)
      * [M-notation](#m-notation)

# Navigating folders

## Listing Files

This section documents commands and various flags used in listing files and folders. Commands include `ls` and `tree`. 

| Command          | Function                                                     |
| ---------------- | ------------------------------------------------------------ |
| `ls`             | Lists all files and directories, excluding dotfiles `.` and `..` |
| `ls -a`          | List all entries including **.** and **..**                  |
| `ls -A`          | List all entries excluding **.** and **..**                  |
| `ls -c`          | Sort files by change time                                    |
| `ls -d `         | List directory entries                                       |
| `ls -h`          | Show filesizes in KB, MB, GB etc.                            |
| `ls -H`          | Same as ls -h, using powers of 1,000 instead of 1,024        |
| `ls -l`          | Show contents in long-listing format                         |
| `ls -o`          | Long-listing format without groups.                          |
| `ls -r`          | Show contents in reverse order.                              |
| `ls -s`          | Print size of each file in blocks.                           |
| `ls -S`          | Sort by files size                                           |
| `ls --sort=WORD` | Sort contents by a word (size, version, status)              |
| `ls -t`          | Sort by modification time                                    |
| `ls -u`          | Sort by last access time                                     |
| `ls -v `         | Sort by version                                              |
| `ls -1`          | List one file per line                                       |
| `tree`           | Lists the contents of the current directory in a tree format |
| `tree -L n`      | Lists contents based on `n` levels.                          |
| `tree -d`        | Lists directories only.                                      |

[Table of Contents!](#Table-of-Contents)

## Listing the ten most recently modified files!

```bash
ls -lt | head	
```

This combines the `l` and `t` flags along with a `head` flag. `head` by default prints the first 10 lines/files. 

[Table of Contents!](#Table-of-Contents)

## Listing all files and folders!

```bash
ls -a 
```

Using the `a` flag would list *ALL* files in the directory, including `.` and `..` which represent the `current directory` and the `parent directory`! 

[Table of Contents!](#Table-of-Contents)

```bash
ls -A
```

`-a`'s big bro would omit the current and parent directories `.` and `..` while listing everything else!

[Table of Contents!](#Table-of-Contents)

## Listing files WITHOUT ls!

```bash
printf "%s\n" *
```

This displays all files and directories in the current directory.

[Table of Contents!](#Table-of-Contents)

```bash
printf "%s\n" */
```

This command which includes the forward slash `/` will list *only* the directories!

[Table of Contents!](#Table-of-Contents)

```bash
printf "%s\n" *.{gif,jpg,png}
```

And finally this! Displaying files according to select file extensions!

[Table of Contents!](#Table-of-Contents)

```bash
files=( * )
# iterate over them
for file in "${files[@]}"; do
echo "$file"
done
```

This script captures files in the current directory, storing them in a variable for processing.  

[Table of Contents!](#Table-of-Contents)

## Navigating files using less^1^

```bash
less file_name.txt
```

Subbing in `oneliners.txt` as an example would return something like this: 

```bash
# Remove text in between the two periods "." and before the file extension (.gif)
for f in ./*; do mv "$f" "${f%.*.gif}.gif" ; done

# Remove text in between the filename and the extension .svg
for file in *; do mv "${Jvcki Wai - [2016.11.02] - EXPOSURE - [FLAC - 44.1kHz - 16bit]}" "${Jvcki Wai - [2016.11.02] - //\EXPOSURE//\ - [FLAC - 44.1kHz - 16bit]/}"; done

oneliners.txt (END)
```

| **Command**           | **Action**                                                   |
| --------------------- | ------------------------------------------------------------ |
| `PageUp` or `b`       | Scroll back one page                                         |
| `PageDown` or `space` | Scroll forward one page                                      |
| `G`                   | Go to the end of the text file *Note that this and the below commands use capital Gs. |
| `1G`                  | Go to the beginning of the text file                         |
| `/your_characters`    | Search forward in the text file for an occurrence of the specified *characters* |
| `n`                   | Repeat the previous search                                   |
| `h`                   | Brings up the user manual                                    |
| `q`                   | Quit `less`                                                  |

## Determining filetypes using file^2^

```bash
file name_of_file.*
```

This command will determine the filetype for us, so try it out and sub the asterisk `*` for your file extension!

| **File Type**                  | **Description**                                              | **Viewable as text?**              |
| ------------------------------ | ------------------------------------------------------------ | ---------------------------------- |
| ASCII text                     | The name says it all!                                        | yes                                |
| Bourne-Again shell script text | A `bash` script                                              | yes                                |
| ELF 64-bit LSB executable      | An executable binary program                                 | no                                 |
| ELF 64-bit LSB shared object   | A shared library                                             | no                                 |
| GNU tar archive                | A tape archive file. A common way of storing groups of files. | no, use `tar tvf` to view listing. |
| gzip compressed data           | An archive compressed with `gzip`                            | no                                 |
| HTML document text             | A web page                                                   | yes                                |
| JPEG image data                | A compressed JPEG image                                      | no                                 |
| PostScript document text       | A PostScript file                                            | yes                                |
| Zip archive data               | An archive compressed with `zip`                             | no                                 |

These are just some examples of (somewhat?) common files found in a typical user `dir`.

**1, 2**: [Looking around](http://linuxcommand.org/lc3_lts0030.php) William E. Shotts, Jr. 

# Peek inside system directories^3^

| **Directory**        | **Description**                                              |
| -------------------- | ------------------------------------------------------------ |
| `/`                  | The root `dir` where the file system begins. Usually(*) this `dir` houses subdirectories and nothing else. |
| `/boot`              | Linux kernel and bootloader files are kept in this folder, where the kernel is a file called `vmlinuz`. |
| `/etc`               | `dir` containing configuration files (`.config` or `.conf`) for the system. All files in this `dir` should be `.txt` text files. <br /> `/etc/passwd` contains user info and is where user accounts are defined. <br /> `/etc/fstab` contains a table of devices that are mounted (such as external HDDs or sharepoints like `smb`) when the system boots up. This file contains the drives' information. <br /> `/etc/hosts` lists the hostnames and IP addresses that are intrinsically known to the system. <br /> `/etc/init.d` contains scripts that start system services upon startup. |
| `/bin`, `/usr/bin`   | They contain most programs for the system; `bin` keeping the essentials for normal operation, and `/usr/bin` safeguards the user applications. |
| `/sbin`, `/usr/sbin` | These two contain programs for `sysadmin` work, mostly for `superuser` use. |
| `/usr`               | It has a buncha stuff. <br /> `/usr/share/X11` supports files for the `X Window` system. <br /> `/usr/share/dict` has dictionaries for spellchecking! Check out `look` and `aspell` from the terminal if curious ;) <br /> `/usr/share/doc` has various doc files in various formats. <br />`/usr/share/man` keeps user manual files. |
| `/usr/local`         | This `dir` along with its siblings are used for software installs and misc files on the local system. Files which do not come bundled with the distro will go here. <br />It's good practice to sideload apps into the `/usr/local/bin` folder for easy management. |
| `/var`               | As the foldername suggests, files in here are `varied` as they tend to change while the system is running. <br />`/var/log` contains log files which are updated on-the-fly. Check these out once in a while to see what's up with your system's health. <br />`/var/spool` holds files that are queued for a process, such as mail messages and print jobs. |
| `/lib`               | Contains the Linux equivalent of DLLs (shared libraries)     |
| `/home`              | User files are kept here, so this *should* be the only place users can modify files! |
| `/root`              | Superuser's home directory.                                  |
| `/tmp`               | Temporary files created by programs are kept here.           |
| `/dev`               | A special directory which does not contain files (most of the time). Devices available to the system live here, and are treated like files. e.g: `/dev/sda` is the first hard drive, `/dev/sda1` is the second, and so on. |
| `/proc`              | Another special place, it doesn't exist (it's virtual so...). It contains a group of numbered entries that correspond to all the running processes. Some of these allow access to the current system configuration! Try looking at `/proc/cpuinfo`, which will detail what the kernel thinks of the CPU. |
| `/media`             | A normal directory used specially for mount points, which include external HDDs, and USB devices. <br />Upon system startup, a list of mounting instructions in the `/etc/fstab` file is passed to the system. This list describes the mount points for each device. TL;DR: `/media` is used by the auto-mount mechanism found in modern Linux distros. P.S Check out `mount`! |

**3**: [A Guided Tour](http://linuxcommand.org/lc3_lts0040.php) William E. Shotts, Jr.

# File manipulation^4^

## Wildcards

Wildcards are a set of special characters which allow selection of filenames based on a certain pattern. 

| **Wildcard**    | **Meaning**                                                  |
| --------------- | ------------------------------------------------------------ |
| `*`             | Matches any character(s)                                     |
| `?`             | Matches any single character                                 |
| `[characters]`  | Matches any character that is a member of the set `characters`. This set may be expressed as a *POSIX character class* such as:<br />`[:alnum:]` - Alphanumeric characters<br />`[:alpha:]` - Alphabetic characters<br />`[:digit:]` - Numerals<br />`[:upper:]` - Uppercase alphabetic characters<br />`[:lower:]` - Lowercase alphabetic characters |
| `[!characters]` | Matches any character that is not a member of the set `characters` |

Here are some examples of selection criteria made for filenames:

| **Pattern**                     | **Matches**                                                  |
| ------------------------------- | ------------------------------------------------------------ |
| `*`                             | Everything                                                   |
| `g*`                            | Everything beginning with "g"                                |
| `b*.txt`                        | Everything beginning with "b" with the `.txt` extension      |
| `Data???`                       | Everything that begins with "Data" followed by three more characters. The `?` denotes a character regardless of case, be it a symbol, number, or letter. |
| `[abc]*`                        | Everything that begins with "a" or "b" or "c" followed by any characters. |
| `[[:upper:]]*`                  | Everything that begins with an uppercase letter. Also an example of a character class! |
| `BACKUP.[[:digit:]][[:digit:]]` | Another example of character classes which matches everything beginning with "BACKUP" followed by two numbers. |
| `*[![:lower:]]`                 | Everything that doesn't end with a lowercase letter.         |

## cp

Stands for "copy". 

| **Command**         | **Results**                                                  |
| ------------------- | ------------------------------------------------------------ |
| `cp file1 file2`    | Copies everything in `file1` into `file2` if `file2` exists, otherwise `file2` will be created on the spot. `file2` will be overwritten using the stuff in `file1`. |
| `cp -i file1 file2` | The `-i` (interactive) flag will produce a confirmation prompt before `file2` is overwritten by the contents of `file1`. |
| `cp file1 dir1`     | Copy everything in `file1` into a new file `file1` inside the directory `dir1`. |
| `cp -R dir1 dir2`   | Copy the contents of `dir1` into `dir2`. If `dir2` doesn't exist, it is created. Otherwise, a new directory `dir1` will be created inside `dir2`. |

## mv

Moves or renames files and directories! 

| **Command**           | **Results**                                                  |
| --------------------- | ------------------------------------------------------------ |
| `mv file1 file2`      | Renames `file1` to `file2` if `file2` doesn't exist. If `file2` exists, its contents are overwritten by `file1`. |
| `mv -i file1 file2`   | `-i` produces a confirmation prompt if `file2` exists, asking the user if it's okay to overwrite `file2` with `file1`'s contents. |
| `mv file1 file2 dir1` | Moves `file1` and `file2` to a directory `dir1`. If `dir1` doesn't exist, `mv` will exit with an error. |
| `mv dir1 dir2`        | If `dir2` doesn't exist, `dir1` will be renamed `dir2`. If `dir2` exists, then `dir1` will be moved into `dir2`. |

## rm

Removes files and directories (be careful!)

| **Command**         | **Results**                                                  |
| ------------------- | ------------------------------------------------------------ |
| `rm file1 file2`    | Delete both files.                                           |
| `rm -i file1 file2` | A confirmation prompt is presented before deleting each file. |
| `rm -r dir1 dir2`   | Deletes everything inside the two directories with the `-r` flag performing a recursive search into both directories. |

When considering the use of `rm` for file/folder removal, do a quick `ls` equivalent of the command you wish to use with `rm`, and once you're sure of the selection, recall the previous command and sub `ls` for `rm`!

## mkdir

Creates directories, 'nuff said.

| **Command**            | **Results**                                                  |
| ---------------------- | ------------------------------------------------------------ |
| `cp *.txt text_files`  | Copies all text (.txt) files in the current folder over to a folder called "text_files". |
| `mv dir ../*.bak dir2` | Moves `dir1` and all `.bak` files in the current folder's parent directory to a folder named `dir2`. |
| `rm *~`                | Deletes all files in the current folder that end with the character "~". Some applications create backup files using this naming scheme. |

**4:** [Manipulating Files](http://linuxcommand.org/lc3_lts0050.php) William E. Shotts, Jr.

# Using commands^5^

## type

Displays the kind of command the shell will execute, an example would be like `type <command>`.

```bash
cal@haus:~$ type type > commands.txt
type is a shell builtin
cal@haus:~$type dir
dir is /usr/bin/dir
cal@haus:~$type typora
typora is aliased to `typora &'
```

Try typing `ls` and `dir --color=auto`; are these two commands the same? 

## which

Is useful for determining which version of a program is currently used (or referrerd to).

```bash
cal@haus:~$ which python
/usr/local/bin/python
cal@haus:~$ which python3
/usr/bin/python3
cal@haus:~$ which pip3
/usr/bin/pip3
```

## help

Is a user manual which is built-in for many apps. In newer Ubuntu versions, `help` is equivalent to `help -m`.

## --help

An alternative to when `help` doesn't work as intended.

## man

Opens the manual entry (if included) for any executable program.

**5**: [Working with Commands](http://linuxcommand.org/lc3_lts0060.php) William E. Shotts, Jr.

# Input & Output

## Standard output (stdout)

Almost all commands display results via the standard output `stdout`. To redirect this output to a file (for logging perhaps?), use the `>` operator:

```bash
tatsuei@eijihaus:~$ ls > files.txt
```

And a list of all files and subdirectories in the current directory will be listed in this text file.

If you would like to add new results to the file (such as when new folders/files are created), simply add another `>` operator!

```
tatsuei@eijihaus:~$ ls >> files.txt
```

## Standard input(stdin)

Like `stdout`, `stdin` can be redirected from a file or keystrokes.

```bash
tatsuei@eijihaus:~$ sort < files.txt
```

This would sort the contents (default is A-Z) of the file. Alternatively, if you would like the sorted contents in a different file for further processing, try:

```bash
tatsuei@eijihaus:~$ sort < files.txt > sorted_files.txt
```

## Pipelines

Used when connecting multiple commands together to form *pipelines*. The output of a command is fed directly into the input of another command, creating a chain!

```bash
tatsuei@eijihaus:~$ ls -l | less
```

In any case, the commands can be reversed as long as the pipe operator `|` is put in the right places!

| **Command**                     | **What it does**                                             |
| ------------------------------- | ------------------------------------------------------------ |
| `ls -lt | head`                 | Displays 10 recently modified files in the current folder.   |
| `du | sort -nr`                 | Displays a list of folders and foldersizes, sorted from largest to smallest. |
| `find . -type f -print | wc -l` | Displays the total number of files in the current folder and all of its subfolders. |

## Filters

These take the input and perform operations upon it, sending the results to the standard output. 

| **Program** | What it does                                                 |
| ----------- | ------------------------------------------------------------ |
| `sort`      | Sorts the input and outputs the sorted result. By default it sorts by A-Z, and 0-9. |
| `uniq`      | It removes duplicate lines of data from a sorted list of data. |
| `grep`      | Checks each line of data from the input and outputs every line containing a specified pattern of characters. |
| `fmt`       | Reads the input and outputs formatted text.                  |
| `pr`        | Takes an input and splits the data into pages with page breaks, headers, and footers. Useful for printing! |
| `head`      | Displays the first 10 lines of input.                        |
| `tail`      | Displays the last 10 lines of input.                         |
| `tr`        | Transforms characters, useful for when converting upper and lowercase characters or changing line termination characters to and from different types. |
| `sed`       | Stream editor which can perform more sophisticated text transformations. |
| `awk`       | A programming language designed for constructing filters.    |

## Examples



# Using cat (no not that cat.)

| Command  | Function                                                     |
| -------- | ------------------------------------------------------------ |
| `cat -n` | Prints line numbers                                          |
| `cat -v` | Show non-printing characters using `^` and M-notation except **LFD** and **TAB**. *Curious about this M-notation? [Click this](#M-notation)* |
| `cat -T` | Show **TAB** characters as ^I `(caret-i)`                    |
| `cat -E` | Show linefeed (LF) characters as *$*                         |
| `cat -e` | Show non-printing characters + show linefeed characters as *$* |
| `cat -b` | Number non-empty output lines, this flag overrides `-n`      |
| `cat -A` | Same as `cat -e` **and** show **TAB** characters as `caret-i` |
| `cat -s` | Suppress repeated empty output lines, `s` refers to `squeeze`. |

[Table of Contents!](#Table-of-Contents)

## Let's concatenate some files!

```bash
cat file1 file2 file3 > file_all	
```

This command would *compress* the contents in all three files and print the result as if it were a single file! 

```bash
cat file1 file2 file3 | grep foo				
```

This would take the contents of all three files and finds the keyword `foo` in the contents. This would save a lotta time :D 

[Table of Contents!](#Table-of-Contents)

## Displaying the file contents

```bash
cat file.txt
```

will read the file and display it in your terminal. If the file has some non-ASCII characters, you could display them using `cat -v`; this would be useful when control characters are invisible.

```bash
cat -v unicode.txt
```

If you're not into using `cat` and its various flags, try out [`less`](#Navigating-files-using-less) and `more`! These are interactive, allowing the user to use keys to navigate the file (much like `emacs` or `vim`).

```bash
less file.txt
```

Wanna pass file contents as an input for a command? Try redirection! `tr` is short for `translate`, a common term used in math (translation),

```bash
tr A-Z a-z <file.txt # same as cat file.txt | tr A-Z a-z
```

Gotta check the file from the end? Sure!

```bash
tac file.txt
```

To display the contents of a file in byte-by-byte form, a hex dump is the way to go! This is especially useful when you don't know the precise encoding. `od -cH` is typically used, though alternatives like `xxd` and `hexdump` are a bit more popular.

```bash
printf 'Hëllö wörld' | xxd
0000000: 48c3 ab6c 6cc3 b620 77c3 b672 6c64
H..ll.. w..rld
```

## Writing to a file?

```bash
cat >file
```

This would read any text input in the terminal and *overwrite* the existing data in the file regardless.

```bash
cat >>file
```

Don't wanna overwrite your data? Add another arrow! `>>` would tell the system you would like to *add* data to your file. P.S Use this key combo `Ctrl+D` when you're done typing!

```bash
cat <<END >file
Hello!
END
```

Here using the double arrows `<<` redirection symbol tells the system to append whatever text behind the `<<` to the end of the file on a new line!

# Appendix

## M-notation 

**Obtained from [Brian C.'s answer](https://stackoverflow.com/questions/44694331/what-is-the-m-notation-and-where-is-it-documented) in StackOverflow on July 6th, 2017 at 14:48**

**TL;DR:** The M-notation can be described (in layman terms) as the key-mappings of certain programs and terminals. M in this case refers to `Meta-Something` or the `Meta` key defined by the program and `Something` refers to the key combo pressed after the `Meta` key. 

>I was wondering this too. I checked the source but it seemed easier to create a input file to get the mapping.

> I created a test input file with a Perl scrip `for( my $i=0 ; $i < 256; $i++ ) {  print ( sprintf( "%c is %d %x\n", $i, $i ,$i ) ); }` and then ran it through cat -v

> Also if you see M-oM-;M-? at the start of a file it is the UTF-8 byte order mark.

> **Scroll down through these to get to the M- values:**

```
^@ is 0 0
^A is 1 1
^B is 2 2
^C is 3 3
^D is 4 4
^E is 5 5
^F is 6 6
^G is 7 7
^H is 8 8
(9 is tab)
(10 is NL)
^K is 11 b
^L is 12 c
^M is 13 d
^N is 14 e
^O is 15 f
^P is 16 10
^Q is 17 11
^R is 18 12
^S is 19 13
^T is 20 14
^U is 21 15
^V is 22 16
^W is 23 17
^X is 24 18
^Y is 25 19
^Z is 26 1a
^[ is 27 1b
^\ is 28 1c
^] is 29 1d
^^ is 30 1e
^_ is 31 1f
...printing chars removed...
^? is 127 7f
M-^@ is 128 80
M-^A is 129 81
M-^B is 130 82
M-^C is 131 83
M-^D is 132 84
M-^E is 133 85
M-^F is 134 86
M-^G is 135 87
M-^H is 136 88
M-^I is 137 89
M-^J is 138 8a
M-^K is 139 8b
M-^L is 140 8c
M-^M is 141 8d
M-^N is 142 8e
M-^O is 143 8f
M-^P is 144 90
M-^Q is 145 91
M-^R is 146 92
M-^S is 147 93
M-^T is 148 94
M-^U is 149 95
M-^V is 150 96
M-^W is 151 97
M-^X is 152 98
M-^Y is 153 99
M-^Z is 154 9a
M-^[ is 155 9b
M-^\ is 156 9c
M-^] is 157 9d
M-^^ is 158 9e
M-^_ is 159 9f
M-  is 160 a0
M-! is 161 a1
M-" is 162 a2
M-# is 163 a3
M-$ is 164 a4
M-% is 165 a5
M-& is 166 a6
M-' is 167 a7
M-( is 168 a8
M-) is 169 a9
M-* is 170 aa
M-+ is 171 ab
M-, is 172 ac
M-- is 173 ad
M-. is 174 ae
M-/ is 175 af
M-0 is 176 b0
M-1 is 177 b1
M-2 is 178 b2
M-3 is 179 b3
M-4 is 180 b4
M-5 is 181 b5
M-6 is 182 b6
M-7 is 183 b7
M-8 is 184 b8
M-9 is 185 b9
M-: is 186 ba
M-; is 187 bb
M-< is 188 bc
M-= is 189 bd
M-> is 190 be
M-? is 191 bf
M-@ is 192 c0
M-A is 193 c1
M-B is 194 c2
M-C is 195 c3
M-D is 196 c4
M-E is 197 c5
M-F is 198 c6
M-G is 199 c7
M-H is 200 c8
M-I is 201 c9
M-J is 202 ca
M-K is 203 cb
M-L is 204 cc
M-M is 205 cd
M-N is 206 ce
M-O is 207 cf
M-P is 208 d0
M-Q is 209 d1
M-R is 210 d2
M-S is 211 d3
M-T is 212 d4
M-U is 213 d5
M-V is 214 d6
M-W is 215 d7
M-X is 216 d8
M-Y is 217 d9
M-Z is 218 da
M-[ is 219 db
M-\ is 220 dc
M-] is 221 dd
M-^ is 222 de
M-_ is 223 df
M-` is 224 e0
M-a is 225 e1
M-b is 226 e2
M-c is 227 e3
M-d is 228 e4
M-e is 229 e5
M-f is 230 e6
M-g is 231 e7
M-h is 232 e8
M-i is 233 e9
M-j is 234 ea
M-k is 235 eb
M-l is 236 ec
M-m is 237 ed
M-n is 238 ee
M-o is 239 ef
M-p is 240 f0
M-q is 241 f1
M-r is 242 f2
M-s is 243 f3
M-t is 244 f4
M-u is 245 f5
M-v is 246 f6
M-w is 247 f7
M-x is 248 f8
M-y is 249 f9
M-z is 250 fa
M-{ is 251 fb
M-| is 252 fc
M-} is 253 fd
M-~ is 254 fe
M-^? is 255 ff
```



















