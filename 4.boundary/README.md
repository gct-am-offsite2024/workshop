# Identity Based Access with HCP Boundary

![HCP Boundary](https://www.boundaryproject.io/_next/image?url=https%3A%2F%2Fwww.datocms-assets.com%2F58478%2F1664218843-boundary-illustration-option2-1.png&w=3840&q=75)

## What we're doing to do

- Introduction
- Concepts
- Configure Waypoint template for Boundary
- Create Boundary instance (using Waypoint)
- Add first target (ec2 — TCP)
  - Connect using CLI
- Add second target (ec2-no-auth — SSH)
  - Connect using CLI
- Add the key pair to Boundary (Manual)
- Deploy Self-Hosted worker
- Enable Session Recording

## Introduction

![Concepts](https://developer.hashicorp.com/_next/image?url=https%3A%2F%2Fcontent.hashicorp.com%2Fapi%2Fassets%3Fproduct%3Dhcp-docs%26version%3Drefs%252Fheads%252Fmain%26asset%3Dpublic%252Fimg%252Fdocs%252Fboundary%252Fboundary-overview.png%26width%3D3222%26height%3D1902&w=3840&q=75&dpl=dpl_EzR85hkQNwBU8MZ1Hhm2WJ9QcxzX)

Boundary provides access to applications and critical systems with fine-grained authorizations without managing credentials or exposing your network.

## Cheat-sheet

### Authenticating to Boundary from CLI

```bash
export ORG_ID=<org-id> # id is on org page
export BOUNDARY_AUTH_METHOD_ID=<auth-id> # id from auth method page)
export BOUNDARY_ADDR=<actual-boundary-address> # url from boundary console page
export BOUNDARY_ADMIN=admin

boundary authenticate
```

### List available targets

```bash
boundary targets list -recursive
```

### Fetch the SSH private key for the EC2 instance

- Log into the AWS Console
- Ensure region is set to eu-central-1 (Frankfurt) in the top right
- Search for "Parameter Store" in the search bar
- Find the key prefixed with your project number, e.g `/project-1/...`
- Save the context to a file, e.g. `/tmp/ec2key.pem` and chmod it: `chmod 400 /tmp/ec2key.pem`

### Accessing the `ec2` instance with SSH

Using the PEM file created above, connect to the EC2 instance using SSH:

```bash
ssh $IP -l ec2-user -i /tmp/ec2key.pem
```

### Accessing the `ec2` target with Boundary

List the targets and find the `ec2` target

Connect to the EC2 instance using Boundary:

```bash
boundary connect ssh -target-id=$TARGET_ID -- -l ec2-user -i /tmp/ec2key.pem
```

### Accessing the `ec2-no-auth` target with Boundary

List the targets and find the `ec2-no-auth` target

Connect to the EC2 instance using Boundary:

```bash
boundary connect ssh -target-id=$TARGET_ID
```

## Exercise

### Add our first targets

> [!NOTE]
> Things worth testing:
>
> Maximum amount of sessions
> Cancelling an active session
> Reviewing logs

### Deploy a self-managed worker

![Self-Managed-Worker](https://developer.hashicorp.com/_next/image?url=https%3A%2F%2Fcontent.hashicorp.com%2Fapi%2Fassets%3Fproduct%3Dtutorials%26version%3Dmain%26asset%3Dpublic%252Fimg%252Fboundary%252Fworkers-model.png%26width%3D1164%26height%3D696&w=3840&q=75&dpl=dpl_Aq1CAi1wW9F2MnuSJUQNjAXgoFU7)

### Access internal target via worker

Use the EC2 template you created in Waypoint and set using a public IP to false.

Add a new Boundary target for this private IP, and configure it with an ingress filter like you did for the `ec2-no-auth` target

## Bonus

### Integrate Boundary with Okta

- [Sign up for Okta account](https://www.okta.com/free-trial/customer-identity)
- [OIDC authentication with Okta](https://developer.hashicorp.com/boundary/tutorials/identity-management/oidc-okta)
- [Manage OIDC IdP groups](https://developer.hashicorp.com/boundary/tutorials/identity-management/oidc-idp-groups)

> [!IMPORTANT]
> Do not use the "Sign up to Okta" link in the Hashicorp Tutorials, it will result in an Auth0 tenant. Rather use the first link provided in the list above.