Download Link: https://assignmentchef.com/product/solved-coen177-lab-8-memory-management
<br>



<strong>Objectives</strong>

<h5>1.     To simulate two kinds of basic page replacement algorithms</h5>

<h5>2.     To evaluate the performance, in terms of miss/hit rate, of these algorithms</h5>

<h5></h5>

<h5><strong>Guidelines</strong></h5>

The goal of this assignment is to gain experience with page replacement (and to a lesser extent, caching) algorithms. In this assignment, your goal is to write programs that simulate page replacement algorithms. Your initial program is to accept at least one numeric command-line parameter, which it will use as the number of available page frames. In this case:

int main(int argc, char *argv[]){

int cacheSize = atoi(argv[1]); // Size of Cache passed by user




A simulation of a page replacement algorithm will then use cacheSize as a number of pages/blocks of memory/cache size. This value can be read for example using “lru” as follows:




<strong>$lru 27</strong>

or

<strong>$simulate -lru 7</strong>




Your program should expect page requests to arrive on standard input (<strong>stdin)</strong>, so a basic <strong>fgets()</strong>, or <strong>scanf()</strong> call cab be used to read in the unsigned integer page numbers being requested. Assuming you have a sequence of page numbers in a text file called “testInput.txt” you should be able to run your simulator by typing:




<strong>$cat testInput.txt| lru 42</strong>

<strong> </strong>

<h5><strong>Generate testInput.txt </strong><strong>file</strong></h5>

<ul>

 <li>Write a C program to generate testInput.txt file. You may use the following code snippet:</li>

</ul>




int main(int argc, char *argv[]) {

FILE *fp;

char buffer [sizeof(int)];

int i;

fp = fopen(“testInput.txt”, “w”);

for (i=0; i&lt;someNumber; i++){

sprintf(buffer, “%d
”, rand()%capNumber);

fputs(buffer, fp);

}

fclose(fp);

return 0;

}




<h5><strong>Date types and definitions</strong></h5>

<ul>

 <li>Define the following data types in your page replacement code:</li>

</ul>

typedef struct {

int pageno;

. . .

} ref_page;




ref_page cache[cacheSize]; // Cache that stores pages

char pageCache[100]; // Cache that holds the input from test file

int totalFaults = 0; // keeps track of the total page faults




<h5><strong> </strong></h5>

<h5><strong>Read page requests iteratively</strong></h5>

<ul>

 <li>Write a C program to read iteratively the output of <strong>$cat testInput.txt</strong>pipelined as a standard input to the executable page replacement program. This can be implemented using</li>

</ul>

char *fgets(char *str, int n, FILE *stream)




Where fgets( ) library function reads a line from the specified stream and stores it into the string pointed to by str. It stops when either (n-1) characters are read, the newline character is read, or the end-of-file is reached, whichever comes first. So, you may capture the page number pipelined to the standard input as follows:

while (fgets(pageCache, 100, stdin)) {

int page_num = atoi(pageCache); // Stores number read from file as an int




In this case, your program will accept page requests on stdin as individual numbers, one per line, where each number indicates the requested page number. Each program is to further ignore any trailing text on the input lines, or any lines that do not start with a number. Your program terminates its simulation when it encounters an end-of-file.




Test for memory sizes of between 10 and 500 pages.




<h5><strong>Page replacement algorithm</strong></h5>

<ul>

 <li>Write a program for each of the following replacement algorithms: FIFO, LRU, and Second Chance Page Replacement.</li>

</ul>




FIFO (First In First Out): is the simplest page replacement algorithm that keeps track of all the pages in the memory in a queue with the oldest page at the front of the queue. On a page fault, it replaces the oldest page that has been present in memory for the longest time. The following code snippet is provided as a guidance. A similar code can be implemented for the LRU and 2<sup>nd</sup> chance:




bool foundInCache = false;

for (i=0; i&lt;cacheSize; i++){

if (cache[i].pageno == page_num){

foundInCache = true;

break; //break out loop because found page_num in cache

}

}

if (foundInCache == false){

//You may print the page that caused the page fault

cache[placeInArray].pageno = page_num;

totalFaults


placeInArray++; //Need to keep the value within the cacheSize

}

Check pseudo code<a href="#_ftn1" name="_ftnref1">[1]</a>




LRU (Least Recently Used): replaces the page that was previously least recently used. You may need to use an index to keep track of the most recently used page. Check pseudo code<a href="#_ftn2" name="_ftnref2">[2]</a>




2<sup>nd</sup> Chance or Clock: gives every page a second chance in the sense that an old page that has been referenced is likely in use and therefore, will not swapped out over a new page that has not been referenced. A queue may be created and will be checked, but instead of paging the page out, a referenced bit would be checked to see if it is set. The page would be swapped out if the reference bit is not set, otherwise the page is inserted at the back of the queue. A counter needs to be implemented to keep track of the reference bit. Check pseudo code.<a href="#_ftn3" name="_ftnref3">[3]</a>




<h5><strong>Expected </strong><strong>Output</strong></h5>

<ul>

 <li>The output of a page replacement program will be every page number that was <strong>not</strong> found to be in the cache. In other words, the output will be a sequence of page numbers that represents all the incoming requests that resulted in a page fault. Using your program, you should be able to get two numbers from the Unix command line (by counting the number of lines read from the input file, and the number of lines produced by your simulator). The first of these numbers is the total number of page/block requests your simulator program has received (you get this by counting the number of valid lines in your input file), and the second number is how many of these page requests did result in a page fault (you get this by counting the number of lines produced as output by your program – which is faithfully reproducing the page replacement algorithm’s behavior).</li>

</ul>




Once again, the size of the memory being managed by your program (the number of page frames, or the size of the cache if you treat this as a caching algorithm) is to be accepted as a command-line argument to your program. Any status output (e.g., messages you wish to print for debugging/user) should be sent to stderr (standard error, in other words, it should be possible to use your program and see nothing in standard output other than the page-faults/cache-misses, by redirecting only stdout).




<h5><strong>Test your Page Replacement Algorithms using a series of commands in a shell script</strong></h5>

<ul>

 <li>In lab 1, you learned about shell scripting as a tool that allows you to execute a series of commands by running a shell program that contains them instead of typing all commands. Create a .sh file to generate different test cases, e.g.</li>

</ul>

#!/bin/bash

make;

echo “———-FIFO———-”

cat testInput.txt | ./fifo 10

echo “———-End FIFO———-”

echo

echo “———-LRU———-”

cat testInput.txt | ./lru 10

echo “———-End LRU———-”

echo

echo “———-Second Chance———-”

cat testInput.txt | ./sec_chance 10

echo “———-End Second Chance———-”




echo “FIFO 10K Test with cache size = 10, 50, 100, 250, 500”

cat testInput10k.txt | ./fifo 10 | wc -l

cat testInput10k.txt | ./fifo 50 | wc -l

cat testInput10k.txt | ./fifo 100 | wc -l

cat testInput10k.txt | ./fifo 250 | wc -l

cat testInput10k.txt | ./fifo 500 | wc -l

echo

echo “LRU 10K Test with cache size = 10, 50, 100, 250, 500”

cat testInput10k.txt | ./lru 10 | wc -l

cat testInput10k.txt | ./lru 50 | wc -l

cat testInput10k.txt | ./lru 100 | wc -l

cat testInput10k.txt | ./lru 250 | wc -l

cat testInput10k.txt | ./lru 500 | wc -l

echo

echo “Second Chance 10K Test with cache size = 10, 50, 100, 250, 500”

cat testInput10k.txt | ./sec_chance 10 | wc -l

cat testInput10k.txt | ./sec_chance 50 | wc -l

cat testInput10k.txt | ./sec_chance 100 | wc -l

cat testInput10k.txt | ./sec_chance 250 | wc -l

cat testInput10k.txt | ./sec_chance 500 | wc -l

echo

make clean;

echo







make is a utility that requires a Makefile, which defines set of tasks to be executed, e.g. your Makefile may include:

all: fifo.c lru.c sec_chance.c

gcc -o lru lru.c

gcc -o fifo fifo.c

gcc -o sec_chance sec_chance.c




clean:; rm -f *.out lru fifo sec_chance




<a href="#_ftnref1" name="_ftn1">[1]</a> https://www.geeksforgeeks.org/program-page-replacement-algorithms-set-2-fifo/

<a href="#_ftnref2" name="_ftn2">[2]</a> https://www.geeksforgeeks.org/program-for-least-recently-used-lru-page-replacement-algorithm/

<a href="#_ftnref3" name="_ftn3">[3]</a> <a href="https://www.geeksforgeeks.org/second-chance-or-clock-page-replacement-policy/">https://www.geeksforgeeks.org/second-chance-or-clock-page-replacement-policy/</a>