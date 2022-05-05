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