---
title: Lab 7 - Reviewing MarkdownParse
---
The implementations of MarkdownParse that I will be looking at in this lab report are from the two following repositories:

[ryanyychen/markdown-parser (Mine)](https://github.com/ryanyychen/markdown-parser)

[ezh247467/markdown-parser (Reviewed)](https://github.com/ezh247467/markdown-parser)

# Part 1 - Test Cases
Three test files will be used to review these markdownParse implementations:

### Snippet 1:
```
`[a link`](url.com)

[another link](`google.com)`

[`cod[e`](google.com)

[`code]`](ucsd.edu)
```

Markdown preview:

![](/LabRep4Pics/Testfile1Preview.png)

Expected output based on preview:
```
['google.com, google.com, ucsd.edu]
```

### Snippet 2:
```
[a [nested link](a.com)](b.com)

[a nested parenthesized url](a.com(()))

[some escaped \[ brackets \]](example.com)
```

Markdown preview:

![](/LabRep4Pics/TestFile2Preview.png)

Expected output based on preview:
```
[a.com, a.com(()), example.com]
```

### Snippet 3:
```
[this title text is really long and takes up more than
one line

and has some line breaks](
      https://www.twitter.com
)

[this title text is really long and takes up more than
one line](
https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule
)

[this link doesn't have a closing parenthesis](github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a
while](https://cse.ucsd.edu/

)

And then there's more text
```

Markdown preview:

![](/LabRep4Pics/TestFile3Preview.png)

Expected output based on preview:
```
[https://sites.google.com/eng.ucsd.edu/cse-15l-spring-2022/schedule]
```
*The Twitter link is not embedded, hence it should not be output

# Adding Test Cases
To test the implementations, we're going to add test cases to the `MarkdownParseTest.java` file in both repositories.

For my repository:

![](/LabRep4Pics/Rep1TestCases.png)

For reviewed repository:

![](/LabRep4Pics/Rep2TestCases.png)

After adding these test cases, the results of these tests are as follows.

All three tests failed for my implementation of MarkdownParse.

![](/LabRep4Pics/Rep1TestOutput.png)

Similarly, all three tests failed for the reviewed repository.

![](/LabRep4Pics/Rep2TestOutput.png)

# Possible Fixes
### Snippet 1
For snippet 1, the difficulty lies in determing comments within the code, especially when a bracket or parenthesis is involved. I think that a potential fix that could be short would be to pair backticks and compare their indices with those of the open/close brackets/parentheses to determine if the bracket/parenthesis is within a comment and therefore treat it as just another character used in text. 

### Snippet 2
For snippet 2, the problem is nested brackets and parentheses. I think that a potential quick fix would be to implement a stack where a 'unit' is identified and the link added to the toReturn list as soon as a pair of brackets and parentheses is found. This is so that nested links can be dealt with properly, starting from the inner-most pair and working outwards. Also, a function should be implemented to find the last closing bracket/parenthesis for a line of code so that the correct end to the embeded link is identified.

### Snippet 3
This snippet is actually pretty easy to deal with. From the preview in VSC, I think that the maximum allowed number of `\n` characters for a link is one before and one after, and if any more is added, the link will not be embedded. This means that trim should be used after a check for multiple `\n` characters within the pair of parentheses so as to identify which ones will actually be embedded. There also needs to be some sort of check for missing close parentheses so that the code does not return a whole chunk of text. 