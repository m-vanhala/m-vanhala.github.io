---
layout: default
---

# Command-line tools

## Introduction
The main objective of this course was to explore the command-line environment and some of the ways in which it can be used to streamline one's work.
I must admit that I was a little bit sceptical at first about the usefulness of the command line, but it really does have some benefits compared to working with GUIs. The most useful use case is definitely moving files, which is sometimes very tedious in Windows.


## Week 1
The first week revolved around the basics of the command line, starting with installation. We learned how to
+ move and copy files
+ install packages
+ quit applications (which is weirdly complicated)
+ use text editors

The most important commands are
```bash
mv, cat, less, cp, rm, mkdir, ls
```
which are used, respectively, to move, concatenate, display, copy, remove, create directory, and list contents. 

This stuff was quite basic and easy. The most annoying thing was moving files between Windows and Unix, for which I fortunately wrote a shell script on week 5.

## Week 2
The second week went deeper into the file system, and introduced us to processes. We also got a brief introduction into remote server use with the CSC servers. The following piece of code is used to connect to puhti servers:
```bash
ssh <CSC USERNAME>@puhti.csc.fi
```

I would have liked to learn more about the CSC servers and whether it is possible to use those resources as university staff. This is somewhat unrelated to this course, but I have some very computationally heavy projects which would really benefit from supercomputer access.

## Week 3
In the third week, we got to the meaty stuff and started doing corpus processing. First, we learned about character encoding and converting between them. Then we did some recapping on regular expressions and how to use them with the `grep` command. We also got a brief introduction on different formatted text files, such as tsv and csv files which one encounters really often. We also learned about piping commands with the `|` operator, and to utilize inputs and outputs with the `>` and `<` operators.

For instance, we can use the following command to find the number of words starting with "pre" and ending with "ed" from the Life of Bee book.
```bash
cat life_of_bee.txt | grep -E "\bpre(\w|-)*ed\b" | wc -l
```
The first part uses `cat` to read the `life_of_bee.txt` file. This is then passed on to the `grep` command with the `-E` flag for Extended. A regular expression is then set up to find words (characterized by word boundaries from the left and right), starting with "pre", ending with "ed", and allowing any number of word characters or dashes in between. The results of the `grep` are then passed on to the `wc` (word count), and since we want to know the number of lines containing such words, we can use the `-l` flag to only display the number of lines.

This week's stuff was very convenient and practical. When working with text data, being able to check text patterns quickly is very useful, even if one does the actual analysis with Python or some corpus processing software.

## Week 4
The fourth week was about more advanced corpus processing. We learned about the `sed` command, which can be used for finding and substituting things. In addition, we started processing texts at more detailed level by creating frequency lists of words, n-gram files, and files with one sentence per line. 
```bash
cat life_of_bee.trigram | egrep "^I am " | sort | uniq -c
```
The code above is used to find trigrams starting with "I am" and their number of occurrences. The first two parts we are already familiar with, the extended grep (`egrep`) uses the `^` operator to search from the beginning of line, and finally the output is sorted alphabetically and duplicates are counted with the `-c` flag and only the unique values are displayed.

This week's material was also very useful and practical. While Python offers even easier ways to do the same things, learning how to do them ourselves with such simple Unix commands was very insightful.

## Week 5
Week 5 introduced scripting, which is very useful for replicating stuff afterwards. Scripts also enable automating tedious tasks as well as quickly updating result files etc. when updating input files (which would otherwise be very annoying to do). We learned to write our own shell scripts and to compile them from Makefiles. Finally, we learned how to do basic loops, variables, variable substitution, printing and so on with Bash.

The following code snippet reads an input file containing a single word on each line, and returns a primitive comparative version of it by just adding "-er" in the end.
```bash
#!bin/bash
#comments about what the code does

file=$1
while read -r line;
do
  echo "${line}er"
done < "$file"
```
The first line, called shebang, must always be included. Comments about what the code does are also practically "required", since returning to some old code which does not have any comments whatsoever is very painful. The dollar sign is used to access variables, and `$1` refers to the first input argument. The `while` loop reads the file line by line, and for each line, prints the contents of the line (by accessing the current `line` variable) and appending "-er". When the loop has gone through all lines, it terminates the program. The `< "$file"` in the end indicates that the input argument is used as the input file for the `while` loop.

The Bash syntax is a little silly, and this is mainly because of its strictness about spaces. Luckily the editor emphasized extra spaces very clearly, so that finding redundant spaces is quite easy in practice. Using Bash scripts would probably be more useful for initiating Python code, like we ended up doing on week 6.


## Week 6
The sixth week included quite many different subjects. We first learned about installing and updating packages with package managers, then a little bit about Python, and finally how to use Makefiles to gather a bunch of functions.

Some of the most important commands from this week:

| Syntax      | Description |
| ----------- | ----------- |
| make      | builds other programs       |
| apt, brew   | packaging tools for installing programs        |
| sudo | authorize as root user |
| pip | install packages in Python for Python |

And we also learned to create virtual environments, which is very convenient so as to avoid conflicts between different versions in different projects and so on. A virtual environment can be created with
```python
py -m venv venv\
```
and then activated with
```python
venv\Scripts\activate
```
after which we can install libraries and other stuff normally:
```python
python -m pip install <package>
```
and finally, when we want to stop working with the virtual environment, we can deactivate it simply with
```python
deactivate
```

## Week 7
The seventh week introduced us to version control via Github. We created our own Github accounts and learned how to push and pull code, to use different branches and merge them when needed, and of course, how to reverse things when something goes awry (which is bound to happen every now and then).


A screenshot from the Git Tower's [Git cheatsheet](https://www.git-tower.com/blog/git-cheat-sheet):

<img src="assets/images/git_cheatsheet" alt="Screenshot" hspace="20" width="30%" align="center"/>

The basic workflow of creating a new branch, committing, and pushing is then
```bash
git checkout -b new_branch
git add -A
git commit -m "Informative message"
git push -u origin new_branch
```
where the flag `-u` checks that the local branch equals the remote branch. Additionally we can check the status of the current commit with
```bash
git status
```

Version control is extremely useful. While Overleaf already provides a great way to handle LaTeX code and collaborate, version control goes a little further since it allows pushing different versions of the text to an online repository. The premium version of Overleaf also has a similar feature through the History tab, but Github is more convenient. But most importantly, Github facilitates coding by making it so easy and effortless to code without fearing that some new feature will break the code, since it will always be possible to return to an earlier operative code. I will definitely start using Git for all my projects, whenever possible.

## Final project
For the final assignment, we learned to build our own webpages with Github Pages and powered by Jekyll. We learned to clone existing templates from Github repositories and modify them, as well as create our own github.io webpage. We can view the webpage locally very simply:
```bash
bundle exec jekyll serve
```
after which the site appears locally. We can push the local repository to Github, and the online site is then automatically updated.

Some useful markdown commands:

| Syntax | Output |
| ----------- | ----------- |
| `*italics*` | *italics* |
| `**bold**` | **bold** |
| `[link text](link)` | [Google](https://www.google.fi/?hl=fi) |
| `#Big header` | #Big header |
| `##Smaller header` | ##Smaller header |

Note that the headers do not display properly inside text because they are supposed to be headers, duh.

Webpage building is surprisingly easy. Many webpages look like they are very complicated (and some dynamic pages without a doubt are), but a simple static webpage is really easy to make with Jekyll, and is also very simple with standard HTML techniques. 

This final assignment was really two birds with one stone, since as a PhD student I've anyways been planning to create a webpage so it was very convenient to be able to create it as part of this course.
