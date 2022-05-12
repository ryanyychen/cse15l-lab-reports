---
title: Lab 5 - SSH Configurations & Working with Github on the Server
---
## Part 1 - Streamlining SSH Configuration
Since having to type `ssh cs15lsp22apy@ieng6.ucsd.edu` is very annoying when we have to constantly log on and off the server, we are going to set up a SSH config file to make it easier for us to log onto the server.

First, we need to navigate to the `.ssh` directory by using the command `cd ~/.ssh`.

Then, in the `.ssh` directory, create a file named *config* using `touch config`.

![](/LabRep3Pics/CreateConfig.png)

Then, to edit the config file, I used the command `open .` to open the directory in Finder on my Mac. Then, from the Finder, I simply opened the config file using TextEdit and appended the following lines:

```
Host ieng6
	HostName ieng6.ucsd.edu
	User cs15lsp22apy
	IdentityFile ~/.ssh/id_rsa
```

Now that the shortcut is set up, we can test by entering `ssh ieng6` in the terminal.

![](/LabRep3Pics/StreamlineSSH.png)

As seen, we are now able to log onto the server by simply typing `ieng6` instead of the full username and server address.

We can also use this alias to `scp` to ieng6:

![](/LabRep3Pics/StreamLineSCP.png)

## Part 2 - Setup Github Access from ieng6
When trying to commit and push from the terminal, the following error occurs:

![](/LabRep3Pics/GitPushError.png)

As explained by [this article](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/), we can no longer use a password for this operation and instead need to use a token-based login mechanism (like SSH keys).

To do so, I followed the instructions on [this GitHub Docs website](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

![](/LabRep3Pics/GitHubSSHKeyGen.png)

Note that I entered a file in which to save the key so as to rename the filename from *id_ed25519* to *id_rsa_github*.

These keys are stored in the `.ssh` folder accessed from the root directory. The public file has a `.pub` extension while the private does not:

![](/LabRep3Pics/KeysStorage.png)

Then, I went into the SSH config file and added these lines:

```
Host github
	HostName github.com
	User ryanyychen
	IdentityFile ~/.ssh/ide_rsa_github
```

After adding this new key using `ssh-add ~/.ssh/id_rsa_github`, I then followed the instructions on [this other GitHub webpage](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to add my SSH key to my GitHub account.

I started by copying the contents of the new SSH key to my clipboard using `pbcopy`. Then, I went on the GitHub website and went to the setting of my account. I then navigated to *SSH and GPG Keys* under the *Access* section:

![](/LabRep3Pics/GitHubAccountSettings.png)

Then, I clicked on the *New SSH Key* button, titled it "Personal MacBook" and pasted my SSH key in the "Key" section.

![](/LabRep3Pics/GitHubAddSSHKey.png)

Before I can push to GitHub, however, I needed to change the origin remote to point at the SSH URL using the following command:

`git remote set-url origin git@github.com:<username>/<project>.git`

Lastly, I verified that I now can commit and push changes to GitHub:

![](/LabRep3Pics/GitHubPushMain.png)

I then also did the same process again on the ieng6 server so that I can push changes to GitHub from the server.

## Part 3 - Copy Whole Directories with `scp -r`
Copying one file at a time to the server is very slow and annoying, so being able to copy whole directories at a times will be very useful. To do so, we can use the `-r` extension to `scp` to tell `scp` to copy recursively, which means that it will go through every file and directory in the specified directory and copy them all to the server.

In this lab, I practiced this by copying my *markdown-parser* repository to the server.

![](/LabRep3Pics/ServerContentBefore.png)

As can be seen, the repository does not exist on the server yet. To copy the repository over, I first navigated to the *markdown-parser* repository then used the command

`scp -r . ieng6:~/markdown-parser`.

This outputs the following lines to the terminal (I only included the first few lines as it is too long):

![](/LabRep3Pics/SCPOutput.png)

To verify that the repository has indeed been copied over, I logged onto the ieng6 server and listed all files and directories:

![](/LabRep3Pics/ServerContentAfter.png)

I can now compile and run the test file on the ieng6 server like so:

![](/LabRep3Pics/CompileAndRunOnServer.png)

I can also combine `scp -r` with semicolons and `ssh` to copy the directory and run tests in one (albeit long) line:

```
scp -r ./markdown-parser ieng6:~/; ssh ieng6 "cd markdown-parser; /software/CSE/oracle-java-17/jdk-17.0.1/bin/javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar MarkdownParseTest.java; /software/CSE/oracle-java-17/jdk-17.0.1/bin/java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest"
```

![](/LabRep3Pics/SCPAndRun.png)

*I only showed the end of the output since the scp output is way too long.