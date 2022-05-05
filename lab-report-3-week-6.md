# Lab 5 - SSH Configurations & Working with Github on the Server
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

## Part 2 - Setup Github Access from ieng6


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