# Table of Content:

1.  [Setting Up SSH Keys for GitHub Access](#setting-up-ssh)
2.  [Version Controlling using Git](#version-controlling)
3.  [Collaborating with Git – Using Branches and Pull Requests](#collaborating)
4.  [AWS EC2 Linux Instance Creation](#ec2linuxcreation)
5.  [Creating a No-Cost VPC (Basics)](#createvpc)
6.  

------------------------------------------------------
# Setting Up SSH Keys for GitHub Access<a name="setting-up-ssh"></a>  

## Objective:  
In this lab, you'll learn how to generate and configure SSH keys to securely connect to GitHub. By the end, you'll be able to use SSH for authentication with GitHub, improving both security and convenience.  

## Prerequisites:    
- A GitHub account    
- Basic familiarity with the command line    

## Tools Required:      
- Terminal or Command Prompt (This lab uses Ubuntu on WSL)    
- Optional: tmux for multitasking    

## Step-by-Step Procedure:
### 1. Generate SSH Keys  
First, open your terminal and run the following command to generate an SSH key pair. Replace your_email@example.com with your actual email address you used to make a Github account:  
     
`ssh-keygen -t rsa -b 4096 -C <your_email@example.com>`

Press _**Enter**_ 2 times
  
- [Rescource: Generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent):

**Explanation of Flags:**  

- `t rsa`: Specifies RSA as the key type (a commonly used secure algorithm).  
- `b 4096`: Sets the key length to 4096 bits for stronger security.  
- `C <your_email@example.com>`: Adds your email as a comment for identification purposes.  
You'll be prompted to choose a file name to save the key (press `Enter` to accept the default path). You can also set a passphrase to protect your private key, or press Enter to skip.

### 2. View the Public Key  
Next, open the file that contains your public key:  
  
`~/.ssh/id_rsa.pub`   

Copy the contents of this file — this is your public key, which will be added to GitHub.  

### 3. Configure SSH (Optional)  
If you don’t already have a configuration file, create one:

`touch ~/.ssh/config`  

Edit this file:   

`nano ~/.ssh/config`  

and add the following lines to configure GitHub access:  

`Host github.com-(your github name)`   
&nbsp; `HostName github.com`      
&nbsp; `User git`    
&nbsp; `IdentityFile ~/.ssh/id_rsa`    
    
*Make sure the file is saved with the correct idents/spacing.      

### 4. Start the SSH Agent    
The SSH agent is a background program that handles your SSH keys for automatic authentication. Start it with the following command:
    
`eval "$(ssh-agent -s)"` 

You should see the agent process ID (PID), confirming it’s running.    

### 5. Add Your Private Key to the SSH Agent    
For easier authentication, you can add your private key to the SSH agent:
    
`ssh-add ~/.ssh/id_rsa`   

If successful, you'll see a confirmation message: "Identity added."    

### 7. Add Your Public Key to GitHub    
Now, let's connect your SSH key to GitHub:    

- Go to your GitHub account settings.    
- Navigate to SSH and GPG keys.    
- Click New SSH key.    
- Paste the public key you copied earlier into the "Key" field.    
- Give the key a recognizable title (e.g., "My Workstation").    
- Click Add SSH key.    

### 7. Test Your Connection        
Finally, let’s test the connection between your machine and GitHub. Run the following command:
    
`ssh -T git@github.com`     

If successful, you'll get a confirmation message like:    
_"Hi [your_username]! You've successfully authenticated, but GitHub does not provide shell access."_    

Bonus: Copying Public Key to Clipboard (Optional)    
To make copying your public key easier, you can send it directly to the clipboard. First, ensure xsel is installed:     

`sudo apt-get install xsel`      

Then, copy the public key with:      

`xsel --clipboard < ~/.ssh/id_rsa.pub`     

**Be careful**: Ensure you're copying the `.pub` file (public key), not your private key!    

**Conclusion:**    
Congratulations! You’ve successfully set up SSH keys for secure access to GitHub. Make sure to document your process in your GitHub repository, and feel free to experiment with other configurations and multiple SSH keys for different projects.  

--------------------------------------------------------------------------------------------------------------------------

# Version Controlling using Git<a name="version-controlling"></a> 

## Benefits for Cloud Security Engineers

- **Version Control:** Git allows auditable and controlled management of infrastructure code and configuration files (e.g., Terraform, CloudFormation).
- **Collaboration:** GitHub supports collaboration across teams and locations, helping everyone work on the same configurations and codebase without conflicts.
- **Auditing & Accountability:** Git provides a history of changes, making it easy to review who made what changes for security and compliance.
- **Backup & Recovery:** Storing code and configurations on GitHub safeguards against local data loss, ensuring you can always access your files.

## What is Git?
Git is a distributed version control system (DVCS) that tracks changes made to files over time. It's primarily used by software developers to manage source code, but it can be applied to any set of files.

## Lab Objective     
This lab will walk you through the process of creating a local Git repository for a directory on your computer, configuring it, and pushing it to GitHub for version control. For Cloud Security Engineers, managing code and configuration files securely with Git is essential, making this process foundational for any IaC (Infrastructure as Code) workflows.  

## Scenario
Imagine you’ve set up a directory called 'cloud-bootcamp' in your Documents folder to store resources for a cloud security bootcamp. You want to version-control this directory using Git and push it to a GitHub repository for backup and collaboration.

## Prerequisites   
- A GitHub account
- Basic familiarity with the command line
- SSH keys set up with GitHub for secure access see the [Setting Up SSH Keys for GitHub Access Lab](#setting-up-ssh)

## Tools
- Terminal or Command Prompt (e.g., WSL - Ubuntu)     
- Follow along [video](https://www.youtube.com/watch?v=PWqS4NBhEY8)

## Steps

### 1. Create a Repository on GitHub
Go to GitHub and create a new repository named 'cloud-bootcamp'. Set it to public.
Select and copy SSH URL under the "<>Code>" "…or push an existing repository from the command line" section.

### 2. Set Up the Local Repository

Navigate to Documents:

`cd ~/Documents`

Create the Directory:

`mkdir cloud-bootcamp`

Move into the Directory:

`cd cloud-bootcamp`

Verify Location:

`pwd`

### 3. Configure Git Identity     
If you haven’t already, set your Git identity to match your GitHub account:

`git config --global user.email <you@example.com>`      

`git config --global user.name "Your Name"`

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

------------------------------------------------------

# Collaborating with Git – Using Branches and Pull Requests<a name="collaborating"></a> 

## Objective
This lab teaches you how to create branches, make commits, push changes to GitHub, and open a pull request for review. Understanding this workflow is essential for Cloud Security Engineers, who often collaborate on shared configurations, policies, and code.

## Why Should a Security Engineer Know This?
Security Engineers frequently collaborate on code and configuration files in version-controlled repositories. Knowing how to create branches, make commits, and manage pull requests ensures that:

**Changes are Controlled:** Working in separate branches reduces risk by isolating updates until they are ready for review and deployment.
**Auditing is Enhanced:** Every change has a clear history, making it easier to track who modified what and why.
**Team Collaboration is Streamlined:** Pull requests promote transparent communication and feedback loops, critical for a security-focused team working on sensitive configurations or code.

## Scenario
Suppose you’re continuing work on your cloud-bootcamp GitHub repository. You’ll make changes in a new branch, push those changes to GitHub, and create a pull request to merge your updates.

## Prerequisites
Basic familiarity with the command line     
An existing Git repository on your local machine (e.g., cloud-bootcamp). *Follow along with the previouos labs above. 

## Steps
### 1. Make Changes to a Tracked Directory
Navigate to the Project Directory:     

_**cd ~/Documents/cloud-bootcamp**_     

Edit the README.md File:     

_**nano README.md**_     

Add a comment such as "Updated content for Git workflow tutorial." Then save and exit.

Check Status:

_**git status**_     

The README file will appear as modified (in red), indicating unstaged changes.     

### 2. Stage and Commit Changes     
Stage Changes: Stage all files with:     

_**git add .**_     

*It prepares _all_ changed files to be included in the next version of your project.     

Commit the Changes:     

_**git commit -m "Updated README for Git workflow tutorial"**_     

*This command creates a new commit in your local repository, capturing the staged changes. The -m "message" argument is required to provide a descriptive commit message for tracking project history.     

### 3. Create and Switch to a New Branch
Create a Branch: Create a new branch for your changes. Let’s name it feature/git-tutorial:

_**git branch feature/git-tutorial**_     

Switch to the New Branch:

_**git checkout feature/git-tutorial**_     

Confirm Branch Change:

_**git branch**_     

The new branch (feature/git-tutorial) will be highlighted, indicating you’re actively working in this branch.

### 4. Make Further Edits in the New Branch
Edit the README.md File Again:

_**nano README.md**_     

Add a line like "This change was made in the feature/git-tutorial branch."

Stage and Commit the New Changes:

_**git add README.md**_     

_**git commit -m "Additional update from feature/git-tutorial branch"**_     

### 5. Push Changes to GitHub
Push the New Branch: Push the changes from feature/git-tutorial to GitHub:

_**git push --set-upstream origin feature/git-tutorial**_     

After running _git push --set-upstream origin tutorial/git_, your local changes will be visible as a new branch named tutorial/git on the GitHub repository. This command also sets up a tracking relationship between your local and remote branches, simplifying future pushes.

Step-by-Step Breakdown of the output of this command:     

**Counting and Compression:** Git first counts the files and metadata (objects) to be pushed. Then, it uses efficient compression to minimize data transfer by only sending changes between your local commits and the remote branch.     

**Uploading Changes:** Compressed objects containing your changes are uploaded to the remote repository (e.g., GitHub). The output shows details like the total number of objects pushed and confirms that none were reused.     

**Remote Message and Branch Setup:**     
A message from the remote repository (likely GitHub) might suggest creating a pull request to integrate your changes.
The destination repository confirmation (e.g., "github.com:_accountname_/cloud-bootcamp.git") is displayed.
The output indicates a successful push:
[new branch] signifies a new branch named "tutorial/git" created on the remote repository.
tutorial/git -> tutorial/git shows your local branch now tracks the remote branch.
Finally, confirmation is provided that the --set-upstream flag established a tracking relationship between your local and remote branches.

### 6. Open a Pull Request on GitHub
Navigate to the Repository on GitHub: Go to your 'cloud-bootcamp' repository on GitHub, where you should see an option to compare and create a pull request for the new branch.

Open the Pull Request:

- Click "Compare & pull request".
- Review the changes in the pull request view to confirm the modifications.
- Click "Create pull request" and add a title and description.
- Submit the Pull Request: After review, the pull request can be merged.

### 7. Merge the Pull Request
- Merge and Confirm: Select "Merge pull request" and then "Confirm merge".
- Verify the Merge: The updates should now be in the main branch on GitHub.

### 8. Pull Changes to Your Local Main Branch
Switch Back to Main Branch Locally:

_**git checkout main**_     

Pull Updates: Synchronize the main branch with GitHub:

_**git pull**_
Verify the Changes Locally:

_**nano README.md**_     

Confirm that the latest updates from feature/git-tutorial are now in your local main branch.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------

# AWS EC2 Linux Instance Creation<a name="ec2linuxcreation"></a>  

## Objective
As a Cloud Security Engineer, I want to make setting up an EC2 Linux instance in AWS as straightforward as possible while keeping security in mind. These notes are all about breaking things down and focusing on the essentials.

## Step-by-Step Procedure
1. Log in to Your AWS Account Console

Always log in as an IAM user, not the root account, for security purposes.

2. Select Your Region

On the AWS Management Console, choose the region closest to you from the dropdown menu in the top-right corner. 

1. Bookmark Commonly Used Services

For future convenience, search for these common services in the search bar and click the star icon on the right to add them to your toolbar:

IAM

VPC

EC2

Lambda

S3

RDS

4. Navigate to EC2

Search for EC2 in the search bar or select it from your bookmarks.

5. Launch an EC2 Instance

Click on Launch Instance.

Name your instance something simple, such as linux_ec2, to keep it organized.

Select Amazon Linux as your Amazon Machine Image (AMI).

Choose t2.micro (free tier eligible) as the instance type.

Under the Key pair (login) section, select Proceed without a key pair for now.  
(Note: In production, always configure a key pair for secure access.)

6. Launch the Instance

Click on Launch Instance.

Navigate to the Instances section in the left-hand menu to monitor your instance’s status.

7. Review the Instance State

**Pending:** The instance is being created and provisioned.   
**Running:** The instance is active and ready to use.

8. Shutting down EC2

Go back to the AWS online instance dashboard

Select the instance > instance state > terminate instance 

This will completely stop and delete the instance. This will help prevent getting charged. 

## Conclusion
Security should always be top of mind when working with AWS EC2 instances. While this guide covers the basics, here are a few things to keep in mind:
Avoid using root accounts for day-to-day tasks.
Use key pairs for SSH access, especially in production environments.
Regularly review and follow AWS security best practices.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Creating a No-Cost VPC (Basics)<a name="createvpc"></a>  

## Objective
This guide walks you through creating a basic VPC at no cost, helping you understand AWS networking fundamentals using the AWS Management Console.

## Prerequisites
- An AWS account
- Basic knowledge of VPC concepts (CIDR blocks, subnets, availability zones)

## Tools Required
- AWS Online Console (No CLI or Terraform for this demo)

## Step-by-Step Procedure

### 1. Access the AWS Console & Navigate to VPC
- Sign in to your AWS account.
- Go to the VPC Service Console (Search for "VPC" in the AWS search bar).
- If this is your first time, you will see a default VPC already created. Every AWS account comes with one by default.
- Review the VPC dashboard (left-hand panel) to get an overview of the VPC-related resources in your selected AWS region.
- Click “Create VPC” to start building a new one based on our "[Building a Secure and Scalable AWS VPC: A Thought Process Guide](https://github.com/Sylkpac/Security/blob/main/README.md#buildingvpc)" project.

### 2. Configure the VPC Settings
- Select "VPC and More" – this option lets AWS handle much of the configuration for you.
- **CIDR Block:** Leave as 10.0.0.0/16 (AWS defaults to this range, which provides 65,536 IP addresses).
  - **Why?** This range is large enough for a variety of networking setups while maintaining flexibility for subnetting.
- **Availability Zones:** Leave at 2 (this ensures redundancy).
  - **Why?** Using multiple Availability Zones (AZs) protects against failures in a single data center.
- **Subnets:**
  - **Public Subnets:** Keep at 2 (one in each AZ).
  - **Private Subnets:** Increase to 4 (two per AZ).
    - **Why?** Public subnets host resources needing internet access, while private subnets keep critical services isolated.
- **NAT Gateway:** You will see a dollar symbol next to this option. NAT Gateways incur charges, so select "None" at this time to avoid costs.
- **Other Settings:** Leave them as default.
- Click "Create VPC."

### 3. Verify Your VPC
- Once created, click "View VPC" to see your newly created VPC.
- On the left-hand panel, select “Your VPCs.”
- You should now see two VPCs:
  - The default VPC (pre-created by AWS).
  - The one you just created.

### 4. Inspect Your Subnets
- On the left-hand panel, click "Subnets."
- Click the refresh button (top right, next to “Actions”) to update the list.
- You should now see a mix of:
  - Subnets from the default AWS VPC
  - Subnets from the VPC you created
- **Filter by your VPC:**
  - On the left panel, select "Filter by VPC."
  - Choose your newly created VPC to see only its subnets.

### Key Takeaways
- This hands-on exercise is a visual guide to understanding VPC creation in the AWS Console.
- In the real world, VPCs should not be provisioned manually via the AWS Console.
- Infrastructure as Code (IaC) tools like Terraform or AWS CloudFormation should be used for scalability and automation.
- **Why did we create this manually then?**
  - To get a visual understanding of AWS networking components.
  - To see how subnets and routing are structured before moving to automation tools.
