# What is `$PATH`?

Mira Sohn

2022-10-16

This resource is aimed at sharing how I learned about `$PATH` with my bioinformatics folks expecially who have limited computational knowledge like me. Technically, the `$PATH` is an environment variable used in Unix system. Windows and DOS use `%PATH%` instead of `$PATH`. The `$PATH` literally stores colon (`:`)-separated paths. Check your `$PATH` variable with the command `echo $PATH` in your terminal. 

```bash
$ echo $PATH
/home/mira/.cargo/bin:/home/mira/.cargo/bin:/home/mira/miniconda3/bin:/home/mira/miniconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/mira/opt/bin:/home/mira/miniconda3/bin:/home/mira/miniconda3/bin:/home/mira/.fzf/bin
```

My `$PATH` has 16 colon-separated paths.

What do they mean? Let's go back to the command `echo` which you ran seconds ago. How did it work? Your computer would answer like this "Give a try running `which echo` in your terminal!" 


```bash
$ which echo
/usr/bin/echo
```

If you knew or guessed that the command `which` shows location of the command, you're absolutely right! I figured out that `echo` locates in the directory `/usr/bin`. That's the third one out of nine paths printed by `echo $PATH`.

What if I search for a command that's not present on my computer? Searching for a random command `cool` gives no output as shown below:

```bash
$ which cool
$
```

That's because my computer failed to find where it locates Unlike the case `echo`. It means my computer has no idea what to do for the command.

Given that, two requirements are needed if you want to run commands from a new tool in your bioinformatics analysis:

1) installing the tool
2) updating your `$PATH` with a new path storing new commands


For example, documentations for [`Bowtie2`](https://bowtie-bio.sourceforge.net/bowtie2/manual.shtml#building-from-source) and [`HISAT2`](http://daehwankimlab.github.io/hisat2/manual/) remind users to ensure that the directory path for `Bowtie2` or `HISAT2` is added to `$PATH` so every command is visible and executable on users' computer. 


How do I add a path to the variable `$PATH`? As the `$PATH` forms a colon-separated list of multiple paths, it can be simply concatenated with a `:` before- or afterhand.

```bash

# New path added to the last
PATH=$PATH:~/path/to/new/tool

# New path added to the front
PATH=~/path/to/new/tool:$PATH
```


Afterwards, the updated `$PATH` has to be exported (`export PATH=$PATH:~path/to/new/tool`) in the terminal so your current shell picks the change.


Here, a question arises. What if I have an identical command from multiple directories in the `$PATH`? The answer is, it's determined by the order set in the `$PATH`. Revisiting the output of my `echo $PATH` below:

```bash
$ echo $PATH
/home/mira/.cargo/bin:/home/mira/.cargo/bin:/home/mira/miniconda3/bin:/home/mira/miniconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/mira/opt/bin:/home/mira/miniconda3/bin:/home/mira/miniconda3/bin:/home/mira/.fzf/bin
```


Commands are executed preferentially from the directory coming first. It indicates that commands in the `/home/mira/.cargo/bin` will always win no matter what other directories contain the same command. Accordingly, you need to adjust order of paths in your `$PATH` if you want a preferential execution of my analysis tool.


In practice, I barely take care of my `$PATH` even if I get to install a new analysis tool as I depend on [`Conda`](https://docs.conda.io/en/latest/). While my `$PATH` has the following paths set under my default conda env:

```bash
(base) mira@mira-xxxx:~/Downloads/path_variable$ echo $PATH
/home/mira/.cargo/bin:/home/mira/.cargo/bin:/home/mira/miniconda3/bin:/home/mira/miniconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/mira/opt/bin:/home/mira/miniconda3/bin:/home/mira/miniconda3/bin:/home/mira/.fzf/bin
```


Meanwhile, I get an additional path `/home/mira/miniconda3/envs/snakemake_mapping/bin` in the front part of the `$PATH` when having a conda env (`snakemake_mapping` in `/home/mira/miniconda3/envs/snakemake_mapping`) activated as shown below:


```bash
(snakemake_mapping) mira@mira-MS-7C90:~/Downloads/path_variable$ echo $PATH
/home/mira/miniconda3/envs/snakemake_mapping/bin:/home/mira/.cargo/bin:/home/mira/.cargo/bin:/home/mira/miniconda3/bin:/home/mira/miniconda3/condabin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/mira/opt/bin:/home/mira/miniconda3/bin:/home/mira/miniconda3/bin:/home/mira/.fzf/bin
```


It clarifies that any tools/commands which I installed in the conda env will be executed preferentially and that the `Conda` not only manages installation of my analysis tools but also updates my `$PATH` automatically.


