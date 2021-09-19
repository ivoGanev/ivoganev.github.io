---
layout: post
title:  "Useful Windows Commands"
date:   2021-09-18 09:25:08 +0000
categories: command line
published: true
---

Here are some useful commands for cmd on Windows.

## findstr
Equivalent to Unix `grep`. Searches for patterns of text in files.

| Example Command | Description |
| --- | --- |
| `findstr hello there x.y` |  Searches for <i>hello</i> or <i>there</i> in file x.y.  |
| `findstr /c:"hello there" x.y` |  Searches for <i>hello there</i> in file x.y  |
| `findstr /s /i Windows *.*` |  Search every file in the current directory and all subdirectories that contained the word Windows, regardless of the letter case. |

## type
Equivalent to Unix `cat`. Lists the content of a file.

| Example Command | Description |
| --- | --- |
| `type file.txt` |  Displays the content of <i>file.txt</i> |

## dir
Displays a list of a directory's files and subdirectories.

| Example Command | Description |
| --- | --- |
| <code>dir /s &#124; findstr [pattern] </code> | Searches the current and all subdirectories for a file with a name matching the regex pattern. |
