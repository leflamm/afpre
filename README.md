[![Build Status](https://travis-ci.org/leflamm/afpre.svg?branch=master)](https://travis-ci.org/leflamm/afpre)

Simple script to access the AWS Federation Proxy (AFP). Its main use case is starting a new shell where your temporary AWS credentials have been exported into the environment.

Inspired by https://github.com/ImmobilienScout24/afp-cli

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

## Configuration

```
$ cat ~/.afpre 
ACCOUNT=<your desired account>
ROLE=<your desired role>
HOST=<your afp host>
_PATH=<your path to service endpoint, typically "/afp-api/latest/account">
NAME=<your username> # optional
PW=<your password> # optional, I wouldn't put it here
PATTERN="${ACCOUNT}|${ROLE}" # optional, the message you want to see in front of the prompt
RENEW_INT=<custom token renew interval> # optional, defaults to token's expiry
```

Use option `--example-cfg` to create an example configuration file.

## Available Packages

See https://github.com/leflamm/afpre/releases

- .deb
