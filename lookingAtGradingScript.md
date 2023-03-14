# Lab Report 5
## Looking at the grading script

In this lab report, we will be looking at the bash grading script and the changes that were made to the original in order to have a better understanding of hwo bash scripts work.

Here is the link to the repository with the original `grade.sh` bash script: [Original](https://github.com/ucsd-cse15l-w23/list-examples-grader)

Here is a link to the repository for my folder with the `grade.sh` Link: [My Grader](https://github.com/karinnamonzon/grader-review-karinnamonzon)

 
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

### Explaining parts of `bash.sh`

### Testing on grading script with files from repositories 
Using `bash sh <link to repository>` to run the bash script on several files:

[https://github.com/ucsd-cse15l-f22/list-methods-lab3](https://github.com/ucsd-cse15l-f22/list-methods-lab3)
Running this repository with the grading script shows how there was a succesful compile with the message `SUCCESSFUL COMPILE` after finding the correct file. This also shows how there is one test run and one failing from the original code in lab 3. 
![first]()

[https://github.com/ucsd-cse15l-f22/list-methods-corrected](https://github.com/ucsd-cse15l-f22/list-methods-corrected)
Running this repository with the grading script shows how there is one test run and passing after fixing the code in lab 3.
![second]()

[https://github.com/ucsd-cse15l-f22/list-methods-compile-error](https://github.com/ucsd-cse15l-f22/list-methods-compile-error)
Running this repository with the grading script shows that on line 15 there is a missing semicolon which results in a compile error. This is shows in the results in the terminal with the message `FAILURE TO COMPILE`
![third]()


[https://github.com/ucsd-cse15l-f22/list-methods-filename](https://github.com/ucsd-cse15l-f22/list-methods-filename)
Running the grading script with a repository that is missing the `ListExample.java` file shows how it will result in the message `ListExamples.java not found`. There are no tests that are compiled and ran because there is no file to test them from.
![fourth]()


