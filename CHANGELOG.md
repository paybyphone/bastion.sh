## v0.2.1

Added the `upload` and `download` commands - these commands allow one to easily
upload or download files to and from the bastion, or to and from hosts behind
the bastion.

## v0.2.0

This version requires Terraform v0.8.0 or higher - the tool will not work
without it. If you have v0.7.x, use the v0.1.0 release.

The script now supports a custom AMI on launch. Use the `--ami-id` and
`--bastion-user` options to pick a custom AMI and bastion login user upon
launch. `bastion ssh` is used as normal and the SSH user is saved in Terraform
state.

## v0.1.0

Initial release.
