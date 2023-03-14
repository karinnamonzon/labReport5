# Lab Report 5
## Looking at the grading script

In this lab report, we will be looking at the bash grading script and the changes that were made to the original in order to have a better understanding of hwo bash scripts work.

Here is the link to the repository with the original `grade.sh` bash script: [https://github.com/ucsd-cse15l-w23/list-examples-grader](https://github.com/ucsd-cse15l-w23/list-examples-grader)

Here is a link to the repository for my folder with the `grade.sh` [https://github.com/karinnamonzon/grader-review-karinnamonzon](https://github.com/karinnamonzon/grader-review-karinnamonzon)

 
## Understanding the grading script
### Changing bash.sh

The original `bash.sh` file looked like this:
```
CPATH='.:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar'

rm -rf student-submission
git clone $1 student-submission
echo 'Finished cloning'

cp student-submission/ListExamples.java ./
javac -cp $CPATH *.java
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples
```

After making changes to make the script work properly, this is what the `bash.sh` file looks like:
```
CPATH=".;../lib/junit-4.13.2.jar;../lib/hamcrest-core-1.3.jar"

rm -rf student-submission

git clone $1 student-submission
echo 'Finished cloning student-submission'

if [[ -f "student-submission/ListExamples.java" ]]
then
    echo 'ListExamples.java found'
else
    echo 'ListExamples.java not found'
    exit 1
fi 

cp TestListExamples.java student-submission/

cd student-submission

javac ListExamples.java
if [[ -f "ListExamples.class" ]] 
then
    echo "SUCCESSFUL COMPILE"
else
    echo "FAILURE TO COMPILE"
    exit 2
fi

javac -cp $CPATH *.java
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples

java -cp $CPATH org.junit.runner.JUnitCore student-submission/*.java > output.txt

grep "Tests run" output.txt > results.txt

TESTSRUN=` cut -d ',' -f 1 results.txt `

FAILURES=` cut -d ',' -f 2 results.txt `

NUMFAILURES="${FAILURES//[^0-9]/}"

NUMTESTS="${TESTSRUN//[^0-9]/}"

```

### Explaining parts of the fixed `grader.sh`

```
CPATH=".;../lib/junit-4.13.2.jar;../lib/hamcrest-core-1.3.jar"

rm -rf student-submission

git clone $1 student-submission
echo 'Finished cloning student-submission'
```
In the code above, there are no changes made from the original lines of `grader.sh`. These lines are used to set up the script so that it is new. The `CPATH` line is used a way to call to a working directory of the `.jar` file that will run the tests. The `rm -rf student-submission` line removes the `student-submission` clone file that may already be saved. `git clone $1 student-submission` creates a new clone that will be used and `echo 'Finished cloning student-submission'` gives indication that the new `student-submission` has been cloned and is ready for the rest of `grade.sh` to run.

```
if [[ -f "student-submission/ListExamples.java" ]]
then
    echo 'ListExamples.java found'
else
    echo 'ListExamples.java not found'
    exit 1
fi 
```
This if-else statement looks for the file `ListExamples.java` in the `student-submission` directory. If the file is found then the terminal will print `ListExamples.java found` which is seen by the `echo` command in the statement. It will print `ListExamples.java not found` if the file is not dound and then has `exit 1` which will stop the execution of the rest of `grader.sh`.

```
cp TestListExamples.java student-submission/

cd student-submission

javac ListExamples.java
if [[ -f "ListExamples.class" ]] 
then
    echo "SUCCESSFUL COMPILE"
else
    echo "FAILURE TO COMPILE"
    exit 2
fi
```
Copying `TestListExamples.java` to the `student-submission/` directory and then using the `cd` command to go into that directory, the `ListExample.java` file is compiled with the use of `javac`. The if-else statement looks for the file `ListExamples.class` that is created by a successful compile, this is indicated by the message `SUCCESSFUL COMPILE` seen in the terminal which uses the `echo` command. If the `ListExamples.class` is not found that means the compile was a failure and the message `FAILURE TO COMPILE` is seen in the terminal which also uses the `echo` command, this then exits from the `grader.sh` script with the `exit 2` command.


```
javac -cp $CPATH *.java
java -cp $CPATH org.junit.runner.JUnitCore TestListExamples

java -cp $CPATH org.junit.runner.JUnitCore student-submission/*.java > output.txt

grep "Tests run" output.txt > results.txt
```
This section in the grading script runs the tester using the `CPATH` directory established at the beginning. This compiles all `*.java` files with `javac` and then runs the tests from the `TestListExamples` file using the `java` command. The result of the tests are put into the `output.txt` file. To help with the formatting within the terminal, these results are saves into `result.txt` with the `grep` command that looks for the `Tests run` pattern.

```
TESTSRUN=` cut -d ',' -f 1 results.txt `

FAILURES=` cut -d ',' -f 2 results.txt `

NUMFAILURES="${FAILURES//[^0-9]/}"

NUMTESTS="${TESTSRUN//[^0-9]/}"
```
These lines are used for formatting the grepped results in `result.txt`. `TESTSRUN` counts and cuts the number value of tests run for `results.txt` while `FAILURES` counts and cuts the number value of tests failed from `results.txt`. `NUMFAILURES` uses the value from `FAILURES` and uses that to format the number of failures in the terminal after the tests have finished. `NUMTESTS` uses the value from `TESTSRUN` and uses that to format the number of tests run in the terminal after the tests have finished. 


## Testing on grading script with files from repositories 
Using `bash grade.sh <link to repository>` to run the bash script on several files:

`bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-lab3`
Running this repository with the grading script shows how there was a succesful compile with the message `SUCCESSFUL COMPILE` after finding the correct file. This also shows how there is one test run and one failing from the original code in lab 3. 
![first](https://github.com/karinnamonzon/labReport5/blob/main/firstTest.png?raw=true)

`bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-corrected`
Running this repository with the grading script shows how there is one test run and passing after fixing the code in lab 3.
![second](https://github.com/karinnamonzon/labReport5/blob/main/second.png?raw=true)

`bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-compile-error`
Running this repository with the grading script shows that on line 15 there is a missing semicolon which results in a compile error. This is shows in the results in the terminal with the message `FAILURE TO COMPILE`
![third](https://github.com/karinnamonzon/labReport5/blob/main/third.png?raw=true)

`bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-filename)`
Running the grading script with a repository that is missing the `ListExample.java` file shows how it will result in the message `ListExamples.java not found`. There are no tests that are compiled and ran because there is no file to test them from.
![fourth](https://github.com/karinnamonzon/labReport5/blob/main/fourth.png?raw=true)


