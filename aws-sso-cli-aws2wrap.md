# Run Terraform using AWS SSO credentials with AWS CLI version 2 and aws2-wrap

I started using AWS Control Tower.

Curious about its features, inspired by their infamous Well Architected framework.

It comes with pre-defined core accounts to form a secure cloud platform, ready to be used by software makers.

Then, i was introduced to AWS CLI version 2 and this command, 

``` shell
aws sso login --profile=abc
```

This command allows me to login to my AWS account and do stuff from my shell.

I can switch to different aws profiles too without an additional tool.

Noice.

Native AWS cli commands works perfectly. 

Good workflow.

Things got sticky when  wanted to run Terraform.

``` shell
$ terraform plan

Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

Error: No valid credential sources found for AWS Provider.
 Please see https://terraform.io/docs/providers/aws/index.html for more information on
 providing credentials for the AWS Provider
```

My shell environment was not configured with the credentials from AWS SSO. 

It didn't work seamlessly.

Like zippers.

The "official" way to do this is to get the credentials using their AWS SSO web console.

![screenshot of aws sso web console](images/screenshot-aws-sso-web-console-api-credentials.png)

When my token expires, I go back to the web console and get new credentials.

Painful.

I want to keep my operating environment in shell.

I found this aws2-wrap project, https://github.com/linaro-its/aws2-wrap.

It sets up my shell environment with the AWS SSO credentials.

I can run terraform.

Noice noice.

I fixed up alias shortcuts for my shell.

``` shell
alias aws-sso-login-abc123="aws sso login --profile=abc123"

alias aws-sso-env-abc123="eval \"$(aws2-wrap --profile abc123 --export)\""
```

Noice noice noice.

When i want to work with AWS account abc123, i run,

``` shell
aws-sso-login-abc123
```

It will open a web browser and prompts me to sign in to my AWS account.

After i signed in, i run,

``` shell
aws-sso-env-abc123
```

This will export the credentials as environment variables.

Let's try running `terraform plan` again.

``` shell
$ terraform plan

Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

...
...
...
...
...

No changes. Infrastructure is up-to-date.

This means that Terraform did not detect any differences between your
configuration and real physical resources that exist. As a result, no
actions need to be performed.
```

Much better workflow.
