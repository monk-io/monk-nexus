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

# and change directory to the monk-nexus/nexus-3 template folder
cd monk-nexus/nexus-3

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

| First clone the repository and change the current directory to the /nexus-3 folder and simply run below command after launching `monkd`: |
| :--------------------------------------------------------------------------------------------------------------------------------------: |

```bash
➜  monk load MANIFEST

✔ Read files successfully
✔ Loaded nexus.yaml successfully

Loaded 2 runnables, 0 process groups, 0 services, 0 entities and 0 entity instances
✨ Loaded:
 └─🔩 Runnables:
    ├─🧩 nexus3/base
    └─🧩 nexus3/nexus
✔ All templates loaded successfully

➜  monk list nexus

✔ Got the list
Type      Template      Repository  Version  Tags
runnable  nexus3/base   local       -        Nexus Repository, Artifact Repository, Package Management, DevOps, Continuous Integration, Continuous Deployment, Open Source, Binary Management, Docker Registry, Maven Repository, Software Development, Cloud Computing, Scalability, Repository Management, Repository Health
runnable  nexus3/nexus  local       -        Nexus Repository, Artifact Repository, Package Management, DevOps, Continuous Integration, Continuous Deployment, Open Source, Binary Management, Docker Registry, Maven Repository, Software Development, Cloud Computing, Scalability, Repository Management, Repository Health

➜ monk run nexus3/nexus

✔ Starting the run job: local/nexus3/nexus... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% sonatype/nexus3:latest local
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ New container local-01b4a985afcbf1cfec293bdba1-local-nexus3-nexus-nexus created DONE
✔ Started local/nexus3/nexus
🔩 templates/local/nexus3/nexus
 └─🧊 Peer local
    └─🔩 templates/local/nexus3/nexus
       └─📦 local-01b4a985afcbf1cfec293bdba1-local-nexus3-nexus-nexus running
          ├─🧩 sonatype/nexus3:latest
          └─💾 /var/lib/monkd/volumes/nexus3 -> /nexus-data

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/nexus3/nexus - Inspect logs
        monk shell     local/nexus3/nexus - Connect to the container's shell
        monk do        local/nexus3/nexus/action_name - Run defined action (if exists)
💡 Check monk help for more!

```

This will start the entire nexus/stack with a Nginx reverse proxy.

## Login to Nexus

To first login with admin user, fetch admin password with below command

```bash
➜ monk exec nexus3/nexus cat /nexus-data/admin.password

✔ Connecting to shell started.
2ae61c29-8467-4679-88b3-74df31d1f4bfk
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

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 nexus3/nexus
 ├─🔗 Process groups:
 │  └─🧩 nexus/stack
 └─⚙️ Entity instances:
    └─🧩 nexus3/nexus/metadata
✔ All templates loaded successfully

➜  monk list nexus

✔ Got the list
Type      Template      Repository  Version  Tags
runnable  nexus3/nexus  local       -        repository
group     nexus/stack   local       -        -   -

➜ monk run nexus/stack


✔ Started local/nexus/stack
```

## Logs & Shell

```bash
# show Nexus logs
➜  monk logs -l 1000 -f nexus3/nexus

# access shell in the container running Nginx
➜  monk shell nexus3/nexus

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge -x nexus/stack nexus3/nexus

✔ nexus/stack purged
✔ nexus3/nexus purged

```
