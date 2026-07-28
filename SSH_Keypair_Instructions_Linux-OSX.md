# Linux / OSX Key-pair Generation Instructions

## Step 1: Generate a new SSH key for MarkUs

1. Open up your Terminal and copy and paste the following text below. If you wish, you can add a label for your key pair. (ex: Laptop, School Computer, etc)
    ```console
    > ssh-keygen -t rsa -C "LABEL GOES HERE"
    ```
2. Leave the default options as is and keep pressing Enter until you see:
    ```console
    Your identification has been saved in .../<yourUsername>/.ssh/id_rsa.
    # Your public key has been saved in .../<yourUsername>/.ssh/id_rsa.pub.
    ```

## Step 2: Add your key to your ssh-agent

An **ssh-agent** is a tool which keeps track of your private & public key pairs and will help authenticate you when you try to establish connections.
___

1. Start up your ssh-agent by typing this into your terminal:
    ```console
    > eval $(ssh-agent -s)
   ```
    Or, if that does not work try:
    ```console
    > ssh-agent -s
   ```
2. Now add your newly generated key to the ssh-agent by running this command:
   ```console
   > ssh-add ~/.ssh/id_rsa
   ```
   You should see something like:
   ```console
   Identity added: .../<yourUsername>/.ssh/id_rsa
   (.../<yourUsername>/.ssh/id_rsa)
   ```

## Step 3: Add your public key to MarkUs

1. Adding your public key to MarkUs is done by visiting [this page](https://markus.teach.cs.toronto.edu/markus/key_pairs/new) and clicking “**New Key Pair**”
2. Now you can choose to upload the public key file itself (located in the hidden folder in your home directory 
   ```console
   ~/.ssh/id_rsa.pub
   ```
   or by executing the following commands to copy and paste your public key contents:
   ```console
   > cat ~/.ssh/id_rsa.pub
   ```
   Then you can copy and paste your entire key into MarkUs:
   ![Copying the output to the clipboard from running the "cat" program on your public key](https://raw.githubusercontent.com/SoftwareDev/Wiki/5c0ab2bbbdb47ed2309cfad27bcb64ff725a022f/images/Key_Pair-01.png)
   ![Copy paste the results of running the "cat" program on your public key and paste it in the MarkUs "Add a Key-pair" form.](https://raw.githubusercontent.com/SoftwareDev/Wiki/5c0ab2bbbdb47ed2309cfad27bcb64ff725a022f/images/Key_Pair-02.png)
