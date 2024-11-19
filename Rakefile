task default: %w[ohmyzsh brew:brew git dotfiles brew:packages vscode:themes ssh clear]

DOTFILES = %w[bin .aliases .emacs .exports .functions .gitconfig .init .path .zshrc]

def title str
    puts "\e[1m#{str}\e[0m"
end

def warning str
    puts "\t\e[93m#{str}\e[0m"
end

def shell command
    puts "\t\e[90m#{command}\e[0m"
    sh "#{command} 2>&1 | sed 's/^/\t/'", verbose: false
end

task :ohmyzsh do
    title "Installing oh-my-zsh..."
    if File.directory? "#{Dir.home}/.oh-my-zsh"
        warning "Oh My Zsh is already installed."
    else
        shell 'sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"'
    end
    puts
end

task :git do
    title "Installing Git..."
    if `which git`.empty?
        shell '/opt/homebrew/bin/brew install git'
    else
        warning "Git is already installed."
    end
    puts
end

task :dotfiles do
    title "Cloning dotfiles..."
    if (Dir.entries(Dir.home) & DOTFILES).empty?
        shell 'git clone https://github.com/eonu/dotfiles.git'
        DOTFILES.each do |file|
            shell "mv dotfiles/#{file} ~/#{file}"
        end
        shell 'rm -rf dotfiles'
    else
        warning "Dotfiles have already been cloned."
    end
    puts
end

namespace :brew do 
    task :brew do
        title "Installing Homebrew..."
        if `which /opt/homebrew/bin/brew`.empty?
            shell '/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"'
        else
            warning "Homebrew is already installed."
        end
        puts
    end
    task :packages do
        title "Installing Homebrew packages..."
        shell '/opt/homebrew/bin/brew install vivid coreutils bat fzf tree htop emacs jq'
        puts
    end
end

namespace :vscode do
    task :vscode do
        title "Installing VS Code..."
        if File.file? "/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code"
            warning 'VS Code is already installed.'
        else
            shell 'curl -sSL "https://code.visualstudio.com/sha/download?build=stable&os=darwin-universal" -o vscode.zip'
            shell 'unzip vscode.zip -d /Applications'
            shell 'rm vscode.zip'
        end
        puts
    end
    task themes: [:vscode] do
        title "Installing VS Code themes: Nord & IntelliJ IDEA Light Theme"
        shell '/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code --install-extension arcticicestudio.nord-visual-studio-code'
        shell '/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code --install-extension AlexTi.intellij-idea-light-theme'
        puts
    end
end

task :ssh do
    title "Setting up SSH directory and creating key pair..."
    if File.directory? "#{Dir.home}/.ssh"
        warning ".ssh directory already exists."
    else
        shell "mkdir ~/.ssh"
        shell "touch ~/.ssh/authorized_keys"
        shell "touch ~/.ssh/config"
        shell "chmod 700 ~/.ssh"
        shell "chmod 644 ~/.ssh/authorized_keys"
        shell "chmod 600 ~/.ssh/config"
        shell "ssh-keygen -t rsa -f ~/.ssh/id_rsa"
    end
    puts
end

task :clear do
    title "Removing Rakefile..."
    shell "rm Rakefile"
    puts
    puts "\e[1;92mInstallation finished :)\e[0m"
end
