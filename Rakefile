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
    if `test -d .oh-my-zsh && echo 1`.empty?
        shell 'sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"'
    else
        puts "\tOh My Zsh is already installed."
    end
    puts
end

task :git do
    title "Installing Git..."
    if `which git`.empty?
        shell '/opt/homebrew/bin/brew git'
    else
        warning "Git is already installed."
    end
    puts
end

task :dotfiles do
    title "Cloning dotfiles..."
    if (Dir.entries(".") & DOTFILES).empty?
        shell 'git clone https://github.com/eonu/dotfiles.git'
        DOTFILES.each do |file|
            shell "mv dotfiles/#{file} #{file}"
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
        if !File.file? "/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code"
            shell 'curl -sSL "https://code.visualstudio.com/sha/download?build=stable&os=darwin-universal" -o vscode.zip'
            shell 'unzip vscode.zip -d /Applications'
            shell 'rm vscode.zip'
        else
            warning 'VS Code is already installed.'
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
    title "Setting up SSH key pair..."
    if !File.file? ".ssh/id_rsa"
        shell "ssh-keygen -t rsa -f .ssh/id_rsa"
    else
        warning ".ssh/id_rsa already exists."
    end
    puts
end

task :clear do
    title "Removing leftovers..."
    shell "rm README.md Rakefile"
    puts
    puts "\e[1;92mInstallation finished :)\e[0m"
end
