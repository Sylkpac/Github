# Table of Content:

1.  [Setting Up SSH Keys for GitHub Access](#setting-up-ssh)
2.  [Version Controlling using Git](#version-controlling)

------------------------------------------------------

# Setting Up SSH Keys for GitHub Access<a name="setting-up-ssh"></a>  

### Objective:  
In this lab, you'll learn how to generate and configure SSH keys to securely connect to GitHub. By the end, you'll be able to use SSH for authentication with GitHub, improving both security and convenience.  

### Prerequisites:    
A GitHub account    
Basic familiarity with the command line    

### Tools Required:      
Terminal or Command Prompt (This lab uses Ubuntu on WSL)    
Optional: tmux for multitasking    

### Step-by-Step Procedure:
1. Generate SSH Keys  
First, open your terminal and run the following command to generate an SSH key pair. Replace your_email@example.com with your actual email address you used to make a Github account:  
     
_**ssh-keygen -t rsa -b 4096 -C "your_email@example.com"**_      

Press _**Enter**_ 2 times
  
*Resource: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent 

**Explanation of Flags:**  

- **t rsa:** Specifies RSA as the key type (a commonly used secure algorithm).  
- **b 4096:** Sets the key length to 4096 bits for stronger security.  
- **C "your_email@example.com":** Adds your email as a comment for identification purposes.  
You'll be prompted to choose a file name to save the key (press Enter to accept the default path). You can also set a passphrase to protect your private key, or press Enter to skip.

2. View the Public Key  
Next, open the file that contains your public key:  
  
_**~/.ssh/id_rsa.pub**_   

Copy the contents of this file — this is your public key, which will be added to GitHub.  

3. Configure SSH (Optional)  
If you don’t already have a configuration file, create one:

_**touch ~/.ssh/config**_  

Edit this file:   

_**nano ~/.ssh/config**_  

and add the following lines to configure GitHub access:  

_**Host github.com-(your github name)    
&nbsp; HostName github.com  
&nbsp; User git  
&nbsp; IdentityFile ~/.ssh/id_rsa**_      
    
*Make sure the file is saved with the correct idents/spacing.      

5. Start the SSH Agent    
The SSH agent is a background program that handles your SSH keys for automatic authentication. Start it with the following command:
    
_**eval "$(ssh-agent -s)"**_   

You should see the agent process ID (PID), confirming it’s running.    

5. Add Your Private Key to the SSH Agent    
For easier authentication, you can add your private key to the SSH agent:
    
_**ssh-add ~/.ssh/id_rsa**_     

If successful, you'll see a confirmation message: "Identity added."    

7. Add Your Public Key to GitHub    
Now, let's connect your SSH key to GitHub:    

Go to your GitHub account settings.    
Navigate to SSH and GPG keys.    
Click New SSH key.    
Paste the public key you copied earlier into the "Key" field.    
Give the key a recognizable title (e.g., "My Workstation").    
Click Add SSH key.    

7. Test Your Connection        
Finally, let’s test the connection between your machine and GitHub. Run the following command:
    
_**ssh -T git@github.com**_      

If successful, you'll get a confirmation message like:    
_"Hi [your_username]! You've successfully authenticated, but GitHub does not provide shell access."_    

Bonus: Copying Public Key to Clipboard (Optional)    
To make copying your public key easier, you can send it directly to the clipboard. First, ensure xsel is installed:     

_**sudo apt-get install xsel**_      

Then, copy the public key with:      

_**xsel --clipboard < ~/.ssh/id_rsa.pub**_      

⚠️ Be careful: Ensure you're copying the .pub file (public key), not your private key!    

**Conclusion:**    
Congratulations! You’ve successfully set up SSH keys for secure access to GitHub. Make sure to document your process in your GitHub repository, and feel free to experiment with other configurations and multiple SSH keys for different projects.  

------------------------------------------------------

# Version Controlling using Git<a name="version-controlling"></a> 

## Objective     
This lab will walk you through the process of creating a local Git repository for a directory on your computer, configuring it, and pushing it to GitHub for version control. For Cloud Security Engineers, managing code and configuration files securely with Git is essential, making this process foundational for any IaC (Infrastructure as Code) workflows.  

## What is Git?
Git is a distributed version control system (DVCS) that tracks changes made to files over time. It's primarily used by software developers to manage source code, but it can be applied to any set of files.

## Scenario
Imagine you’ve set up a directory called 'cloud-bootcamp' in your Documents folder to store resources for a cloud security bootcamp. You want to version-control this directory using Git and push it to a GitHub repository for backup and collaboration.

## Prerequisites   
- A GitHub account
- Basic familiarity with the command line
- SSH keys set up with GitHub for secure access see the [Setting Up SSH Keys for GitHub Access Lab](#setting-up-ssh)

## Tools
Terminal or Command Prompt (e.g., WSL - Ubuntu)

## Steps

### 1. Create a Repository on GitHub
Go to GitHub and create a new repository named 'cloud-bootcamp'. Set it to public.
Select SSH under the setup options, and copy the SSH URL provided on the "…or push an existing repository from the command line" section.

### 2. Set Up the Local Repository

Navigate to Documents:

_**cd ~/Documents**_

Create the Directory:

_**mkdir cloud-bootcamp**_

Move into the Directory:

_**cd cloud-bootcamp**_

Verify Location:

_**pwd**_

### 3. Configure Git Identity     
If you haven’t already, set your Git identity to match your GitHub account:

_**git config --global user.email "you@example.com"**_      

_**git config --global user.name "Your Name"**_

This allows Git to associate your commits with your GitHub account.

### 4. Create a Basic README File
Use the echo command to create a README file:

_**echo "# cloud-bootcamp" >> README.md**_

Verify the contents:

_**cat README.md**_

### 5. Initialize Git and Stage Changes

Initialize Git: Set up Git in this directory to enable version control.

_**git init**_

Stage Changes: Use git add to stage the README file:

_**git add README.md**_

Commit Changes: Commit the staged changes with a message:

_**git commit -m "Initial commit"**_

*The -m switch is for message 

### 6. Link to the GitHub Repository (Remote)
Add the Remote Repository: Connect your local repository to the GitHub remote repository URL you copied:

_**git remote add origin <GitHub_SSH_URL>**_

Replace <GitHub_SSH_URL> with the SSH URL of your GitHub repository.

### 7. Push to GitHub
Push your local commits to the remote repository:

_**git push -u origin main**_

*the -u switch is to set the upstream branch. The upstream branch is a remote branch that your local branch is tracking.

After running this command, you should see your files on the GitHub repository page.

### 8. Update Files and Push Again
Edit the README File: Open the README file for editing, e.g., with nano:

_**nano README.md**_

Add a line in the file "Updated file with additional info." just so there is a change in the file. 

Check the Git Status:

_**git status**_

*_Red:_ A file listed in red indicates that it is untracked. This means the file is new and hasn't been added to the Git repository yet.     
_Green:_ A file listed in green indicates that it is staged. This means the file is ready to be committed to the repository.

Stage and Commit Changes:

_**git add README.md**_          

_**git commit -m "Updated README with additional info"**_

Push Changes to GitHub:

_**git push**_

Refresh your GitHub repository page to see the updated README.

## Benefits for Cloud Security Engineers

Version Control: Git allows auditable and controlled management of infrastructure code and configuration files (e.g., Terraform, CloudFormation).
Collaboration: GitHub supports collaboration across teams and locations, helping everyone work on the same configurations and codebase without conflicts.
Auditing & Accountability: Git provides a history of changes, making it easy to review who made what changes for security and compliance.
Backup & Recovery: Storing code and configurations on GitHub safeguards against local data loss, ensuring you can always access your files.
By following these steps, you've learned the essential workflow for version controlling files on GitHub, a critical skill in cloud security.

