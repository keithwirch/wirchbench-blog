---
title: "Set Environment Variables in Bash"
date: 2023-08-08
tags: [
    "Development",
    "Linux",
]
draft: false
---

# Intro
Setting a environment varibales in bash can be done a few ways but these ways are tried and true always been reliable for me.  Bash doesn't really have "types" per say but it can with math with integers.  Variables are typically treated like strings.


## Setting Temporary Environment Variables
To setup a variable that is temporary, use the following syntax in a bash shell.

```
$ export <VARIABLE_NAME>=<VALUE>
```
Real world example would be:

```
# String
$ export MY_VAR1='abc'

# Integer
$ export MY_VAR2=4
```

## Setting Permament Environment Variables
If you need your variables to last past reboots and be available upon login, I reccomend using ~/.bashrc.  You'll put the same commands above inside your .bashrc file.

```
nano ~/.bashrc

<scroll to the end of the file>
# String
export MY_VAR1='abc'

# Variable
export MY_VAR2=4
```

On your next login or new terminal session, your environment variables will be there.  You can force your existing terminal session to have them by issuing this command:  `source ~/.bashrc` .

Hope this helps you!