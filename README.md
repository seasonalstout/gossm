# HI JEREMY

PLEASE ACCEPT MY PR



# gossm

`gossm` is interactive CLI tool that you should select server in AWS and then could connect or send files your AWS server using start-session, ssh, scp under AWS Systems Manger Session Manager.
<p align="center">
<img src="https://storage.googleapis.com/gjbae1212-asset/gossm/start.gif" width="500", height="450" />
</p>

<p align="center"/>
<a href="https://circleci.com/gh/gjbae1212/gossm"><img src="https://circleci.com/gh/gjbae1212/gossm.svg?style=svg"></a>
<a href="https://hits.seeyoufarm.com"/><img src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fgjbae1212%2Fgossm"/></a>
<a href="/LICENSE"><img src="https://img.shields.io/badge/license-MIT-GREEN.svg" alt="license" /></a>
<a href="https://goreportcard.com/report/github.com/gjbae1212/gossm"><img src="https://goreportcard.com/badge/github.com/gjbae1212/gossm" alt="Go Report Card"/></a>
</p>

## Overview
`gossm` is interactive CLI tool that is related AWS Systems Manger Session Manager.
It can select a ec2 server installed aws-ssm-agent and then can connect its server using start-session, ssh.
As well as files can send using scp.  
If you will use `gossm` tool, this mean there will no need to open inbound 22 port in your ec2 server when is using ssh or scp command.  
Because AWS Systems Manger Session Manager is using ssh protocol tunneling.   
<br/>
**Additionally Features**
- `mfa` command has added. this command is to authenticate through AWS MFA, and then to save issued a temporary credentials in $HOME/.aws/credentials_mfa. (default expired time is after 6 hours)  
You should export global environment, such as `export AWS_SHARED_CREDENTIALS_FILE=$HOME/.aws/credentials_mfa`.    
With completed, you can execute aws cli and gossm conveniently without mfa authenticated.    
Refer to detail information below.
   
## Prerequisite 
- [required] Your ec2 servers in aws are installed [aws ssm agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html).
EC2 severs have to apply **AmazonSSMManagedInstanceCore** iam policy.     
If you would like to use ssh, scp command using gossm, aws ssm agent version **2.3.672.0 or later** is installed on ec2. 
- [required] **aws access key**, **aws secret key**
- [required] **ec2:DescribeInstances**, **ssm:StartSession permission**, **ssm:DescribeInstanceInformation**     
- [optional] It's better to possibly get to additional permission for **ec2:DescribeRegions**, **ssm:TerminateSession**

## Install
Support **x86_64**
```bash 
# homebrew
$ brew tap gjbae1212/gossm
$ brew install gossm

# mac
$ wget https://github.com/gjbae1212/gossm/releases/download/v1.3.2/gossm_1.3.2_Darwin_x86_64.tar.gz

# linux
$ wget https://github.com/gjbae1212/gossm/releases/download/v1.3.2/gossm_1.3.2_Linux_x86_64.tar.gz

# window
$ wget https://github.com/gjbae1212/gossm/releases/download/v1.3.2/gossm_1.3.2_Windows_x86_64.tar.gz
```

## How to use
### global command args
| args           | Description                                               | Default                |
| ---------------|-----------------------------------------------------------|------------------------|
| -c             | (optional) aws credentials file | $HOME/.aws/credentials |
| -p             | (optional) if you are having multiple aws profiles in credentials, it is name one of profiles | default |
| -r             | (optional) region in AWS that would like to connect |  |
| -t             | (optional) instanceId of server in AWS that would like to connect | |

If your machine don't exist $HOME/.aws/.credentials, have to pass `-c` args.  
```
# credentials file format
[default]
aws_access_key_id = AWS ACCESS KEY
aws_secret_access_key = AWS SECRET KEY
``` 
  
`-r` or `-t` don't pass args, it can select through interactive CLI.  
    
### command

#### start
```bash
$ gossm start 
```

#### ssh, scp
`-e` must pass args when is using scp.   
`-e` args is command and args when usually used to pass ssh or scp.
```bash
# ssh(if pem is already registered using ssh-add)
$ gossm ssh -e 'user@server-domain'

# ssh(if pem isn't registered)
$ gossm ssh -e '-i key.pem user@server-domain'

# ssh(if pem is already registered using ssh-add and don't pass -e option) -> select server using interactive cli
$ gossm ssh

# ssh(if pem isn't registered and don't pass -e option) -> select server using interactive cli
$ gossm ssh -i key.pem
 
# scp(if pem is already registered using ssh-add)
$ gossm scp -e 'file user@server-domain:/home/blahblah'

# scp(if pem isn't registered)
$ gossm scp -e '-i key.pem file user@server-domain:/home/blahblah'

```

#### cmd 
`-e` required args, it is a parameter for execute to command on selected servers.

```bash
# It is to execute a command("uptime") on selected multiple servers, waiting for a response on its result.
$ gossm cmd -e "uptime" 
```

#### fwd
`-z` Optionally specify the remote port to access
`-l` Optionally specify the local port to forward (If not specified when using `-z`, then this value defaults to the value of `-z`)

```bash
$ gossm fwd -z 8080 -l 42069
```
If not specified, you will be prompted to enter a remote and local port after selecting a target. 
 
**ex)**  
<p align="center">
<img src="https://storage.googleapis.com/gjbae1212-asset/gossm/ssh.gif" width="500", height="450" />
</p>

#### mfa
`-deadline` it's to set expire time for temporary credentials. **default** is 6 hours.  
`-device` it's to set mfa device. **default** is your virtual mfa device.
```bash
$ gossm mfa <your-mfa-code>
```
**Must set to `export AWS_SHARED_CREDENTIALS_FILE=$HOME/.aws/credentials_mfa` in .bash_profile, .zshrc.**

**ex)**  
<p align="center">
<img src="https://storage.googleapis.com/gjbae1212-asset/gossm/mfa.png" />
</p>
 
## LICENSE
This project is following The MIT.
