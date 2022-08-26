# Monk & Solr

This repository contains Monk.io template to deploy Solr system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).


## Start

Set up Monk - https://docs.monk.io/docs/monk-in-10/

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk solr repository

In order to load templates and change configuration simply use below commands: 
```bash
git clone https://github.com/kaganmersin/monk-solr

# and change directory to the monk-solr template folder
cd monk-solr
```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `stack.yaml/variables` section

```yaml
  variables:
    solr-image-tag: "latest"
```

### Solr configuration file

You can find configuration file (solr.xml) in `/files` directory in repository and can edit before the running kit.


| Configuration File	 | Format Used | Directory in Container | Purpose 
|----------|-------------|------|---------|
| **solr.xml** | XML | `/opt/solr/server/solr/solr.xml` | The solr.xml file defines some global configuration options that apply to all or many cores.


##  Template variables

| Variable | Description | Type | Example |
|----------|-------------|------|---------|
| **solr-image-tag** | Solr image version. | string | latest |




## Local Deployment

First clone the repository and simply run below command after launching `monkd`:
:

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 solr/solr
 ├─🔗 Process groups:
 │  └─🧩 solr/stack
 └─⚙️ Entity instances:
    └─🧩 solr/solr/metadata
✔ All templates loaded successfully

➜  monk list solr

✔ Got the list
Type      Template    Repository  Version  Tags
runnable  solr/solr   local       -        self hosted, search platform,
group     solr/stack  local       -        -


➜  monk run solr/stack

✔ Started local/solr/stack

```

This will start the entire solr/stack. 


## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name solr
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p gcp -f <path/to/your-key.json>
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Cloud provider gcp
? Name of the new instance my-instance
? Tags (split by whitespace) solr
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
 │  └─🧩 solr/solr
 ├─🔗 Process groups:
 │  └─🧩 solr/stack
 └─⚙️ Entity instances:
    └─🧩 solr/solr/metadata
✔ All templates loaded successfully

➜  monk list solr

✔ Got the list
✔ Got the list
Type      Template    Repository  Version  Tags
runnable  solr/solr   local       -        self hosted, search platform,
group     solr/stack  local       -        -

➜  monk run solr/stack

✔ Started solr/stack

```

## Logs & Shell

```bash
# show Solr logs
➜  monk logs -l 1000 -f solr/solr

# access shell in the container running Solr
➜  monk shell solr/solr

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge -x solr/stack solr/solr

✔ solr/stack purged
✔ solr/solr purged

```