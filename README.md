# Terraform Beginner Bootcamp 2023

## Semantic Versioning :mage:

This project is going to utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, e.g. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Considerations with the Terraform CLI changes
The Terraform CLI installation instruction have changed due to gpg keyring changes. So we needed to refer to the latest install CLI instructions via Terraform Documentation and the change the scripting for the install.

[Install Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Considerations for Linux Distribution 

This project is built against Ubuntu.
Please check your Linux Distribution 
and change accordingly to distribution needs.

[How to Check OS Version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of checking OS version
```sh
$cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
```


### Refactoring into Bash Scripts

While fixing the Terraform CLI gpg deprecation issues, we noticed that the bash script steps were a considerable amount more code. So we decided to create a bash script to install the Terraform CLI.

This bash script is located here: [./bin/install_terraform_cli](./bin/install_terraform_cli.sh)

- This will keep the Gitpod Task File ([.gitpod.yml](gitpod.yml)) tidy.
- This will allow us an easier time to debug and execute manually Terraform CLI install.
- This will allow better portability for other projects that need to install Terraform CLI. 

### Shebang

A Shebang (pronounced sha-bang) tells the bash script what program will interpret the script. e.g. `#!/bin/bash`

ChatGpt recommended this format for bash: `#!/usr/bin/env bash`

- for portability for different OS distributions
- will search the user's PATH for the bash executable

https://en.wikipedia.org/wiki/Shebang_(Unix)


## Execution Considerations

When executing the bash script we can use the `./` shorthand notation to execute the bash script.

e.g.    `./bin/install_terraform_cli`

If we are using a script in .gitpod.yml we need to point the script  to a program to interpret it

e.g.   `source ./bin/install_terraform_cli`

#### Linux Permissions Considerations

Linux permissions works as follows:

In order to make our bash scripts executable we need to change Linux permissions for the fix to be executable.

```sh
chmod u+x ./bin/install_terraform_cli
```
alternatively:

```sh
chmod 744 ./bin/install_terraform_cli
```
https://en.wikipedia.org/wiki/Chmod


### Github Lifecycle (Before, Init, Command)

We need to be careful when using the Init because it will not rerun if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle

