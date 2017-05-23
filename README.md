Simple script to access the AWS Federation Proxy (AFP). Its main use case is starting a new shell where your temporary AWS credentials have been exported into the environment.

Similar to https://github.com/ImmobilienScout24/afp-cli

## Configuration

```
$ cat ~/.afpre 
ACCOUNT=<your desired account>
ROLE=<your desired role>
HOST=<your afp host>
_PATH=<your path to service endpoint, typically "/afp-api/latest/account">
NAME=<your username> # optional
PW=<your password> # optional, I wouldn't put it here
```
