---
title: Lab 1 - Remote Access
---
## Part 1 - Installing Visual Studio Code
To install Visual Studio Code, we simply need to follow these steps:

1. Visit the [Visual Studio Code](https://code.visualstudio.com/download) website and download the application for your operating system.
2. After unzipping the download file, open the Visual Studio Code application and follow the steps for setup.

After finishing setup, you should see a window similar to this (some details may be different depending on your settings):

![](/LabRep1Pics/VSC.png)

## Part 2 - Remotely Connecting
For Window users only: [Install OpenSSH](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)

To connect to the server, you need your course-specific account. Look that up [here](https://sdacs.ucsd.edu/~icc/index.php).

Then, to connect to the remote server, we are going to use Visual Studio Code's terminal. Open the terminal by navigating to Terminal -> New Terminal or an equivalent keyboard shortcut.

The terminal will look like this:

![](/LabRep1Pics/VSC-Terminal.png)

Enter the following command to initiate the connection (replace apy with letters in your course-specific account):

`ssh cs15lsp22apy@ieng6.ucsd.edu`

If it's your first time connecting, you will probably get a message along the lines of (taken from lab 1's tutorial):

```
$ ssh cs15lsp22apy@ieng6.ucsd.edu

The authenticity of host 'ieng6.ucsd.edu' (128.54.70.227) can't be estbalished.

RSA Key Fingerprint is
SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/1x99wUQcec.

Are you sure you want to continue connecting
(yes/no/[fingerprint])?
```

Enter yes to these messages to continue connecting to the remote server and enter your password whem prompted to do so.

After successfully logging in, you will see a series of messages similar to the following:

![](/LabRep1Pics/SSH-Login.png)

## Part 3 - Trying Some Commands
Some of the more common commands include `ls`, `pwd`, `cd`, `mkdir`, and `cp`. Running some of these commands will result in the following outcome:

![](/LabRep1Pics/UNIX-Commands.png)

To log out of the remote server, you can use Ctrl-D or type `exit`.

## Part 4 - Moving Files with scp
One key step in working remotely is the ability to transfer data between the *client* and the *server*.

The command to copy a file from the client to the server is called `scp` that will be run from the client.

As an example, we will transfer WhereAmI.java from our computer to the server.

First, create a new file named WhereAmI.java and input the following code:
```
class WhereAmI {
   public static void main(String[] args) {
      System.out.println(System.getProperty("os.name"));
      System.out.println(System.getProperty("user.name"));
      System.out.println(System.getProperty("user.home"));
      System.out.println(System.getProperty("user.dir"));
   }
}
```
If we run this code using `javac` and `java` on our computer, it will display the computer's operating system, user name, the home directory, and the directory the file is in.

Next, enter the following code in the terminal to copy the file from the client to the server:

`scp WhereAmI.java cs15lsp22apy@ucsd.edu:~/`

You will be prompted to enter your password just like when logging into the server. You will see a message similar to the one shown in the screenshot below:

![](/LabRep1Pics/SCP-Command.png)

Then, log in to ieng6 again and use the command `ls` to check if the Java file is there. You can also use `javac` and `java` commands to run the file.

![](/LabRep1Pics/SCP-Result.png)

## Part 5 - Setting an SSH Key
Everytime we log in or run `scp` we have to enter our password, which is time consuming. A solution to this is `ssh` keys.

(From the lab tutorial) The idea behind `ssh` keys is that a program, called `ssh-keygen`, creates a pair of files called the *public key* and *private key*. You copy the public key to a particular location on the server, and the private key in a particular location on the client. Then, the `ssh` command can use the pair of files in place of your password.

To do this, enter the following lines of commands:
```
# on client
$ ssh-keygen

Generating public/private rsa key pair.

Enter file in which to save the key (/Users/ryanchen/.ssh/id_rsa):

Enter passphrase (empty for no passphrase):

Enter same passphrase again:

Your identification has been saved in /Users/ryanchen/.ssh/id_rsa.

Your public key has been saved in /Users/ryanchen/.ssh/id_rsa.pub.

The key fingerprint is:
SHA256:1cdvT9sFJQHOrvris7dg6KTCuT75vfgQU1p9jey0egI ryanchen@Ryans-MacBook-Pro-5.local

The key's randomart image is:
+---[RSA 3072]----+
|            ..o..|
|       . . * . o |
|      o . * = +  |
|     +   = o . o |
|    + E S o .   =|
|     o o . .   o=|
| . o. o = o    .o|
|  *  B .o=.      |
| .o=+.=o+*o.     |
+----[SHA256]-----+
```

If on Windows, follow the extra `ssh-add` step [here](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement#user-key-generation).

Afterwards, we want to log in to the remote server and use the `mkdir` command to create a new directory named `.ssh`.

Back on the client, we need to use the `scp` command to copy `id_rsa.pub` to the `.ssh` directory on the server:

`scp /Users/ryanchen/.ssh/id_rsa.pub cs15lsp22apy@ieng6.ucsd.edu:~/.ssh/authorized_keys`

After this is complete, you can log in to the server without having to enter the password.

![](/LabRep1Pics/SSHKey.png)

## Part 6 - Optimizing Remote Running
You can write a command in quotes at the end of an `ssh` command to directly run it on the server, then exit. For example:

`ssh cs15lsp22apy@ieng6.ucsd.edu "ls"`

You can use semicolons to run multiple commands on the same line. For example:

`javac WhereAmI.java; java WhereAmI`

You can use the up arrow key to recall the last command that was run.

To quickly update a local edit to the server and run the file, we can use the command

`scp WhereAmI.java cs15lsp22apy@ieng6.ucsd.edu:~/; ssh cs15lsp22apy@ieng6.ucsd.edu "javac WhereAmI.java; java WhereAmI"`