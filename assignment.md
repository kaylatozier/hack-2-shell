# Question 1: Get the data files
Download the following data files from the internet using the curl command: http://eaton-lab.org/pdsb/test.fastq.gz and http://eaton-lab.org/pdsb/iris-data-dirty.csv. Use the less or zless commands to look at each file. Describe what these commands do. Finally, use the head command to print the first 5 lines of the file iris-data-dirty.csv.

**My solution:** This was my first time using the `curl` command, so it was tricky at first to figure it out. I kept getting an HTML error, and then ended up searching this error on the internet and finding the option `-O` to keep the remote name as the output name. For some reason, this fixed the problem once but then the error came back. I spent a long time trying to figure it out using other internet sources and `man curl` but because I am still new to options, the manual was a bit overwhelming. Going over it in class today was very insightful, as I learned that the option `-L` needed to be used to redirect the data, informed by the "Permanently Moved" message in the error. Then, I used `man head` to figure out how to print five lines of code with the `head` command, which was the option `-n 5`. 

```bash
curl -L -O http://eaton-lab.org/pdsb/test.fastq.gz
curl -L -O http://eaton-lab.org/pdsb/iris-data-dirty.csv
head -n 5 iris-data-dirty.csv
```
# Question 2: Clean the data
Use grep, uniq, and sed for this question. Check that all of the species names are spelled correctly in the file iris-data-dirty.csv. Also check for missing values stored as NA. Create a new file where mispelled names are replaced with the correct values, and lines with NA are excluded, and save it as iris-data-clean.csv. Use cut, sort and uniq to list the number of data values there are for each species in the new cleaned data file. Describe your work.

**My solution:** Because I got stuck on the first question, the rest of these questions were answered with the help of the assignment review in class today. Once I had the data correctly downloaded, I could easily use `grep` to find the missing values, called NA, in the data. Then, I used the `grep -v` option to find data that didn't contain any NA, since this is the data we want to work with, and saved it under a new name "iris-data-noNA.csv" using the `>` command. To check for spelling mistakes, we first had to isolate the columns with words in the data, and get rid of the ones with just numbers. This was the fifth row in every line, so I used `cut -f 5 -d ','` to tell the program to cut off everything until the fifth column, and told it to define columns using the commas. I learned to use the `uniq` command to look for lines of unique words in this list of words, but had to sort them because the `uniq` function just compares pairwise, not globally, creating duplicates. I also used the `-c` option to count the rows with that name to make it clear which was the typo, and which was correct, where the correct spelling should have many more occurances. I then used the `sed` command to replace the spelling mistakes with the correct spelling, and saved this list to a new fire called "iris-data-clean.csv". 

```bash
$ grep NA iris-data-dirty.csv
$ grep -v NA iris-data-dirty.csv > iris-data-noNA.csv
$ cut -f 5 -d ',' iris-data-noNA.csv | sort | uniq -c
$ grep -v NA iris-data-noNA.csv | sed 's/Iris-setsa/Iris-setosa/g' | sed 's/Iris-versicolour/Iris-versicolor/g' > iris-data-clean.csv
```

# Question 3: Find a sequence
Find how many lines in the data file test.fastq.gz start with "TGCAG" and end with "GAG". Describe your work.

**My solution:** With the help of the assignment review, I learned that you can unpack the data with gunzip using `-c` to push the data to stdout instead of having to unzip the file in place. In order to find the files that both start with "TGCAG" and end with "GAG" we have to pipe with grep so that it first finds TGCAG at the beginning of the line with the `^` and using `$` at the end of the line. I'll also pipe `wc -l` to count the lines. Doing so gives me the solution of 44 lines. 

```bash
gunzip -c test.fastq.gz | grep "^TGCAG" | grep "GAG$" | wc -l
```

# Question 4: Find a sequence chunk
Using grep and other tools if necessary find all lines that contain the sequence "AAAACCCC" and for each print that line, the line above it, and two lines below it (so that a 4-line chunk around each search hit is printed). Describe your work.

**My solution:** First I used `grep` to find the lines with the sequence "AAAACCCC". From the `man grep` we learned that you can use the option `-A #` for x amount of lines after and option `B #` for x amount of lines before. 

```bash
$ gunzip -c test.fastq.gz | grep AAAACCCC
$ gunzip -c test.fastq.gz | grep AAAACCCC -B 1 -A 2
```

This prints out:
```
@32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
TGCAGAATAGATAGGAAACGTTTTGGCGCTGTAGACATTAAAACCCCAGTAGGACACGGGTATCACAACGTACA
+32082_przewalskii.98 GRC13_0027_FC:4:1:5669:1669 length=74
IIIIIIIIIIIIIIIIIIHIHIIIIIIIIGIIIGIIIIIIHIIIIIIIHIIIIHIIIIIIEHIHHIIIIICIHI
--
@33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
TGCAGTGGATCGAAAACCCCGAGGCTCAAGGTCACGCCACCGTCTTCGTGGCCAAGTTCTTCGGCCGCGCCGGC
+33413_thamno.59 GRC13_0027_FC:4:1:5000:1620 length=74
IIIIIIIIIIIIIIIIIIIIDHIIHHIIIIIEIBGBGGGIIHEHHHIEBBHHIEGGDGIGGHAEFDBFBDDB?D
```
