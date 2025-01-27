# Question 1: Get the data files
Download the following data files from the internet using the curl command: http://eaton-lab.org/pdsb/test.fastq.gz and http://eaton-lab.org/pdsb/iris-data-dirty.csv. Use the less or zless commands to look at each file. Describe what these commands do. Finally, use the head command to print the first 5 lines of the file iris-data-dirty.csv.

# Question 2: Clean the data
Use grep, uniq, and sed for this question. Check that all of the species names are spelled correctly in the file iris-data-dirty.csv. Also check for missing values stored as NA. Create a new file where mispelled names are replaced with the correct values, and lines with NA are excluded, and save it as iris-data-clean.csv. Use cut, sort and uniq to list the number of data values there are for each species in the new cleaned data file. Describe your work.

# Question 3: Find a sequence
Find how many lines in the data file test.fastq.gz start with "TGCAG" and end with "GAG". Describe your work.

# Question 4: Find a sequence chunk
Using grep and other tools if necessary find all lines that contain the sequence "AAAACCCC" and for each print that line, the line above it, and two lines below it (so that a 4-line chunk around each search hit is printed). Describe your work.
