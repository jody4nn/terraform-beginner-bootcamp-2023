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

### Working with Env Vars

#### env command
We can list out all Environment Variables (Env Vars) using the `env` command

We can filter specific env variables using grep e.g.   `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO='world'`

In  the terminal we can unset using `unset HELLO`

We can set an env var temporarily when just running a command

```sh
HELLO = 'world' ./bin/print_message
```

Within a bash script we can set env without writing export e.g.

```sh
#!/usr/bin/env bash

HELLO='world'
echo $HELLO
```

### Printing Vars

We can print an env var using echo e.g. `echo $HELLO`

### Scoping of Env Vars

When you open up a new bash terminals in VSCode it will not be aware of env vars that you have set in another winodw.

If you want Env Vars to persist across all future bash terminals that are open you need to set env vars in your bash profile. e.g. `.bash_profile`.

#### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```sh
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminas opened in those workspaces.

You can also set env vars in the `gitpod.yml` but this can only contain non-sensitive env vars.

### AWS CLI Installation

AWS CLI is installed for the project via the bash script [./bin/install_aws_cli](./bin/install_aws_cli)

[Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

[Getting Started (AWS ENV VARS)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

We can check if our AWS credentials is configured correctly by running the following AWS CLI command

```sh
aws sts get-caller-identity
```

If it is successful you should see a json payload returnh that looks like this:

```json
{
    "UserId": "ABCDE1234",
    "Account": "1234567890",
    "Arn": "arn:aws:iam::1234567890:user/terraform_beginner_bootcamp"
}


```

We'll need to generate AWS CLI credits from IAM user in order to  use the AWS cli.