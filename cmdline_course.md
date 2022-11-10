# Command Line Tools for Linguists Course

This course was about the command-line environment, specifically focusing on command line tools that linguistics would find helpful. This includes executing a variety of tasks involved with data processing, corpus work, and more.

Overarching goals of this course included:

-   Mastering use of command-line tools
-   Gaining familiarity and experience using regular expressions
-   Gaining experience and confidence in processing text corpora using command line tools
-   Running and installing programs from the command line
-   Writing simple scripts
-   Working on the server

## Week 1: Introduction

The first week of the course was focused on introducing us to the command line and getting everything set up on our computers. Here we covered some basic commands such as `cd`, `mkdir`, and `rm`. We also learned escape commands such as `ctrl-c`. In addition to these basic commands to get us comfortable, we briefly went over the history of Linux.

Our exercise for this week included using `wget` and emacs. This introduced us to concepts needed later for corpus processing. Below is a snippet of code that was called in the command line for this exercise:

``` bash
  wget https://www.gutenberg.org/files/215/215-0.txt 
  mv 215-0.txt call_of_the_wild.txt
  emacs
```

I got a copy of the book *Call of the Wild* from Project Gutenberg using the `wget` command with the URL of the location of the book and then used `mv` to rename the file so it would be easier to identify. Then I opened emacs by calling for it in the Terminal.

I had used command line infrequently in the past before taking this course and was already familiar with a few of the command introduced this week. As may be expected, learning the escape command `ctrl-c` was invaluable, especially when working with emacs.

## Week 2: Navigating a UNIX System

In week 2, we went over some of the commands introduced in week 1, such as `mkdir`, while also practicing new commands like `mv` and learning key symbols such as \* and ?.

We also learned about the file system, the scp and ssh and practiced logging into Puhti - a supercomputer.

An example of how the command for `scp` is formatted to copy a file from your local computer to a server is below.

``` bash
  scp username@server: /home/username/examplefile username@server: /home/username/filepath/
```
The reminders that SCP can only be done from your own computer, you can't be logged into the server came in handy a few weeks ago when I was trying to download a file from the server onto my computer. After a few errors, I remembered how I should be using SSH to grab the file but not actually have signed into the server using SSH! I appreciated being able to get comfortable remoting into a server through this course. Since then I've used it several times in many other classes. 

## Week 3: Basic Corpus Processing

In week 3 we learned about:

-   streams, stream redirections, and pipes

-   character encoding

-   regular expressions

-   `grep`

All of these were learned within the context of corpus processing. The assignment this week required some of the following sorts of commands:

```bash
  wget https://www.gutenberg.org/cache/epub/4511/pg4511.txt #get file from project gutenberg
  mv pg4511.txt life_of_bee.txt #rename file
  cat life_of_bee.txt | grep -E "\bpre(\w|-)*ed\b" | wc -l
```

Above, we see various command line commands used in the exercise in Week 3. Much like in Week 1, we use `wget` to retrieve a book from Project Gutenberg and then rename it. One of the exercises was to use regular expressions to find the wordcount of words that begin with the string 'pre' and end with the string 'ed'. Above you can see the regular expression used where a word boundary was identified with `\b` followed by the initial string we're looking for. Between the 'pre' and 'ed' could be any number of other characters or hyphens indicated by `(\w|-)*`. Finally we end with the expected end string 'ed' and the final word boundary. This is then piped to the command `wc` so all what shows up on the terminal is the word count within the text that fits the conditions established by the regular expression. 

``` bash
  #create word list
  cat life_of_bee.txt | tr -s "[:space:][:punct:]" "\n" | sort | uniq -i > bee_word_list.txt
```

Another task was to create a word list. First we read the text using `cat`, pipe this into the `tr` command using the pipe `|`. We transform characters like spaces and punctuation into new lines, but avoid empty new lines, as seen by using `tr -s`. This brings all new words on a new line for our list of words. We use the `uniq` command to remove duplicate lines, the results of these get redirected to a new file *bee_word_list.txt* with the `>` symbol.

Learning how to use streams turned out to be quite useful in later tasks for this course. Being able to redirect output into new files or append to the ends of files was very useful in Week 4 as we continued to do corpus processing. Similarly, being able to use pipes to redirect data from one command to another is incredibly useful. This meant I could accomplish several commands in the same line at once in the terminal without needing to direct outputs to file after file. I see using pipes and stream redirects as commands I use on a regular basis as I continue to use the command line. 

