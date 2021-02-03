# AWS CDK Samples

A set of samples I used to experiment with AWS CDK.

## Projects

All CDK examples have been converted to `worktree` branches and they can be download a working copy following these steps:

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

## References

- [AWS CDK](https://github.com/aws/aws-cdk)
- [AWS CDK Documentation](https://docs.aws.amazon.com/cdk/latest/guide/home.html)
- [AWS CDK Examples](https://github.com/aws-samples/aws-cdk-examples)


## Troubleshooting

### 1. Incompatibility of dependencies

> unable to determine cloud assembly output directory. Assets must be defined indirectly within a "Stage" or an "App" scope  
> Subprocess exited with error 1  
   

> ⨯ Unable to compile TypeScript:  
> lib/simple-frontend-backend-ecs-stack.ts:12:29 - error TS2345: Argument of type 'this' is not assignable to parameter of type 'Construct'.  

#### Reference:
* [https://github.com/aws/aws-cdk/issues/9578](https://github.com/aws/aws-cdk/issues/9578)

#### Solution:

It's because your dependencies aren't aligned. You'll need the same version of all the CDK packages. Align the subtle difference in the list of CDK modules in the `package.json`. You should have something like this:
```json
[...]
  },
  "dependencies": {
    "@aws-cdk/aws-ec2": "1.57.0",
    "@aws-cdk/aws-ecr": "1.57.0",
    "@aws-cdk/aws-ecr-assets": "1.57.0",
    "@aws-cdk/aws-ecs": "1.57.0",
    "@aws-cdk/aws-ecs-patterns": "1.57.0",
    "@aws-cdk/aws-logs": "1.57.0",
    "@aws-cdk/core": "1.57.0",
    "source-map-support": "0.5.16"
  }
}
```
Set up the same version updating the `package.json`, then remove `node_modules` folder and `package-lock.json` file and re-import all modules running `npm install`.

### 2. Docker is not installed

> CDK can not build the Docker images required for your project.

```sh
...
[0%] start: Publishing a0e03e994b7d88ba9c688c24b33ee785b12e591b4188fdfd1acddb8865270899:current
[100%] fail: write EPIPE

 ❌  SimpleFrontendBackendEcsStack failed: Error: Failed to publish one or more assets. See the error messages above for more information.
Error: spawn docker ENOENT
    at Process.ChildProcess._handle.onexit (internal/child_process.js:240:19)
    at onErrorNT (internal/child_process.js:415:16)
    at process._tickCallback (internal/process/next_tick.js:63:19)
``` 
#### Solution:

You need to install Docker in your local computer.  
```sh
$ sudo apt install -y docker.io
$ sudo systemctl enable --now docker
$ sudo apt-mark hold docker.io
$ sudo usermod -aG docker $USER

// check the installation
$ docker --version
Docker version 19.03.8, build afacb8b7f0

$ sudo docker container run hello-world

// cleanning installation
$ sudo docker container stop $(docker container ls -aq)
$ sudo docker system prune -a --volumes

// you can now uninstall Docker as any other package installed with apt
$ sudo apt purge docker.io
$ sudo apt autoremove
```

### 3. Docker is installed but it isn't working

> We have installed Docker but it still is not working.  

```sh
...
[0%] start: Publishing a0e03e994b7d88ba9c688c24b33ee785b12e591b4188fdfd1acddb8865270899:current
[100%] fail: docker login --username AWS --password-stdin https://263455585760.dkr.ecr.us-east-1.amazonaws.com exited with error code 1: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/auth: dial unix /var/run/docker.sock: connect: permission denied

 ❌  SimpleFrontendBackendEcsStack failed: Error: Failed to publish one or more assets. See the error messages above for more information.
Failed to publish one or more assets. See the error messages above for more information.
``` 

#### Solution:

Once Docker has been installed, you have to restart your computer. If it already has been installed, you can check its status to see if it is disabled, if so, you have to execute this:
```sh
$ sudo systemctl status docker
$ sudo systemctl enable --now docker
$ sudo systemctl start docker
```
