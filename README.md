# AWS CDK Samples

A set of samples I used to experiment with AWS CDK.

## Preparation to work with AWS CDK

### Install AWS CDK

```sh
$ sudo apt install -yqq nodejs npm

$ sudo npm i -g aws-cdk
```

Versions used in these projects are:
```sh
$ nodejs -v
v10.19.0

$ npm -v
6.14.4

$ cdk --version
1.91.0 (build 0f728ce)
```

### Creating a new AWS CDK project

```sh
$ mkdir <project-name> && cd <project-name>

$ cdk init --language=typescript
```

## Available projects in this repository

Some CDK examples have been converted to `worktree` branches and other are in sub folders, all `worktree` projects can be downloaded a working copy following these steps:

1. Clone the repo   

```sh
$ git clone https://github.com/chilcano/<this-repo>
```

2. List all branches  

```sh
$ git branch -a

+ code-server-ec2
* main
+ simple-frontend-backend-ecs
+ simple-php-ts-ecs
  remotes/origin/HEAD -> origin/main
  remotes/origin/code-server-ec2
  remotes/origin/main
  remotes/origin/simple-frontend-backend-ecs
  remotes/origin/simple-php-ts-ecs
```

3. Choose the branch where you want to work on  

```sh
$ git worktree add -B simple-php-ts-ecs simple-php-ts-ecs origin/simple-php-ts-ecs
```


## For further information

Follow this guide:   
[https://github.com/chilcano/how-tos/blob/main/src/git_frequent_commands.md](https://github.com/chilcano/how-tos/blob/main/src/git_frequent_commands.md)
