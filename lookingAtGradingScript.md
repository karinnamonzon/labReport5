# Lab Report 5
## Looking at the grading script

In this lab report, we will be looking at the bash grading script and the changes that were made to the original in order to have a better understanding of hwo bash scripts work.

Here is the link to the repository with the original `grade.sh` bash script: [Original](https://github.com/ucsd-cse15l-w23/list-examples-grader)

Here is a link to the repository for my folder with the `grade.sh` Link: [My Grader](https://github.com/karinnamonzon/grader-review-karinnamonzon)

 
### Understanding the grading script

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




### Testing on grading script with files from repositories 
Using `bash sh <link to repository>` to run the bash script on several files:

[https://github.com/ucsd-cse15l-f22/list-methods-lab3](https://github.com/ucsd-cse15l-f22/list-methods-lab3)
This shows how there is one test passing and one failing from the original code in lab 3.

[https://github.com/ucsd-cse15l-f22/list-methods-corrected](https://github.com/ucsd-cse15l-f22/list-methods-corrected)
This shows how there is two test passing after fixing the code in lab 3.

[https://github.com/ucsd-cse15l-f22/list-methods-compile-error](https://github.com/ucsd-cse15l-f22/list-methods-compile-error)

[https://github.com/ucsd-cse15l-f22/list-methods-signature](https://github.com/ucsd-cse15l-f22/list-methods-signature)
[https://github.com/ucsd-cse15l-f22/list-methods-filename](https://github.com/ucsd-cse15l-f22/list-methods-filename)
[https://github.com/ucsd-cse15l-f22/list-methods-nested](https://github.com/ucsd-cse15l-f22/list-methods-nested)
