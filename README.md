bastion.sh
===========

Extreme AWS bastion hosting, with a little help from our friends:

 * [Terraform][1]
 * [aws-cli][2]
 * [ssh][3] and [curl][4].

PS: You need these tools to run this script. :)

## Synopsis

Bastion hosting is a popular concept when using Amazon EC2. It's a host that
exists outside of the main application stack that allows you to connect to the
actual instances that make up the application by bouncing off of it.

However, having a permanent bastion host, in the author's opinion, voilates the
very popular notion that if you have to log into to your instances in
production, then your automation has failed. Hence, infrastructure should not be
designed with bastion hosts in mind. But where does that leave someone when
designing and debugging infrastructure before it hits produciton? The ability to
log into an instance is pretty invaluable.

This script is designed to assist with this - it gives one the ability to
create, on the fly, non-invasive bastion infrastructure that exists outside of
an individual application stack, and tear it down when it's no longer needed.
No modification of existing infrastructure is necessary.

A few ways you can use bastion-on-demand tooling:

 * More rapid development of application infrastructure - troubleshoot apps in
   AWS directly on the instances with less manual cleanup after the fact.
 * Speed up bootstrapping of your logging tooling so that SSH is no longer
   necessary.
 * Assist with emergency triage of production infrastructure without worrying
   about breaking configuration.

## Installation

Just run:

```
curl https://raw.githubusercontent.com/paybyphone/bastion.sh/master/bastion > /usr/local/bastion
chmod +x /usr/local/bastion
```

You also need the tools mentioned in the intro, and bash 4.x or higher. Note
that Terraform 0.7+ is also required!

## Usage

**Note**: Single-subnet VPC topologies are not supported currently.

```
    Usage: bastion OPTIONS COMMAND

    Options are:

      --private-subnet-id ID      The subnet ID for the private subnet.
      --public-subnet-id ID       The subnet ID for the public subnet.
      --security-group-id ID      The security group ID to allow the bastion
                                  host on.
      --ami-id ID                 Supply a custom AMI to use for the bastion
                                  host. Required when using --bastion-user.
      --bastion-user USER         Supply a custom login user for logging in to
                                  the bastion host. Required when using
                                  --ami-id.

    COMMAND is one of the following:

      launch                      Launch the bastion host and create the rules
      ssh [USER@HOST]             SSH to a host behind the bastion.
                                  If USER@HOST is blank, SSH to the bastion
                                  itself.
      terminate                   Terminates the bastion host, and removes
                                  rules.
```

Direct SSH connections to hosts behind the bastion are supported thru the use of
`ProxyCommand`. Make sure you set up the instances with a specific key pair, or
have deployed the instances with pre-loaded SSH keys.

Your public IP address is pulled down via https://www.ipify.org/.

## License

```
Copyright 2016 PayByPhone Technologies, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[1]: https://terraform.io
[2]: https://github.com/aws/aws-cli
[3]: http://www.openssh.com/
[4]: https://curl.haxx.se/

