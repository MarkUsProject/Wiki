# Linux / MacOS instructions
## Step 1: Generate a new SSH key for MarkUs

1. Open up your Terminal and copy and paste the following text below. If you wish, you can add a label for your key pair. (ex: Laptop, School Computer, etc)  
`> ssh-keygen -t rsa -C "LABEL GOES HERE"`  
2. Leave the default options as is and keep pressing Enter until you see:  
`Your identification has been saved in .../<yourUsername>/.ssh/id_rsa.`  
`# Your public key has been saved in .../<yourUsername>/.ssh/id_rsa.pub.`  

## Step 2: Add your key to your ssh-agent
An **ssh-agent** is a tool which keeps track of your private & public key pairs and will help authenticate you when you try to establish connections.
___
1. Start up your ssh-agent by typing this into your terminal:  
`> eval $(ssh-agent -s)`  
Or, if that does not work try:  
`> ssh-agent -s`  
2. Now add your newly generated key to the ssh-agent by running this command:  
`> ssh-add ~/.ssh/id_rsa`  
You should see something like:  
`Identity added: .../<yourUsername>/.ssh/id_rsa`  
`(.../<yourUsername>/.ssh/id_rsa)`  

## Step 3: Add your public key to MarkUs
1. Adding your public key to MarkUs is done by visiting this page and clicking “**New Key Pair**”  
2. Now you can choose to upload the public key file itself (located in the hidden folder in your home directory `~/.ssh/id_rsa.pub` or by executing the following commands to copy and paste your public key contents:  
`> cat ~/.ssh/id_rsa.pub`  
Then you can copy and paste your entire key into MarkUs:  
![Copying the output to the clipboard from running the "cat" program on your public key](http://alexgrenier.ca/MarkUs/1.png)  
![Copy paste the results of running the "cat" program on your public key and paste it in the MarkUs "Add a Key-pair" form.](http://alexgrenier.ca/MarkUs/2.png)


***

# Windows instructions
## Step 1: Install Git

1. Download and install Git on your machine. http://git-scm.com/downloads  
2. Open **Git bash** by clicking the start menu and typing in “git bash”.  
  
**Windows 7**  
 
![Search for "git bash" when you open your Start Menu in Windows 7](http://alexgrenier.ca/MarkUs/3.png)

**Windows 8**  
  
![After pressing the Windows Key or clicking the Start button start typing "Git bash" to search for the program](http://alexgrenier.ca/MarkUs/4.png)  
  
## Step 2: Generate a new SSH key for MarkUs
1. Within the Git bash copy and paste the following text below. If you wish, you can add a label for your key pair. (ex: Laptop, School Computer, etc)  
`> ssh-keygen -t rsa -C "LABEL GOES HERE"`  
2. Leave the default options as is and keep pressing **Enter** until you see:  
`Your identification has been saved in .../<yourUsername>/.ssh/id_rsa.`  
`# Your public key has been saved in .../<yourUsername>/.ssh/id_rsa.pub.`  

## Step 3: Add your key to your ssh-agent  

An **ssh-agent** is a tool which keeps track of your private / public key pairs and will help authenticate you when you try to establish connections.  
___
1. Now add your newly generated key to the ssh-agent by running this command:
`> ssh-add ~/.ssh/id_rsa`  
You should see something like:  
`Identity added: .../<yourUsername>/.ssh/id_rsa`  
`(.../<yourUsername>/.ssh/id_rsa)`  
_**From now on, continue to use the Git bash for your Git commands on your Windows machine.**_

## Step 4: Add your public key to MarkUs  

1. Adding your public key to MarkUs is done by visiting this page and clicking “**New Key Pair**”  
2. Now you can choose to upload the public key file itself (located in the hidden folder in your home directory `~/.ssh/id_rsa.pub` or by executing the following commands from within your **Git bash** to copy and paste your public key contents:  
`notepad ~/.ssh/id_rsa.pub`  
Notepad will then open up and you can then copy the public key contents and paste it into Markus:
![Copy your public key contents.](http://alexgrenier.ca/MarkUs/5.png)  
![Paste your public key contents into MarkUs.](http://alexgrenier.ca/MarkUs/6.png)


