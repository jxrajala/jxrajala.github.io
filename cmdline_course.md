---
layout: default
---

# Command-Line Tools for Linguists Course 

Command-Line Tools for Linguists is a intermediate-level course offered by the University of Helsinki in the Bachelor's Programme in Languages. The learning outcomes of the course consist of basic usage of the Unix command line and of some more specific tools used in computational linguistics.

Even though I am a student in the Phonetics track of the Master's Programme in Linguistic Diversity and Digital Humanities, I selected the course to gain practical knowledge in preparation to master-level courses. I now feel more confident in studying more advanced computational phonetics and language technology. Since I worked on the Ubuntu for Windows shell, the course also sparked an interest in installing an entire Linux OS on my computer.

![University of Helsinki Main Library](https://uni.materialbank.net/NiboWEB/uni/getFile.do?type=thumbnail&uuid=176473&ticket=699303&cartUuid=173956&cart=true&type=thumbnail)

_University of Helsinki Main Library (Kaisa House), photo credit Tuomas Uusheimo.  
Source: University of Helsinki._

## Week 1

The first week or the Command-Line course guided us to set up the Unix shell environment. Since I use a Windows OS, I had to download the Ubuntu for Windows shell. The material of week 1 also introduced basic commands of listing, changing directories, creating, removing, copying and moving files, downloading content from the internet, and using a text editor.

The two most important commands introduced in week 1 are those of changing directory and listing the contents of a directory. They are also the ones that I use the most often when working on the command-line. The commands are presented below.

```
$ cd
$ ls
```

## Week 2

The first part of week 2 introduced the basic commands of copying, moving and deleting in the case of directories. As opposed to individual files, the contents of an entire directory have to be copied and deleted recursively, with the following syntax:

```
$ cp -r example_dir example_dir_backup
$ rm -r example_dir
```
The first part of the week also introduced permissions and compressing. Changing permissions of a file seemed complicated to me; therefore I have frequently used the command presented below that gives read, write and execute permissions to owner only.

```
$ chmod 700 example.txt
```
The second part of the week dealt with processes. Even though their handling is a useful skill to have, I didn't really use process management in the subsequent weeks. The third part was about connecting to remote servers and using the CSC Puhti server in particular. I'm looking forward to working on remote servers in my future projects.

## Week 3

Week 3 started to delve into corpus processing and commands used in computational linguistics, after a short introduction to character encoding. The first part taught us to create a sorted list of unique word types with the following steps:
 1. tokenization: i.e. replacing all space and punctuation character with newlines (and getting rid of extra newlines with squueze option)
 2. sorting the words in an alphabetical ording and ignoring case
 3. finally, removing duplicate words and ignoring case: this gives us a list of unique word types

The steps above can be executed on the command-line with the following commands:
```
$ cat example.txt | tr -s "[:space:][:punct:]" "\n" | sort -f | uniq -i > example.word_list.txt
```
The second part of week 3 introduced simple regular expressions and the grep command. I was able to use my newly-acquired skills with command-line regular expressions in the Introduction to Language Technology course that I had simultaneously. I used the regular expressions presented in the table below in order to search for different parts of speech in a POS-tagged English text:

| part of speech | regex syntax     |
|----------------|------------------|
| nouns          | `*_N[NP]?`       |
| verbs          | `*_V[BDHV]?`     |
| adjectives     | `*_JJ?`          |
| pronouns       | `*_PP? and *_WP?`|
| determiners    | `*_DT`           |
| adverbs        | `*_RB?`          |

The last part of week 3 gave an introduction to formatted text files such as CSV (comma-separated values). 

## Week 4

Week 4 introduced some more advanced corpus processing, namely with the sed command and pipelines, combined with regular expressions. The first language technology application learnt was to generate a frequency list of a text file with the following pipeline:
```
cat example.txt | tr -s '\n\r\t ' '\n' | tr -dc "[:alnum:]\n'" | sort | uniq -c | sort -nr > example.freq
```
The second operation learnt was to transform a text into sentence-per-line format using the sed command with the following syntax:
```
cat example.txt | dos2unix | sed 's/^$/#/' | tr '\n' ' ' | sed -E 's/([.?!]) ([A-Z])/\1# \2/g' | tr '#' '\n' | sed 's/^ *//' | sed 's/ *$//' | grep -v '^$' > example.sentences.txt
```
The final operation introduced was transforming a text file in sentence-per-line format into a list of trigrams. It started first by creating a three lists of unigrams starting at the first, second and third words, respectively, and then combining these unigrams into a single trigram file as follows:
```
cat example.sentences.txt | sed -E 's/^/# /' | sed -E 's/$/ #/' | sed -E 's/([a-zA-Z])([;:,.?!])/\1 \2/g' | tr -s '[:space:]' '\n' > example.unigram
cat example.unigram | tail -n +2 > example.unigram_plus_one
cat example.unigram | tail -n +3 > example.unigram_plus_two
paste -d" " example.unigram example.unigram_plus_one example.unigram_plus_two > example.trigram
```

## Week 5

In week 5, we learnt how to write simple shell scripts that would replace consecutive commands typed one by one. The material also discussed environmental variables and bash configuration. The latter subject seemed less relevant to me at this point since I am a novice in command-line; the shell scripts, on the other hand, seemed very useful. Since scripts perform multiple commands in a single call, they resemble programme code a lot. The frequency list, created with a pipeline in week 4, can be created with the following script:
```
#! /bin/bash

if [ $# -ne 2 ]
then
  echo "ERROR: Two command line arguments required"
  echo "$0 input_text_file output_freq_file"
  exit 1
fi

cat $1 |
dos2unix |
tr -s "[:space:]" "\n" |
tr -d "[:punct:]" |
sort |
uniq -c |
sort -nr >$2
```

## Week 6

Week 6 introduced a central functionality in Unix systems, namely installing software and changing users. The ease of installing software in Unix is considerable compared to Windows since all you need to do is to give the following command in the command-line:
```
sudo apt-get install <software name>
```
The second part of the week introduced the make utility. After the shell scripts introduced in week 5, it was a step further towards programming. The make utility really revealed the usefulness of the Unix shell in building projects.

## Week 7

The final week of the course was about version control in project development. We were presented, Git which is the one of the most commonly used and flexible systems, as well as Github, which is the one of the most well-known and widely used Git support platforms. The key processes in Git are adding changes to the staging area, then committing them, and finally pushing the changes to remote repositories for collaboration. The command syntax is presented below:
```
git add <filename>
git -m commit "Commit message"
git push origin master
```
In addition to learning how to use version control with Git, students created their own Github account. After creating our account, the final assignment of the Command-Line Course was to create a homepage on Github pages. The homepage was made using Jekyll, a static site generator. This homepage is the result of my final assignment; however, I will continue updating it in the future to present my educational and professional background.
