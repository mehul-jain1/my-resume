---
layout: post
title:  "Docker Notes"
date:   2024-07-16 16:02:06 +0530
categories: docker development
---
We started using Makefiles instead of scripts for set up tasks that are outside
the scope of Docker or Docker Compose. A good Makefile can centralize commands for building,
running, and managing your development environment.

For handling secrets like Artifactory credentials or API keys, it's a good practice
to use Docker's built-in secret management.

[Managing sensitive data with Docker secrets](https://docs.docker.com/engine/swarm/secrets/)
explains this concept well.

Check [the make documentation](https://www.gnu.org/software/make/manual/html_node/index.html)
to learn about Makefiles. I highly suggest reading the documentation - particularly
the sections about recipes and variables. It's easy to get fooled into thinking
Makefiles behave like shell scripts, but in many ways they do not.

A well-structured `docker-compose.yml` is also key. Note that like most configuration
files, it's helpful to list everything in alphabetical order for readability.

When Docker is slow on macOS, the filesystem is usually the culprit.
[docker-sync](http://docker-sync.io/) is a good solution. There are some edge
cases and behaviors, so it's a trade-off, and you should hold off on using it
until you absolutely cannot live with the performance hit of using Docker on macOS.

I like to include [MailHog](https://github.com/mailhog/MailHog) or a similar tool in my docker compose
configurations to intercept outgoing mail from the application during
development.

### Docker Shortcomings

The biggest shortcoming of using Docker for development with complex applications is that
we often need to start up many services to get a working environment. For example,
you might need an authentication service and a primary application database running
in addition to the application you are actually working on.

There is not an easy way to combine `docker-compose.yml` files from multiple
repos and bring everything up all at once. You typically need to create a `docker-compose.yml`
inside the main application repo that pulls in the other dependencies as services.

This is annoying because you have to reference outside applications, but a way
to make this less annoying is to adopt a consistent directory structure for your projects.
If all projects are organized similarly, you can use relative paths in scripts and
configuration files, and they will work on anyone's local machine.

For example, you could structure your source code folder by grouping projects under a common namespace:

```
/users/mehul.jain/src/my-company
├── core-services
│   ├── auth-service
│   └── billing-service
├── web-apps
│   ├── main-app
│   └── admin-dashboard
```

With a consistent structure, from inside any project, you can reliably point to
`../core-services/auth-service`, for example. This makes it much easier to diagnose problems
on a co-worker's machine because all of the code will be in a known, consistent location.

### Alternatives to Docker (and development databases)

I think something worth looking into is running development containers on a cloud provider.
Give every developer a cloud sandbox and write some [Terraform](https://www.terraform.io/)
scripts to spin up (and easily tear down) an entire chunk of infrastructure
running everything they need.

I did some back-of-the-envelope pricing, and for a reasonable monthly cost per developer,
you can spin up a powerful development environment for 40 hours a week:

- Multiple small to medium virtual machines
- A managed cache service (like Redis)
- A managed database service
- Sufficient database storage for a full copy of the production data

Do developers spend more time and money fiddling with local dev environments than the cost of a cloud setup? Probably.

This seems to be the future trend - cloud-based development. 