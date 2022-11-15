# Monk & Nexus

This repository contains Monk.io template to deploy Nexus 3 system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).


## Start

Set up Monk - https://docs.monk.io/docs/monk-in-10/

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

##  Template variables

| Variable | Description | Type | Example |
|----------|-------------|------|---------|
| **nexus-image-tag** | Nexus image version. | string | latest |



## Local Deployment

First clone the repository and change the current directory to the /nexus-2 folder and simply run below command after launching `monkd`:
:

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 nexus/nexus2
 ├─🔗 Process groups:
 │  └─🧩 nexus/stack
 └─⚙️ Entity instances:
    └─🧩 nexus/nexus2/metadata
✔ All templates loaded successfully

➜  monk list nexus

✔ Got the list
Type      Template      Repository  Version  Tags
runnable  nexus/nexus2  local       -        repository
group     nexus/stack   local       -        -   -

➜ monk run nexus/stack

✔ Started local/nexus/stack

```

This will start the entire nexus/stack with a Nginx reverse proxy. 

## Login to Nexus

To first login with admin user, use below credentials

Use following url; ` http://<url>:<port>/nexus `

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

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).
```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 nexus/nexus2
 ├─🔗 Process groups:
 │  └─🧩 nexus/stack
 └─⚙️ Entity instances:
    └─🧩 nexus/nexus2/metadata
✔ All templates loaded successfully

➜  monk list nexus

✔ Got the list
Type      Template      Repository  Version  Tags
runnable  nexus/nexus2  local       -        repository
group     nexus/stack   local       -        -   -

➜  monk run nexus/stack

✔ Started local/nexus/stack
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
➜ monk purge -x nexus/stack nexus/nexus2 

✔ nexus/stack purged
✔ nexus/nexus2 purged

```