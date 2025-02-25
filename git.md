#Git Hub Lab
##Prerequisites:
- make sure you have Visual Studios installed
- Make sure you have Git installed https://gitforwindows.org/

##Configure your git  
1. Open terminal window in Visual Studio. Change the terminal to Git Bash
2. Check what version of git `git --version`
3. Configure your git with your name and email
```

git config --global user.name “agondry”
git config --global user.email “agondry@costco.com”
```

##Create a SSH to connect to Git
1. Enter the two commands below
- This will create a SSH on your laptop. Replace the email with your email
  ```
  
  ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
- Output the key you just generated
  cat /u//.ssh/id_rsa.pub`

2. Navigate to [Azure Devops](https://dev.azure.com/costcocloudops/LTL-PlatformAndOperationsTeam) 
Select the gear icon and select SSH public key
Selet New Key. Enter a name ex. ag_windows_laptop. Paste the public key that you created.



##Create a branch and pull exsisting repo
1. Create a new branch. In Azure Devops select Repos > Files. Make sure that you are in the 
`ltlf-training-and-cohort` Repo. Drop down where it says main. Select new branch. Name it ex. ag_lab 
2. Select clone in the top right. Choose SSH and copy comand line
3. In Visual Studio open your terminal window clone the repo `ltlf-training-and-cohort` 
  ```
  
  git clone git@ssh.dev.azure.com:v3/costcocloudops/LTL-PlatformAndOperationsTeam/ltlf-training-and-cohort 
  ```

##Edit your branch
1. Add a file under git-lab. Name it initials_gitlab.md. In that file copy text below.
```

I just made my first comit!
```
2. Stage your commit
```

git add ag_gitlab.md
```
3. Check the status of your branch
```

git status
```
4. Commit your change
```

git commit -m "Adding ag_gitlab.md"
```
5. Push your change
```

git push ag_lab
```
Go to the Repo and see the changes you made to your branch

##Edit your branch using VS Code built in featurs
1. Edit your file initials_gitlab.md add todays date at the top
2. To stage your commit go to source control on the left hand side. Select the + next to the file you would like to change. 
3. To Commit your change enter a message and press commit
4. Push your change select sync. If you do not see the sync button, you can select the sync button next to your branch. 
