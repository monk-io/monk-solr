# Monk & Solr - zookeeper

This repository contains Monk.io template to deploy Solr system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

This template includes Zooekper with solr out of box.

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

# and change directory to the monk-solr/solr-cluster-with-zookeeper template folder
cd monk-solr/solr-cluster-with-zookeeper

```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `stack.yaml/variables` section

```yaml
  variables:
    solr-image-tag: "latest"
    zookeeper-image-tag: "3.6.2"
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
| **zookeeper-image-tag** | zookeeper image version. | string | 3.6.2 |





## Local Deployment

First clone the repository and simply run below command after launching `monkd`:
:

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  ├─🧩 solr-zookeeper/solr3
 │  ├─🧩 solr-zookeeper/solr2
 │  ├─🧩 solr-zookeeper/zoo2
 │  ├─🧩 solr-zookeeper/solr1
 │  ├─🧩 solr-zookeeper/zoo1
 │  └─🧩 solr-zookeeper/zoo3
 ├─🔗 Process groups:
 │  └─🧩 solr-zookeeper/stack
 └─⚙️ Entity instances:
    ├─🧩 solr-zookeeper/zoo3/metadata
    ├─🧩 solr-zookeeper/solr3/metadata
    ├─🧩 solr-zookeeper/solr1/metadata
    ├─🧩 solr-zookeeper/zoo2/metadata
    ├─🧩 solr-zookeeper/zoo1/metadata
    └─🧩 solr-zookeeper/solr2/metadata
✔ All templates loaded successfully

➜  monk list solr-zookeeper

✔ Got the list
Type      Template             Repository  Version  Tags
runnable  solr-zookeeper/solr1  local       -        self hosted, search platform,
runnable  solr-zookeeper/solr2  local       -        self hosted, search platform,
runnable  solr-zookeeper/solr3  local       -        self hosted, search platform,
group     solr-zookeeper/stack  local       -        -
runnable  solr-zookeeper/zoo1   local       -        configuration, services
runnable  solr-zookeeper/zoo2   local       -        configuration, services
runnable  solr-zookeeper/zoo3   local       -        configuration, services


➜  monk run solr-zookeeper/stack 

✔ Started solr-zookeeper/stack 

```

This will start the entire solr-zookeeper/stack. 


## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name solr-zookeeper
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
 │  ├─🧩 solr-zookeeper/solr3
 │  ├─🧩 solr-zookeeper/solr2
 │  ├─🧩 solr-zookeeper/zoo2
 │  ├─🧩 solr-zookeeper/solr1
 │  ├─🧩 solr-zookeeper/zoo1
 │  └─🧩 solr-zookeeper/zoo3
 ├─🔗 Process groups:
 │  └─🧩 solr-zookeeper/stack
 └─⚙️ Entity instances:
    ├─🧩 solr-zookeeper/zoo3/metadata
    ├─🧩 solr-zookeeper/solr3/metadata
    ├─🧩 solr-zookeeper/solr1/metadata
    ├─🧩 solr-zookeeper/zoo2/metadata
    ├─🧩 solr-zookeeper/zoo1/metadata
    └─🧩 solr-zookeeper/solr2/metadata
✔ All templates loaded successfully

➜  monk list solr-zookeeper

✔ Got the list
Type      Template             Repository  Version  Tags
runnable  solr-zookeeper/solr1  local       -        self hosted, search platform,
runnable  solr-zookeeper/solr2  local       -        self hosted, search platform,
runnable  solr-zookeeper/solr3  local       -        self hosted, search platform,
group     solr-zookeeper/stack  local       -        -
runnable  solr-zookeeper/zoo1   local       -        configuration, services
runnable  solr-zookeeper/zoo2   local       -        configuration, services
runnable  solr-zookeeper/zoo3   local       -        configuration, services


➜  monk run solr-zookeeper/stack 

✔ Started solr-zookeeper/stack 

```

## Logs & Shell

```bash
# show Solr logs
➜  monk logs -l 1000 -f solr-zookeeper/solr1 

➜  monk logs -l 1000 -f solr-zookeeper/solr2

➜  monk logs -l 1000 -f solr-zookeeper/solr3 

# show Zookeeper logs

➜  monk logs -l 1000 -f solr-zookeeper/zoo1 

➜  monk logs -l 1000 -f solr-zookeeper/zoo2 

➜  monk logs -l 1000 -f solr-zookeeper/zoo3 


# access shell in the container running Solr
➜  monk shell solr-zookeeper/solr1

➜  monk shell solr-zookeeper/solr2

➜  monk shell solr-zookeeper/solr3

# access shell in the container running Zookeper
➜  monk shell solr-zookeeper/zoo1

➜  monk shell solr-zookeeper/zoo2

➜  monk shell solr-zookeeper/zoo3

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge -x solr-zookeeper/stack solr-zookeeper/solr1 solr-zookeeper/solr2 solr-zookeeper/solr3 solr-zookeeper/zoo1 solr-zookeeper/zoo2 solr-zookeeper/zoo3

✔ solr-zookeeper/stack purged
✔ solr-zookeeper/solr1 purged
✔ solr-zookeeper/solr2 purged
✔ solr-zookeeper/solr3 purged
✔ solr-zookeeper/zoo1 purged
✔ solr-zookeeper/zoo2 purged
✔ solr-zookeeper/zoo3 purged

```