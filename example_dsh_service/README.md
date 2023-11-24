# DSH SDK Example Service

This is an example service that uses the DSH SDK Platform and will demonstrate a simple consumer that prints the messages to the console. It also shows how to properly shutdown the service.

## Description

This example shows you how to setup:
- Consumer
- Graceful shutdown
- Build and publish to Harbor

## Build image

To build (and push) the docker image, open the makefile and update the following variables with your tenant information:
```
TENANT=
TENANTUID=
```

If not already done, login to Harbor (password is your Harbor client secret):
```bash
make login
```

Run the following command, to build and push the image to Harbor:

```bash
make all
```

See help for all avaialable options.
```bash
make help
```

## Dockerfile

The dockerfile is setup in such a way that it will build the service in a seperate stage and then copy the binary to a scratch image. This will result in a very small image. With this setup, there are no depencies on which environment the application is built.

If you need an extra depency during compile/build time, you can add it to the dockerfile in the build stage. If you need an extra dependency during runtime, you can add it to the dockerfile in the runner stage.

## Run the service on dsh

To run the service on dsh, you can use the following command (replace tenant_name and topic_name with your own values):

```
{
  "name": "dsh-sdk-example",
  "image": "registry.cp.kpn-dsh.com/{tenant_name}/dsh-sdk-example:example",
  "cpus": 0.1,
  "mem": 64,
  "env": {
    "TOPICS": "{topic_name}"
  },
  "instances": 0,
  "singleInstance": false,
  "needsToken": true,
  "user": "1903:1903"
}
```