# gitreceive-next (alpha)

An SSH server made specifically for accepting git pushes that will trigger a receiver script to handle the push however you like. It supports key based authentication and soon authorization (ACLs). 

This is a more advanced, standalone version of [gitreceive](https://github.com/progrium/gitreceive). This project will eventually be renamed to `gitreceive` and the old project will be renamed to `gitreceive-classic`. 

## Using gitreceive

```
Usage:  ./gitreceived [options] <privatekey> <receiver>

  -k="/tmp/keys": path to named keys
  -p="22": port to listen on
  -r="/tmp/repos": path to repo cache
```

The named key path is a path that contains public keys as files using filename as the name of the key. This is generally the user/owner of the key. 

The receiver argument is a path to an executable that will handle the push. It will get a tar stream of the repo via stdin and the following arguments:

	receiver-script USER REPO KEYNAME FINGERPRINT

* USER is the virtual "system" user pushed to. This can be anything git, etc that was used in the push
* REPO is the repo name that was pushed to. It can also be anything and may include slashes (ie progrium/repo)
* KEYNAME is the name of the key used to authentication. This is often used for the name of the user
* FINGERPRINT is a standard public key fingerprint of the key used which can be used to do further auth

## Todo

* Proper handling of errors, and some kind of testing
* Runtime config (keys, acls, etc) stored in etcd for clustering
* ACLs (assign keyname access to URL patterns)
* Action routes (multiple receivers based on URL pushed to)

## The Future

This project is mostly just an SSH frontend -- the git specific code is pretty minimal. It would not be hard to re-implement [sshcommand](https://github.com/progrium/sshcommand) with this codebase. So it seems likely at some point this code could be generalized into a simple SSH frontend (`simplessh`) that would backend to shell scripts or other network services. Anything specific to git would be encapsulated into a module. Modules could also allow builtin backend service discovery, and more. Then `simplessh` would become a sort of Nginx for SSH.

## License
 
 BSD