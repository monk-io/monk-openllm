# OpenLLM & Monk

This repository contains Monk.io template to deploy OpenLLM platform.

## Prerequisites

- Install Monk
- Register and Login Monk
- Add Cloud Provider
- Add Instance with GPU

## Clone Repository

```bash
git clone https://github.com/monk-io/monk-openllm
```

## Load Template

```bash
cd monk-openllm
monk load MANIFEST
```

## Deploy

```bash
$ monk run bentoml/openllm

✔ Starting the run job: local/bentoml/openllm... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% ghcr.io/bentoml/openllm:latest test-instance
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Runnable templates/local/bentoml/openllm connections graph updating DONE
✔ Runnable templates/local/bentoml/openllm connections graph has been updated DONE
✔ Runnable templates/local/bentoml/openllm services initialization DONE
✔ Runnable templates/local/bentoml/openllm services have been initialized DONE
✔ Host ports have been added to container 9fedc250bc0aa64e75200be50e32cd66--bentoml-openllm-server DONE
✔ New container 9fedc250bc0aa64e75200be50e32cd66--bentoml-openllm-server created DONE
✔ Container 9fedc250bc0aa64e75200be50e32cd66--bentoml-openllm-server network has been configured DONE
✔ Container 9fedc250bc0aa64e75200be50e32cd66--bentoml-openllm-server has been started DONE
✔ Started local/bentoml/openllm
🔩 templates/local/bentoml/openllm
 └─🧊 Peer test-instance
    └─🔩 templates/local/bentoml/openllm 
       └─📦 9fedc250bc0aa64e75200be50e32cd66--bentoml-openllm-server running
          ├─🧩 ghcr.io/bentoml/openllm:latest      
          └─🔌 open (public) TCP 34.32.245.208:3000 -> 3000

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/bentoml/openllm - Inspect logs
	monk shell     local/bentoml/openllm - Connect to the container's shell
	monk do        local/bentoml/openllm/action_name - Run defined action (if exists)
💡 Check monk help for more!

```

## Wait for the model to load and initialize
```bash
$ curl http://34.32.245.208:3000/healthz
```

## Check web gui

```
http://34.32.245.208:3000/
```

## Test generate API

```bash
$ curl -X 'POST' \
    'http://34.32.245.208:3000/v1/generate' \
    -H 'accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"prompt": "What are Large Language Models?"}'
```

## Variables

The variables are in `openllm.yaml` file. You can quickly setup by editing the values here.

model - The LLM name to be used for the deployment.

backend - Runtime to use for both serialisation/inference engine.

[List of supported models](https://github.com/bentoml/OpenLLM/tree/main?tab=readme-ov-file#-supported-models)

To use the GPU you need to use the vllm backend. PyTorch is also available (pt)

To run some models you may need to change entrypoint and specify additional parameters, for example --max-model-len.

Also for large models it is recommended to use SSD drives to speed up the loading of data onto the disk.

## Stop, remove and clean up workloads and templates

```bash
monk purge bentoml/stack
```
