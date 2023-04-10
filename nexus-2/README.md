# Monk & Nexus

This repository contains Monk.io template to deploy Nexus 3 system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Start

Set up Monk - [https://docs.monk.io/docs/monk-in-10/](https://docs.monk.io/docs/monk-in-10/)

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk nexus repository

In order to load templates and change configuration simply use below commands:

```bash
git clone https://github.com/monk-io/monk-nexus

# and change directory to the monk-nexus/nexus-2 template folder
cd monk-nexus/nexus-2

```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `nexus/stack/variables` section

```yaml
  variables:
    nexus-image-tag: "latest"
```

## Template variables

| Variable            | Description          | Type   | Example |
| ------------------- | -------------------- | ------ | ------- |
| **nexus-image-tag** | Nexus image version. | string | latest  |

## Local Deployment

| First clone the repository and change the current directory to the /nexus-2 folder and simply run below command after launching `monkd`: |
| :--------------------------------------------------------------------------------------------------------------------------------------: |

```bash
➜  monk load MANIFEST
✔ Read files successfully
✔ Loaded nexus.yaml successfully

Loaded 2 runnables, 0 process groups, 0 services, 0 entities and 0 entity instances
✨ Loaded:
 └─🔩 Runnables:
    ├─🧩 nexus2/base
    └─🧩 nexus2/nexus
✔ All templates loaded successfully

➜  monk list nexus

✔ Got the list
Type      Template      Repository  Version  Tags
runnable  nexus2/base   local       -        Nexus Repository, Artifact Repository, Package Management, DevOps, Continuous Integration, Continuous Deployment, Open Source, Binary Management, Docker Registry, Maven Repository, Software Development, Cloud Computing, Scalability, Repository Management, Repository Health
runnable  nexus2/nexus  local       -        Nexus Repository, Artifact Repository, Package Management, DevOps, Continuous Integration, Continuous Deployment, Open Source, Binary Management, Docker Registry, Maven Repository, Software Development, Cloud Computing, Scalability, Repository Management, Repository Health
➜ monk run nexus/stack

✔ Starting the run job: local/nexus2/nexus... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ New container local-2f5b5561219050743a02ae052f-local-nexus2-nexus-nexus created DONE
✔ Started local/nexus2/nexus
🔩 templates/local/nexus2/nexus
 └─🧊 Peer local
    └─🔩 templates/local/nexus2/nexus
       └─📦 local-2f5b5561219050743a02ae052f-local-nexus2-nexus-nexus running
          ├─🧩 sonatype/nexus:latest
          └─💾 /var/lib/monkd/volumes/nexus2 -> /sonatype-work

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/nexus2/nexus - Inspect logs
        monk shell     local/nexus2/nexus - Connect to the container's shell
        monk do        local/nexus2/nexus/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

This will start the entire nexus/stack with a Nginx reverse proxy.

## Login to Nexus

To first login with admin user, use below credentials

Use following url; `http://<url>:<port>/nexus`

```bash
User: admin
Password: admin123
```

## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name nexus
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p gcp -f <path/to/your-key.json>
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Cloud provider gcp
? Name of the new instance my-instance
? Tags (split by whitespace) nexus
? Region europe-central2
? Zone europe-central2-a
? Instance type e2-medium
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for gcp is HDD (pd-standard). Would you like to change it? No
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on gcp... DONE
✔ Creating node: my-instance-1 DONE
✔ Initializing node: my-instance-1 DONE
✔ Creating node: my-instance-2 DONE
✔ Initializing node: my-instance-2 DONE
✔ Creating node: my-instance-3 DONE
✔ Initializing node: my-instance-3 DONE
✔ Connecting: my-instance-1 DONE
✔ Syncing peer: my-instance-1 DONE
✔ Connecting: my-instance-2 DONE
✔ Connecting: my-instance-3 DONE
✔ Syncing peer: my-instance-2 DONE
✔ Syncing peer: my-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

## Logs & Shell

```bash
# show Nexus logs
➜  monk logs -l 1000 -f nexus/nexus2

# access shell in the container running Nginx
➜  monk shell nexus/nexus2

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge -x nexus2/nexus

✔ nexus/stack purged
✔ nexus/nexus2 purged

```
