# Lab 3 - Debugging

Here's the link to the repository containing the code we are working with: <br>
[https://github.com/nidhidhamnani/markdown-parser.git](https://github.com/nidhidhamnani/markdown-parser.git)

## Step 1 - Identifying a Symptom
When trying to run the code with the given test file (*test-file.md*), we run into an OutOfMemoryError in the Java heap space:

![](/LabRep2Pics/Bug1.png)

This error is the symptom that the bug is causing. Now that a symptom is identified along with a failur-inducing input, we can take a look at the code to see which part of the code (aka the bug) is causing this error.

## Step 2 - Using the Debugger
Since the debugger does not allow for command line inputs when running the code (the args array), we can make an edit to the code so that it reads *test-file.md* like so:

![](/LabRep2Pics/CodeChange1-ReadFile.png)

Then, using the debugger in Visual Studio Code, we can go through the code line by line to identify where the bug is. To do so, we first need to set breakpoints so that we can use the debugger to run one line of code at a time.

We can set these breakpoints by first hovering our pointer over the area to the left of the line numbers and then clicking on the red circle that appears.

Then, we can navigate to the *Run and Debug* function using the control panel to the left and click the blue *Run and Debug* button to begin.

The interface of the debugger will look something like this:

![](/LabRep2Pics/VSCDebugger.png)

As can be seen in the screenshot above, the debugger stopped at where I placed a breakpoint.

Using the control panel located in the middle at the top of the screen, we can control to the debugger to, in order from left to right, run until the next breakpoint, run the next line of code, step into a method, step out of a method, restart, or stop.

## Step 3 - Identifying the Bug
Using the debugger, I was bale to figure out that the while loop is the source of the bug. Specifically, the terminating condition for the loop is never reached.

![](/LabRep2Pics/InfiniteLoop.png)

As seen in the screenshot, in the variables section, the while loop has already ran twice to obtain the two URLs that are in the file. However, the terminating condition of `current index >= markdown.length` is never reached.

## Step 4 - Using Print Statements to Get Details
To properly understand why the bug exists, we need to get more details about the code and the failure-inducing input. Specific to this case, we should look at the length of the markdown file and the values of `currentIndex`.

I added a print statement for the length of the markdown file since it is not displayed in the debugger. This is so that I know the value that `currentIndex` should reach in order to terminate the loop.

![](/LabRep2Pics/CodeChange1-PrintLength.png)

Running the debugger again with this new print statement prints to the console the following line:

```
Length of markdown file: 65
```

Now that I know that currentIndex needs to reach 65 to terminate the loop, I can begin to identify the cause of the bug.

## Step 5 - Making an Assumption of the Cause of the bug
By tracing the values of `currentIndex` while the code runs, I find out that the largest value it reaches is 64, which is 1 less than the required 65 to end the loop.

Through looking at the code, the reason that `currentIndex` ends up at 64 is because it is set to `closeParen + 1`. Since the index of the close parenthesis character is 63, the value of `closeParen` is 63. However, due to the extra line at the end of the test file on line 5, (as shown in the screenshot below) there is a new line character which increases the length of the markdown file to 65.

![](/LabRep2Pics/Bug1-TestFile.png)

This means that the code will only work for files which have no extra empty lines in the end and have the last character in the file as ")".

## Step 6 - Resolving the Bug
To tackle this bug, I thought of using another while loop inside the current loop to have the code skip new line characters so that it will reach the end of the file properly.

To do this, I looked up how to check for new line characters in java and used this [website](https://www.codegrepper.com/code-examples/java/how+to+check+new+line+character+in+java) as a reference.

Here is the edit that I made to the original file to resolve the bug caused by extra empty lines at the end of the file:

![](/LabRep2Pics/CodeChange1-Fix.png)

## Step 7 - Testing the Code
To make sure that the bug has been resolved by the changes made, I compiled and then ran the program again. Below is the output from the program:

![](/LabRep2Pics/Fixed1.png)

As seen, the code now runs as expected: printing out the two URLs in the file in an array.

To make sure that the fix is applicable to other similar situations, I created a new test file with several (5) empty lines in the end.

The contents of the test file is as follows:
```
# Title

[link](https://google.com)





```

And the output of the program is:

![](/LabRep2Pics/Bug1-TestFile2.png)

This shows that the edit made to the code has indeed resolved the bug relating to empty lines in the end of the file.

## Step 8 - Cleaning Up the Code
The final step in this process of debugging is cleaning up the code by adding in-line comments and removing the print statements added for testing purposes.

![](/LabRep2Pics/Fixed1-CleanCode.png)

## *Notes on Debugging*
If the attempt to resolve the bug does not work, try the following strategies:

- Attempt to reduce the size of the failure-inducing input
- Add more watchpoints or debug print statements
- Find a tool that can bring new information to light
- Take a step back and question your assumptions
- Talk to someone new and discuss if possible
- Stare at the code until you figure it out

In the end, the essence of debugging is figuring out where the code went wrong and trying to find a solution for it.

## Bug #2
The second bug is yet another infinite loop that results in an OutOfMemoryError as the symptom. The failure-inducing input and the symptom is as follows:

![](/LabRep2Pics/Bug2.png)

To fix this bug, I realized that simply using the `while` loop to skip empty lines will not work. Thus, recognizing that `indexOf` will return `-1` if it does not find the character we asked it to search for, I added an `if` statement to end the loop if `-1` is returned for the next "[" search.

This works because there will be no more parentheses/brackets after the second link has been identified, and an `indexOf` search for "[" will return `-1`.

Below is the change I made to the code:

![](/LabRep2Pics/CodeChange2.png)

To test this change, I ran the updated `MarkdownParse.java` file again with the failure-inducing file.

![](/LabRep2Pics/Fixed2.png)

As can be seen, the symptom does not occur anymore and the change to the code successfully fixed this bug.

# Bug #3
The third bug that I identified in this lab is a bug that results in an IndexOutOfBounds exception when I try to run the code with `test-file3.md` as the input.

The failure-inducing input and the symptom of the bug is as follows:

![](/LabRep2Pics/Bug3.png)

In order to fix this error, I must account for there not being a pair of brackets after the pair of parentheses. Thus, using the same concept as the solution to bug #2, I added a few more arguments to the if statement to make sure that the loop ends when any of the `indexOf` methods returns `-1`.

![](/LabRep2Pics/CodeChange3.png)

I also recognized that no longer need the `while` loop that I wrote to fix the first bug since watching our for `-1` outputs from the searches will bypass the problem of extra empty lines at the end of the file.

To test this fix, I ran the code with the same failure-inducing file as the input:

![](/LabRep2Pics/Fixed3.png)

As seen, the program now runs without an error and outputs the correct result: an empty array because there isn't a link embedded in the file.

Just to make sure that the code still works properly for the failure-inducing inputs of the two previous bugs, I ran the code with those files as the input too:

![](/LabRep2Pics/Fixed3-TestAllBugs.png)

Now I can confirm that all three bugs addressed in this lab have been fixed.