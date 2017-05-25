# Setup PS console
## Installed required components
We need install next components:
* Chocolatey - package manager
* Chocolatey packages (can be installed from vendor sites as well)
** git
** ConEmu
** vim-x86
* PowerShell Modules
** posh-git
** PSReadLine

Open elevated PowerShell Console:
```PowerShell
# Set your PowerShell execution policy
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Force

# Install Chocolatey
iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex

# Install Chocolatey packages
choco install git.install -y
choco install conemu -y
choco install vim-x86 -y
# Install PowerShell Modules
Install-Module -Name Posh-Git
Install-Module -Name PSReadLine
```
## ConEmu
Set the following settings:
* Main -> Appearance -> Single instance mode
* Main -> Quake style -> Quake style slide down 
* StartUp -> Specified named task -> {Shells:PowerShell}
* Set custom color schema from https://github.com/joonro/ConEmu-Color-Themes
** Simple add in the apropriate place content of Dracula.xml (from above repo) into C:\Program Files\ConEmu\ConEmu.xml and then choose Dracula schema in Features -> Colors -> Schemas -> Dracula

##PowerShell profile
Use profile.ps1 from this repo

##Git
Git configuration:
* Adding git binaries into PATH
```PowerShell
# Permanently add C:\Program Files\Git\bin to machine Path variable
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Program Files\Git\bin", "Machine")

# Permanently add C:\Program Files\Git\usr\bin to machine Path variable
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Program Files\Git\usr\bin", "Machine")
```

* Create ssh key for GitHub\Bitbucket 
```PowerShell
# Generate the key and put into the your user profile .ssh directory
ssh-keygen -t rsa -b 4096 -C "your@email.com" -f $env:USERPROFILE\.ssh\id_rsa
```

* Add public key to GitHub\Bitbucket
```PowerShell
# Copy the public key. Be sure to copy the .pub for the public key
Get-Content $env:USERPROFILE\.ssh\id_rsa.pub | Set-Clipboard
```

* Configure ssh-agent
```PowerShell
# Add ssh key
Add-SshKey $env:USERPROFILE\.ssh\id_rsa

# List ssh keys
ssh-add -l

# Test ssh connection to GitHub
ssh -T git@github.com
```
* Configure global git settings
```
git config --global user.email "your@email.com"
git config --global user.name "Your Name"
git config --global push.default simple
git config --global core.ignorecase false

# Configure line endings for windows
git config --global core.autocrlf true

# Configure git lol (alias for better output for git log)
git config --global --add alias.lol "log --graph --decorate --pretty=oneline --abbrev-commit --all"
```
## VIM
Add PowerShell syntax
PowerShell syntax for VIM and plugign manangement taken from the next repos (detailed instructions available there):
https://github.com/tpope/vim-pathogen
https://github.com/PProvost/vim-ps1

```PowerShell
#Create required folders in user's home
mkdir ~/vimfiles
mkdir ~/vimfiles/autoload
mkdir ~/vimfiles/bundle

#Download wim script for easily adding plugings
Invoke-WebRequest -Uri https://tpo.pe/pathogen.vim -UseBasicParsing | Set-Content -Path ~\vimfiles\autoload\pathogen.vim

#Add to vimrc next to easy manage your 'runtimepath'
$content = @"                               
execute pathogen#infect()                   
syntax on                                   
filetype plugin indent on                   
"@                                          
                                            
$content | Set-Content ~\.vimrc             

#Add syntax
cd ~/vimfiles/bundle
git clone https://github.com/PProvost/vim-ps1
```

