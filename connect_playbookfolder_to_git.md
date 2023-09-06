Install git 
    sudo apt update 
    sudo apt install git 
    which git 

Create a new repository on your github account: "cumulus_automation"

Under profile, settings, ssh and gpg keys. Add ssh key you created
Go to the repository and under code button, select  "code - clone with ssh" and 

    git clone "url taken with command above"
    git config --global user.name "ercin torun"
    git config --global user.email "ercintorun@gmail.com"
    cat ?/.gitconfig

After doing your updates: 

    git status 
    git diff
    git add Readme.md 
    git commit -m "your comment if you would like to add"
    git push origin master
