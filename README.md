# Table of Content:

1.  [Setting Up SSH Keys for GitHub Access](#security-research-projects)
2.  

------------------------------------------------------

# Setting Up SSH Keys for GitHub Access<a name="security-research-projects"></a>  

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
First, open your terminal and run the following command to generate an SSH key pair. Replace your_email@example.com with your actual email address.  
_**ssh-keygen -t rsa -b 4096 -C "your_email@example.com"**_  

**Explanation of Flags:**  

- **t rsa:** Specifies RSA as the key type (a commonly used secure algorithm).  
- **b 4096:** Sets the key length to 4096 bits for stronger security.  
- **C "your_email@example.com":** Adds your email as a comment for identification purposes.  
You'll be prompted to choose a file name to save the key (press Enter to accept the default path). You can also set a passphrase to protect your private key, or press Enter to skip.  

2. View the Public Key  
Next, open the file that contains your public key:  

Linux/macOS: _**~/.ssh/id_rsa.pub**_  
Copy the contents of this file — this is your public key, which will be added to GitHub.  

3. Configure SSH (Optional)  
If you don’t already have a configuration file, create one:

_**touch ~/.ssh/config**_  
Edit this file (_**nano ~/.ssh/config**_) and add the following lines to configure GitHub access:  

_**Host github.com-(your github name)    
&nbsp; HostName github.com  
&nbsp; User git  
&nbsp; IdentityFile ~/.ssh/id_rsa**_      
    
Make sure the file is saved with the correct idents/spacing.      

5. Start the SSH Agent    
The SSH agent is a background program that handles your SSH keys for automatic authentication. Start it with the following command:    
_**eval "$(ssh-agent -s)"**_    
You should see the agent process ID (PID), confirming it’s running.    

5. Add Your Private Key to the SSH Agent    
For easier authentication, you can add your private key to the SSH agent:    
_**ssh-add ~/.ssh/id_rsa**_    
If successful, you'll see a confirmation message: "Identity added."    

6. Add Your Public Key to GitHub    
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
_**Hi [your_username]! You've successfully authenticated, but GitHub does not provide shell access.**_    

Bonus: Copying Public Key to Clipboard (Optional)    
To make copying your public key easier, you can send it directly to the clipboard. First, ensure xsel is installed:    
_**sudo apt-get install xsel**_    
Then, copy the public key with:    
_**xsel --clipboard < ~/.ssh/id_rsa.pub**_    
⚠️ Be careful: Ensure you're copying the .pub file (public key), not your private key!    

Conclusion:    
Congratulations! You’ve successfully set up SSH keys for secure access to GitHub. Make sure to document your process in your GitHub repository, and feel free to experiment with other configurations and multiple SSH keys for different projects.    

