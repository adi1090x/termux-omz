#!/data/data/com.termux/files/usr/bin/bash

## Author  : Aditya Shakya
## Twitter : @adi1090x
## Github  : @adi1090x
## Reddit  : @adi1090x

## A simple script to setup oh-my-zsh in termux terminal emulator.

## ANSI Colors (FG & BG)
RED="$(printf '\033[31m')"         GREEN="$(printf '\033[32m')"
ORANGE="$(printf '\033[33m')"      BLUE="$(printf '\033[34m')"
MAGENTA="$(printf '\033[35m')"     CYAN="$(printf '\033[36m')"
WHITE="$(printf '\033[37m')"       BLACK="$(printf '\033[30m')"
REDBG="$(printf '\033[41m')"       GREENBG="$(printf '\033[42m')"
ORANGEBG="$(printf '\033[43m')"    BLUEBG="$(printf '\033[44m')"
MAGENTABG="$(printf '\033[45m')"   CYANBG="$(printf '\033[46m')"
WHITEBG="$(printf '\033[47m')"     BLACKBG="$(printf '\033[40m')"
DEFAULT_FG="$(printf '\033[39m')"  DEFAULT_BG="$(printf '\033[49m')"

## Directories
PREFIX='/data/data/com.termux/files/usr'
DIR="$(pwd)"

## Reset colors
reset_color () {
	{ printf '\033[39m'; printf '\033[49m'; }
}

## Banner
banner () {
    clear
    cat <<- EOF
	${BLUE} ┌─────────────────────────────────┐
	${BLUE} │${RED}   ┏━┓╻ ╻   ${ORANGE}┏┳┓╻ ╻   ${GREEN}╺━┓┏━┓╻ ╻   ${BLUE}│
	${BLUE} │${RED}   ┃ ┃┣━┫${CYAN}╺━╸${ORANGE}┃┃┃┗┳┛${CYAN}╺━╸${GREEN}┏━┛┗━┓┣━┫   ${BLUE}│
	${BLUE} │${RED}   ┗━┛╹ ╹   ${ORANGE}╹ ╹ ╹    ${GREEN}┗━╸┗━┛╹ ╹   ${BLUE}│
	${BLUE} └─────────────────────────────────┘
	${CYAN}  By: ${MAGENTA}Aditya Shakya ${ORANGE}// ${MAGENTA}adi1090x
	EOF
}

## Script Termination
exit_on_signal_SIGINT () {
    { printf "\n\n%s\n" " ${BLUE}[${RED}*${BLUE}] ${RED}Script interrupted." 2>&1; echo; reset_color; }
    exit 0
}

exit_on_signal_SIGTERM () {
    { printf "\n\n%s\n" " ${BLUE}[${RED}*${BLUE}] ${RED}Script terminated." 2>&1; echo; reset_color; }
    exit 0
}

trap exit_on_signal_SIGINT SIGINT
trap exit_on_signal_SIGTERM SIGTERM

## Install ZSH
install_zsh () {
	{ echo ${ORANGE}" [*] Installing ZSH..."${CYAN}; echo; }
	if [[ -e $PREFIX/bin/zsh ]]; then
		{ echo ${GREEN}" [*] ZSH is already Installed!"; echo; }
	else
		{ pkg --check-mirror update -y; pkg install -y zsh; }
		(type -p zsh &> /dev/null) && { echo; echo ${GREEN}" [*] Succesfully Installed!"; echo; } || { echo; echo ${RED}" [!] Error Occured, ZSH is not installed."; echo; reset_color; exit 1; }
	fi
}

## Backup user settings
backup_tmux () {
	if [[ ! -d "$HOME/.termux" ]]; then
		mkdir $HOME/.termux
	else
		cp -r $HOME/.termux{,.backup}
	fi
}

## Setup OMZ
setup_omz () {
	{ echo ${ORANGE}" [*] Cloning oh-my-zsh framework..."; echo ${CYAN}; }
	git clone --depth 1 https://github.com/ohmyzsh/ohmyzsh.git $HOME/.oh-my-zsh
	[[ -d $HOME/.oh-my-zsh ]] && { echo; echo ${GREEN}" [*] Succesfully Cloned!"; echo; } || { echo; echo ${RED}" [!] Error Occured, failed to clone OMZ."; echo; reset_color; exit 1; }
	{ echo ${ORANGE}" [*] Setting up configurations..."; echo; }
	cp $HOME/.oh-my-zsh/templates/zshrc.zsh-template $HOME/.zshrc
	{ echo ${ORANGE}" [*] Changing default shell to 'zsh'."; echo; }
	chsh -s zsh
	{ echo ${ORANGE}" [*] Applying color-scheme, font & zsh theme..."; echo; }
	{ cp $DIR/.files/colors.properties $HOME/.termux/colors.properties; cp $DIR/.files/font.ttf $HOME/.termux/font.ttf; }
	sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="agnoster"/g' $HOME/.zshrc
	termux-reload-settings
	{ echo ${GREEN}" [*] Setup completed. Enjoy oh-my-zsh in Termux."; echo; }
	{ reset_color; exit; }
}

## Main
main () {
	banner
	# Check for previous installation
	if [[ -d $HOME/.oh-my-zsh ]]; then
		{ echo; echo ${GREEN}" [!] ${MAGENTA}Oh-my-zsh ${GREEN}is already installed."; }
		{ read -p ${ORANGE}" [?] Do you wanna re-install it? (y/n): ${GREEN}"; echo; }
		if [[ "$REPLY" =~ ^[y/Y]$ ]]; then
			{ rm -rf $HOME/.oh-my-zsh; install_zsh; backup_tmux; setup_omz; } 
		else
			{ reset_color; exit; }
		fi
	else
		{ echo; install_zsh; backup_tmux; setup_omz; }
	fi
}

main
