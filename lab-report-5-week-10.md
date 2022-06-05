---
title: Lab 9 - Comparing MarkdownParse Implementations
---
Implementations of MarkdownParse being looked at:

[ryanyychen/markdown-parser (Mine)](https://github.com/ryanyychen/markdown-parser)

[nidhidhamnani/markdown-parser (Given)](https://github.com/nidhidhamnani/markdown-parser)

# Part 1 - Running Tests
A new folder with test-files has been provided in the given repository, so I used that folder to run tests on both implementations. A bash file has also been provided to run MarkdownParse on each test file. Thus, I ran the tests on the given implementation using:

```
bash script.sh >> result.txt
```

This stored the results of running `script.sh` in `result.txt` for later use. I then copied the `test-files` folder and the bash script to my own implementation's repository and ran the same tests, again storing the results in `result.txt`.

# Part 2 - Determining Differences
To determine the differences in outputs from the two implementations, I ran `vimdiff` on the two `result.txt` files using the following command:
```
vimdiff result.txt ~/my-markdown-parser/result.txt
```

Here is a screenshot of some of the differences (highlighted in red), where the left side is the output of the given implementation and the right side is the output of my implementation:
![](/LabRep5Pics/Vimdiff.png)

For this lab report, I am going to focus on the two following test-files in the test-files folder:

[test-files/194.md](https://github.com/nidhidhamnani/markdown-parser/blob/main/test-files/194.md)

[test-files/201.md](https://github.com/nidhidhamnani/markdown-parser/blob/main/test-files/201.md)

# Part 3 - Examining Test-Files and Identifying Bugs
To determine the correct output of the two selected test-files, I simply used Visual Studio Code's built-in preview function.

For test-file 194:
![](/LabRep5Pics/194ExpectedOutput.png)

The expected output for test-file 194 is one link: [my_url] that is labeled "title (with parens)".

For this file, both implementations fail to give the correct output.

For test-file 201:
![](/LabRep5Pics/201ExpectedOutput.png)

The expected output for test-file 201 is nothing ([]).

For this test file, my implementation gives the correct output while the given implementation does not. However, my implementation's success is simply lacking a check for this way of link embedding, as described below.

Looking at the contents of file 194, the bug is in another way to embed links:

```
[<embed text>]:<url>
[<same embed text>]
```

On the other hand, for test-file 201, no link is embedded because there is are these symbols inside: `<>`, which apparently prevents the link from being embedded.
```
[foo]: <bar>(baz)

[foo]
```

So there are two bugs: one a failure to check for this different way of link embedding in my implementations, and one a failure to identify `<>` in the given implementation.

# Part 4 - Identifying Bugs in the Code
For the incorrect output in the given implementation for failing to check for `<>`, I ran the debugger built into Visual Studio Code to find out which part of the code is identifying `baz` as a link.

![](/LabRep5Pics/201Debugger.png)

I found out that the code does not check for spaces or other characters between the closing bracket and the opening parenthesis, hence why it ignored `<bar>` and mistook `(baz)` as the link embedding part and then adding `baz` to the return array. A possible fix would be to add in a check for spaces or other characters between the bracket and the parenthesis and then also adding a check for `<>`. Further, I also found out that this implementation does not check for this way of link embedding, hence a way to identify this way of link embedding should be added, then the check for `<>` on top of that.

For my implementation, I ran the debugger using `194.md` as the test file:

![](/LabRep5Pics/194Debugger.png)

After carefully stepping through the program, I found out that my implementation was not able to return a link because it always checked if the opening parenthesis is right behind the closing bracket. However, with this way of link embedding, a colon follows the closing bracket instead, which is why I need to implement a second check after the first to see if this form of embedding is being used in order to fix the bug.