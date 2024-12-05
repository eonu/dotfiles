# TODO: convert to script, install ruby for bin scripts

# clone repo
git clone https://github.com/eonu/dotfiles.git

# copy all dotfiles to ~
# copy bin to ~

# fetch VSCode keys
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.mcode --install-extension arcticicestudio.nord-visual-studio-codeicrosoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
dnf check-update

# install packages
sudo dnf install code bat htop jq fzf emacs zsh -y

# install ohmyzsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# install gnome-terminal
gnome-terminal
# create new default profile using /usr/bin/zsh as shell

# install nord for gnome-terminal
git clone https://github.com/nordtheme/gnome-terminal.git
cd gnome-terminal/src
./nord.sh
rm gnome-terminal
# switch to new Nord theme as default

# install Nord and IntelliJ IDEA Light VSCode themes
code --install-extension arcticicestudio.nord-visual-studio-code
code --install-extension AlexTi.intellij-idea-light-theme