## Week 4: Advanced Corpus Processing

In week 4, we continued working on corpus processing skills. Namely, we focused on `sed`: the stream editor. `sed` can be used to replace patterns but also can be used to find patterns, like `egrep`.

``` bash
  sed -nE '/that [a-z]+d[ \.,?:!$]/p' ulysses.txt  
```

For instance, in the code block above, we can see `sed` being used to locate a pattern "that ...d" in the file *ulysses.txt*. The capital e, `-E` is a flag for regular expressions, which is what we write out to indicate which pattern we're searching for. The flag `-n` suppresses the typical behavior of `sed` to echo each line to the standard output after reading through it.

To be frank, it took me quite a while to feel comfortable using `sed`. Notably, it was difficult to remember that `sed`'s default behavior is to capture the first occurrence rather than all of them. 

## Week 5: Scripting & Configuration Files

Week 5 was when we were introduced to bash script and practiced scripting files.

Some variable names we learned included:

| Variable |                 Description                  |
|:--------:|:--------------------------------------------:|
|   \$0    |             name of bash script              |
|  \$1-9   |           first 9 arguments passed           |
| \$RANDOM |           returns a random number            |
|   \$?    | exit status of the most recently run process |

We practiced creating several small bash scripts to get the hang of it.

![Drafted bash script mydiff.sh](/assets/img/mydiff.sh_image.png)

In the image above, we see a bash script which simply repurposes the `diff` command for a slightly more complicated script which takes two files in (stored as variables 1 and 2). If they are equal the script should print out a 0, if they aren't equal it should print out a 1. Conveniently, these are the values of the exit status of diff (stored in the variable \$?) and so we can simply use `echo` to print out the variables we want.

This could also have been written as in the code block below, however I had wanted to practice my if statements, using the `if … fi` construction.

``` bash
diff $1 $2 >/dev/null
echo $?
```

As I go deeper and deeper into my education for language technology, I see huge benefits in being able to draft your own scripts to run regular programs or pipelines to help with processing data. Especially if a task is rote and you don't expect any particular surprises.

## Week 6: Installing and Running Programs

In week 6, we covered the topics of package managers, installing software, and using makefiles.

I had used pip several times before but never had thought about what it is and what exactly it's doing, other than allowing me to install whatever package or software I was looking for. I had also used virtual environments before, but those I had created withing my Anaconda Navigator rather than through my terminal. It was actually quite easy to create and activate a virtual environment within my terminal, I simply have to use code like this:

``` bash
python3 -m venv venv 
source venv/bin/activate
```

This will be very useful to use when I want to activate a virtual environment within terminal. I had also never encountered the `make` command before or Makefiles. I primarily use Anaconda Navigator these days and set up virtual environments outside of the terminal, however I now have more experience using pip as well as creating adn activating virtual environments through the terminal rather than going through another app.

## Week 7: Jekyll & Github Pages

For our final week of class, we covered git, github pages, and jekyll.

Git is a version control system that consists of a main branch other branches which can be used to draft and test out code that you aren't ready to finalize. After creating a project on Github, it's easy to clone the project to your local repository using some simple code.

``` bash
  git clone repo-URL
  git checkout branch-name
  git push repo branch
```

After creating a local repository of the project you can create your own branches using `git checkout` as seen in the code block above, or you can push changes to the remote repository for others to access, as depicted in the final line of code in the code block.

Before this course, I hadn't used Github very much, but for the times I did use it, I primarily used the web interface. I hadn't used git on the command line previously. I had however, created Github Pages for classes and had used Markdown previously so this wasn't completely new to me.

This is where jekyll comes in! Jekyll takes written content (such as Markdown files) to generate static sites - this is what is used for Github pages! As our final project, we are to create a github page (as you see now) that is generated by markdown files. Most of this file is simply text, but there are some fancier elements like code chunks or tables. The tables require some fun layouts to create them! Below is what the table looks like before Jekyll makes it look more user friendly:

``` markdown
| Variable |                 Description                  |
|:--------:|:--------------------------------------------:|
|   \$0    |             name of bash script              |
|  \$1-9   |           first 9 arguments passed           |
| \$RANDOM |           returns a random number            |
|   \$?    | exit status of the most recently run process |
```

It's very convenient to be able to have Github Pages. Sharing code and ensuring it is accessible to others is very important. Being able to host a summary page that can easily associated with the code stored on github helps achieve visibility for a project as well as provide ease of use or understanding to those examining the code or project.

