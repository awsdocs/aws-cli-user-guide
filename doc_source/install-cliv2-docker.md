# Using the official AWS CLI version 2 Docker image<a name="install-cliv2-docker"></a>

This topic describes how to run, version control, and configure the AWS CLI version 2 on Docker\. For more information on how to use Docker, see [Docker's documentation](https://docs.docker.com/)\.

Official Docker images provide isolation, portability, and security that AWS directly supports and maintains\. This enables you to use the AWS CLI version 2 in a container\-based environment without having to manage the installation yourself\. 

**Note**  
The AWS CLI version 2 is the only tool that's supported on the official AWS Docker image\.

**Topics**
+ [Prerequisites](#cliv2-docker-prereq)
+ [Run the official AWS CLI version 2 Docker image](#cliv2-docker-install)
+ [Use specific versions and tags](#cliv2-docker-upgrade)
+ [Update to the latest Docker image](#cliv2-docker-update)
+ [Share host files, credentials, and configuration](#cliv2-docker-share-files)
+ [Shorten the Docker command](#cliv2-docker-aliases)

## Prerequisites<a name="cliv2-docker-prereq"></a>

You must have Docker installed\. For installation instructions, see the [Docker website](https://docs.docker.com/install/)\. 

To verify your installation of Docker, run the following command and confirm there is an output\.

```
$ docker --version
Docker version 19.03.1
```

## Run the official AWS CLI version 2 Docker image<a name="cliv2-docker-install"></a>

The official AWS CLI version 2 Docker image is hosted on DockerHub in the `amazon/aws-cli` repository\. The first time you use the `docker run` command, the latest Docker image is downloaded to your computer\. Each subsequent use of the `docker run` command runs from your local copy\. 

To run the AWS CLI version 2 Docker image, use the `docker run` command\.

```
$ docker run --rm -it amazon/aws-cli command
```

This is how the command functions:
+ `docker run --rm --it amazon/aws-cli` – The equivalent of the `aws` executable\. Each time you run this command, Docker spins up a container of your downloaded `amazon/aws-cli` image, and executes your `aws` command\. By default, the Docker image uses the latest version of the AWS CLI version 2\.

  For example, to call the `aws --version` command in Docker, you run the following\.

  ```
  $ docker run --rm -it amazon/aws-cli --version
  aws-cli/2.0.47 Python/3.7.3 Linux/4.9.184-linuxkit botocore/2.0.0dev10
  ```
+ `--rm` – Specifies to clean up the container after the command exits\.
+ `-it` – Specifies to open a pseudo\-TTY with `stdin`\. This enables you to provide input to the AWS CLI version 2 while it's running in a container, for example, by using the `aws configure` and `aws help` commands\. 

For more information about the `docker run` command, see the [Docker reference guide](https://docs.docker.com/engine/reference/run/)\.

## Use specific versions and tags<a name="cliv2-docker-upgrade"></a>

The official AWS CLI version 2 Docker image has multiple versions you can use, starting with version 2\.0\.6\. To run a specific version of the AWS CLI version 2, append the appropriate tag to your `docker run` command\. The first time you use the `docker run` command with a tag, the latest Docker image for that tag is downloaded to your computer\. Each subsequent use of the `docker run` command with that tag runs from your local copy\. 

You can use two types of tags: 
+ `latest` – Defines the latest version of the AWS CLI version 2 for the Docker image\. We recommend you use the `latest` tag when you want the latest version of the AWS CLI version 2\. However, there are no backward\-compatibility guarantees when relying on this tag\. The `latest` tag is used by default in the `docker run` command\. To explicitly use the `latest` tag, append the tag to the container image name\.

  ```
  $ docker run --rm -it amazon/aws-cli:latest command
  ```
+ `<major.minor.patch>` – Defines a specific version of the AWS CLI version 2 for the Docker image\. If you plan to use the Docker image in production, we recommend you use a specific version of the AWS CLI version 2 to ensure backward compatibility\. For example, to run version 2\.0\.6, append the version to the container image name\.

  ```
  $ docker run --rm -it amazon/aws-cli:2.0.6 command
  ```

## Update to the latest Docker image<a name="cliv2-docker-update"></a>

Because the latest Docker image is downloaded to your computer only the first time you use the `docker run` command, you need to manually pull an updated image\. To manually update to the latest version, we recommend you pull the `latest` tagged image\. Pulling the Docker image downloads the latest version to your computer\.

```
$ docker pull amazon/aws-cli:latest
```

## Share host files, credentials, and configuration<a name="cliv2-docker-share-files"></a>

Because the AWS CLI version 2 is run in a container, by default the CLI can't access the host file system, which includes configuration and credentials\. To share the host file system, credentials, and configuration to the container, mount the host system’s `~/.aws` directory to the container at `/root/.aws` with the `-v` flag to the `docker run` command\. This allows the AWS CLI version 2 running in the container to locate host file information\.

```
$ docker run --rm -it -v ~/.aws:/root/.aws amazon/aws-cli command
```

For more information about the `-v` flag and mounting, see the [Docker reference guide](https://docs.docker.com/storage/volumes/)\. 

### Example 1: Providing credentials and configuration<a name="cliv2-docker-share-files-config"></a>

In this example, we're providing host credentials and configuration when running the `s3 ls` command to list your buckets in Amazon Simple Storage Service \(Amazon S3\)\.

```
$ docker run --rm -ti -v ~/.aws:/root/.aws amazon/aws-cli s3 ls
2020-03-25 00:30:48 aws-cli-docker-demo
```

### Example 2: Downloading an Amazon S3 file to your host system<a name="cliv2-docker-share-files-s3"></a>

For some AWS CLI version 2 commands, you can read files from the host system in the container or write files from the container to the host system\. 

In this example, we download the `S3` object `s3://aws-cli-docker-demo/hello` to your local file system by mounting the current working directory to the container's `/aws` directory\. By downloading the `hello` object to the container's `/aws` directory, the file is saved to the host system’s current working directory also\.

------
#### [ Linux and macOS ]

```
$ docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli s3 cp s3://aws-cli-docker-demo/hello .
download: s3://aws-cli-docker-demo/hello to ./hello
```

------
#### [ Windows ]

```
$ docker run --rm -it -v %cd%.aws:/root/.aws -v %cd%:/aws amazon/aws-cli s3 cp s3://aws-cli-docker-demo/hello .
download: s3://aws-cli-docker-demo/hello to ./hello
```

------

To confirm the downloaded file exists in the local file system, run the following\.

------
#### [ Linux and macOS ]

```
$ cat hello
Hello from Docker!
```

------
#### [ Windows ]

```
$ type hello
Hello from Docker!
```

------

## Shorten the Docker command<a name="cliv2-docker-aliases"></a>

To shorten the Docker `aws` command, we suggest you use your operating system's ability to create a [https://www.linux.com/tutorials/understanding-linux-links/](https://www.linux.com/tutorials/understanding-linux-links/) \(symlink\) or [https://www.linux.com/tutorials/aliases-diy-shell-commands/](https://www.linux.com/tutorials/aliases-diy-shell-commands/) in Linux and macOS, or [https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/doskey) in Windows\. To set the `aws` alias, you can run one of the following commands\.
+ For basic access to `aws` commands, run the following\.

------
#### [ Linux and macOS ]

  ```
  $ alias aws='docker run --rm -it amazon/aws-cli'
  ```

------
#### [ Windows ]

  ```
  C:\> doskey aws=docker run --rm -it amazon/aws-cli $*
  ```

------
+ For access to the host file system and configuration settings when using `aws` commands, run the following\.

------
#### [ Linux and macOS ]

  ```
  $ alias aws='docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli'
  ```

------
#### [ Windows ]

  ```
  C:\> doskey aws=docker run --rm -it -v ~/.aws:/root/.aws -v %cd%:/aws amazon/aws-cli $*
  ```

------
+ To assign a specific version to use in your `aws` alias, append your version tag\.

------
#### [ Linux and macOS ]

  ```
  $ alias aws='docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli:2.0.6'
  ```

------
#### [ Windows ]

  ```
  C:\> doskey aws=docker run --rm -it -v ~/.aws:/root/.aws -v %cd%:/aws amazon/aws-cli:2.0.6 $*
  ```

------

After setting your alias, you can run the AWS CLI version 2 from within a Docker container as if it's installed on your host system\. 

```
$ aws --version
aws-cli/2.0.47 Python/3.7.3 Linux/4.9.184-linuxkit botocore/2.0.0dev10
```