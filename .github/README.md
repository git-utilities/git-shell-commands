# Git Shell Commands
[heading__title]:
  #git-shell-commands
  "&#x2B06; Top of this page"


[`git-shell-commands`][master__get_shell_commands], contains a collection of scripts for Git Shell accounts. The following covers how to install this branch within a git-shell restricted account.


## [![Repository Size][badge__master__git_shell_commands__size]][master__get_shell_commands] [![Open Issues][badge__issues__git_shell_commands]][issues__git_shell_commands] [![Open Pull Requests][badge__pull_requests__git_shell_commands]][pull_requests__git_shell_commands] [![Latest commits][badge__commits__git_shell_commands__master]][commits__git_shell_commands__master]



------


#### Table of Contents


- [&#x2B06; Top of ReadMe File][heading__title]

- [:zap: Quick Start][heading__quick_start]

- [&#x1F5D2; Notes][notes]

- [:copyright: License][heading__license]


------


## Quick Start
[heading__quick_start]:
  #quick-start
  "&#9889; ...well as quick as it may get with things like this"


**Bash Variables**


```Bash
_git_user='git-user'
_git_group='devs'
_git_home_base='/srv'
_ssh_pub_key_path='/home/admin/client-keys/git-user/id_rsa.pub'
_git_https_url='https://github.com/git-utilities/git-shell-commands.git'
```


**Add Git shell**


```Bash
tee -a /etc/shells 1>/dev/null <<<"$(which git-shell)"
```


**Add Git user**


```Bash
adduser\
 --system\
 --disabled-password\
 --gecos ''\
 --shell "$(which git-shell)"\
 --home "${_git_home_base,,}/${_git_user,,}"\
 --ingroup "${_git_group}"\
 "${_git_user}"
```


**Clone to Git user's home directory**


```Bash
sudo su --login "${_git_user}" --shell /bin/bash <<EOF
git clone --recurse-submodules "${_git_https_url}"
EOF
```


**Add SHH public key**


```Bash
sudo su --login "${_git_user}" --shell /bin/bash <<EOF
mkdir .ssh
tee -a .ssh/authorized_keys 1>/dev/null <<<"$(<"${_ssh_pub_key_path}")"
chmod 600 .ssh/authorized_keys
EOF
```


**Set executable permissions**


```Bash
sudo su --login "${_git_user}" --shell /bin/bash <<'EOF'
while IFS= read -r -d '' _path; do
  _file_type="$(file --brief --mime-type "${_path}")"
  if [[ "${_file_type}" == 'text/x-shellscript' ]]; then
    chmod --verbose u+x "${_path}"
  fi
done < <(find 'git-shell-commands/' -type f -not -path '*.*' -print0)
EOF
```


___


## Notes
[notes]:
  #notes
  "&#x1F5D2; Additional notes and links that may be worth clicking in the future"


To disable `push` and `pull` remove the Git tracking files and directories


```Bash
sudo su --login "${_git_user}" --shell /bin/bash <<'EOF'
find "./git-shell-commands" -type d -name '.git' -exec bash -c 'rm -r "$0"' {}
find "./git-shell-commands" -type f -name '.git' -exec bash -c 'rm "$0"' {}
find "./git-shell-commands" -type f -name '.gitmodules' -exec bash -c 'rm "$0"' {}
EOF
```


To disable interactive logins

```Bash
sudo su --login "${_git_user}" --shell /bin/bash <<EOC
tee 'git-shell-commands/no-interactive-login' 1>/dev/null <<'EOF'
#!/usr/bin/env bash
printf 'Hi %s, you have successfully authenticated!\n' "${USER}"
printf 'However, there is not an interactive shell here.\n'
exit 128
EOF

chmod u+x 'git-shell-commands/no-interactive-login'
EOC
```


To list scripts available to Git user


```Bash
ssh "${_git_user}"@localhost -i "${_ssh_pub_key_path}" list --help
```


**Example client `~/.ssh/config`** SSH configurations such as the following may be useful in making SSH/Git commands more terse


```sshd_config
Host git-user
   IdentitiesOnly yes
   IdentityFile ~/.ssh/id_rsa
   HostName 192.168.0.2
   User git-user
```


Each script should have documentation on arguments and usage accessible via `--help` or `-h` options


```Bash
ssh git-user git-init --help
```


Pull Requests are welcomed! Check the _`Community`_ section for development tips and code of conduct relevant updates.


___


## License
[heading__license]:
  #license
  "&#x00A9; Legal bits of Open Source software"


```
Git Shell Commands submodule quick start documentation
Copyright (C) 2019  S0AndS0

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published
by the Free Software Foundation; version 3 of the License.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```



[badge__travis_ci__git_shell_commands]:
  https://img.shields.io/travis/git-utilities/git-shell-commands/example.svg

[travis_ci__git_shell_commands]:
  https://travis-ci.com/git-utilities/git-shell-commands
  "&#x1F6E0; Automated tests and build logs"


[branch_example__example_usage]:
  https://github.com/git-utilities/git-shell-commands/blob/example/example-usage.sh
  "Bash script that shows some ways of utilizing code from the master branch of this repository"


[badge__commits__git_shell_commands__master]:
  https://img.shields.io/github/last-commit/git-utilities/git-shell-commands/master.svg

[commits__git_shell_commands__master]:
  https://github.com/git-utilities/git-shell-commands/commits/master
  "&#x1F4DD; History of changes on this branch"


[git_shell_commands__community]:
  https://github.com/git-utilities/git-shell-commands/community
  "&#x1F331; Dedicated to functioning code"


[git_shell_commands__example_branch]:
  https://github.com/git-utilities/git-shell-commands/tree/example
  "If it lurches, it lives"


[badge__issues__git_shell_commands]:
  https://img.shields.io/github/issues/git-utilities/git-shell-commands.svg

[issues__git_shell_commands]:
  https://github.com/git-utilities/git-shell-commands/issues
  "&#x2622; Search for and _bump_ existing issues or open new issues for project maintainer to address."


[badge__pull_requests__git_shell_commands]:
  https://img.shields.io/github/issues-pr/git-utilities/git-shell-commands.svg

[pull_requests__git_shell_commands]:
  https://github.com/git-utilities/git-shell-commands/pulls
  "&#x1F3D7; Pull Request friendly, though please check the Community guidelines"


[badge__master__git_shell_commands__size]:
  https://img.shields.io/github/languages/code-size/git-utilities/git-shell-commands.svg

[master__get_shell_commands]:
  https://github.com/git-utilities/git-shell-commands/
  "&#x2328; Project source code!"
