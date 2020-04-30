[![Build Status](https://travis-ci.org/leflamm/afpre.svg?branch=master)](https://travis-ci.org/leflamm/afpre) [![GitHub release](https://img.shields.io/github/release/leflamm/afpre.svg)](https://github.com/leflamm/afpre/releases/latest)

Simple script to access the [AWS Federation Proxy (AFP)](https://docs.aws.amazon.com/amp/latest/userguide/install-option-connector.html) or [AWS Security Token Service (STS)](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html). Its main use case is starting a new shell where your temporary AWS credentials have been exported into the environment.

Inspired by https://github.com/ImmobilienScout24/afp-cli

# Flavours

## AWS Federation Proxy (AFP)

When used with an AFP server, performs basic auth towards the configured host, obtains and extracts temporary credentials.

## AWS Security Token Service

When used with the AWS STS, performs `aws sts assume-role` call with configured role and account, obtains and extracts temporary credentials.

# Features

## No Expired Tokens
The started `bash` will notice when the AWS tokens are about to expire. It will then renew the necessary tokens itself. No need to log out and in again.

```
$ ./afpre 
token expired - need to renew...
authenticating as some.user for some.account/some.role against some.afp.host
done.
AFP| ~/git/hub/afpre$ # do some work ...
AFP| ~/git/hub/afpre$ # typically for an hour or so ...
AFP| ~/git/hub/afpre$ # your tokens have expired ...
token expired - need to renew...
authenticating as some.user for some.account/some.role against some.afp.host
done.
AFP| ~/git/hub/afpre$ # do some more work ...
```
You can type `awsenv` to print aws-specific environment information.

## Manual Renewal

Whithin an `afpre` session you can - if you feel the need - manually trigger a token renewal. Either only if your old tokens have expired ...
```
$ condrenew
```
... or even unconditionally by calling
```
$ renew
```
You can call `awsenv` to check for your current AWS credentials.
```
AFP| ~/git/hub/afpre$ awsenv 
AWS_ROLE=admin
AWS_SECRET_ACCESS_KEY=...
AWS_VALID_SECONDS=3600
AWS_ACCOUNT=...
AWS_SESSION_TOKEN=...
AWS_ACCESS_KEY_ID=...
AWS_SECURITY_TOKEN=...
```

In case you find that `condrenew` command somewhat useless you got it pretty much right - because that's what `afpre` does anyway. But since version `0.9.15` both commands are available for subprocesses as well. So even (e. g.) running scripts could trigger their own token renewals.

## Run Commands in an afpre Session

You can pass commands to `afpre` after a separating `--`. The session will close immediatly after the command has exited. Typically this is very usefull when iterating over accounts and running a command in all of them.

```
$ ./afpre [OPTIONS] -- <command>
```

Commands can also be Bash functions. Make sure to export them using `export -f <function name>` to make them available in the afpre session.


## Configuration

```
$ cat ~/.afpre 
ACCOUNT=<your desired account>
ROLE=<your desired role>
HOST=<your afp host> # not mandatory if STS mode used
_PATH=<your path to service endpoint, typically "/afp-api/latest/account">
NAME=<your username> # optional
PW=<your password> # optional, I wouldn't put it here
PATTERN=\${ACCOUNT}/\${ROLE} # optional, the message you want to see in front of the prompt
RENEW_INT=<custom token renew interval> # optional, defaults to token's expiry
INSECURE=<true|false> # optional, perform "insecure" SSL connections, defaults to false
```

Use option `--example-cfg` to create an example configuration file.

## Available Packages

See https://github.com/leflamm/afpre/releases

- .deb
- .rpm
