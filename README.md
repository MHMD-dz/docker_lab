# Cloud Security Learning Lab

This repository is part of my journey into cloud security, DevOps, and infrastructure. It is hosted on a virtual machine in Microsoft Azure and will grow over time as I practice, experiment, and add new projects.

This is a record of hands-on work and progress—not a step-by-step tutorial. Each project captures something I am learning about containers, services, networking, access control, deployment, and secure operations in the cloud.

## Current projects

### Project 1 — Static school website

A static school website packaged and served by Nginx inside a Docker container.

- Docker image based on the official `nginx` image
- Static files copied into Nginx's web root
- Port `80` exposed by the container
- Deployed from an Azure VM and accessed through the VM's public IP when the appropriate Azure network rule is enabled

See [project-one/README.md](./project-one/README.md) for the project-specific commands and details.

### Project 2 — PostgreSQL practice environment

A PostgreSQL container used to practice databases, SQL, PL/pgSQL, users, roles, privileges, and persistent storage.

- PostgreSQL `17`
- Named Docker volume for persistence
- Database named `practice`
- Port `5432` bound to `127.0.0.1` so the database is not publicly exposed by default

See [project-two/README.md](./project-two/README.md) for the project-specific setup and commands.

## What I am practicing

- Building and running Docker containers
- Deploying workloads on an Azure virtual machine
- Understanding ports, firewalls, and Azure network security rules
- Applying least-exposure principles to services
- Managing persistent container data
- Practicing database access control and privileges
- Improving operational habits through repeatable commands and documentation

