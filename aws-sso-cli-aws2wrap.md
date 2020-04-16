# run terraform commands using aws sso with aws cli version 2

i started using aws control tower.

curious about its features that i believe is inspired by their infamous well architected framework.

it comes with pre-defined core accounts to form a secure cloud platform, ready to be used by software makers.

i was introduced to aws cli version 2 and this command, `aws sso login profile=abc`.

this command allows me to login to my aws account and do stuff. all from my shell.

i can switch to different aws profiles too.

noice.

native aws cli commands is all covered.

things got sticky when i wanted to run terraform.

my shell environment was not configured with the credentials. it doesn't work seamlessly. like zippers. seamless.

the official way to do this is to get the credentials using their aws sso web console.

![screenshot of aws sso web console](images/screenshot-aws-sso-web-console-api-credentials.png)

argh.

when my token expires, i go back to the web console and get new credentials.

painful.

i want to keep my operating environment in shell.

my search brought me to this nifty project, https://github.com/linaro-its/aws2-wrap.

it's a tool that helps me export the aws sso credentials and let run commands in my shell environment.

noice noice.

i fixed up alias shortcuts for my shell.

``` bash
alias aws-sso-login-abc123="aws sso login --profile=abc123"

alias aws-sso-env-abc123="eval \"$(aws2-wrap --profile abc123 --export)\""
```

noice noice noice.

when i want to work with aws account abc123, i run,

``` shell
aws-sso-login-abc123
```

after i sign in to my aws account via the web console, i run,

``` shell
aws-sso-env-abc123
```

this will export the credentials as environment variables.

much better workflow.
