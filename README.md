# 

... is a simple resource sharing platform focused on simplicity, programmability and security.  
Users in need of computation or storage power can craete computation pools. Each pool contains one or more job that have to be completed.  
A user that has excess computation power can join a pool (`script join pool POOL_ID`) and earn TOKENs for it.
Built on top an orchestration tool 

Example pools:
- GPT demo, a simple web interface built on top of open source language models e.g. mistral-7b, llama3, etc.
`MNEMONIC="" script pool new --template gpu --model "mistralai/Mistral-7B-v0.1"` 
- residential proxy server 
`script pool new --template proxy --per-user 1` 

These two represent two different scenarios, in terms or resources needed.

Users can also define their custom jobs using our JavaScript and Python SDK
``` python
from sdk import define_init, create_job, config

def custom_init:
  from transformers import AutoTokenizer, AutoModelForTokenClassification
  from transformers import pipeline

  tokenizer = AutoTokenizer.from_pretrained("dslim/bert-base-NER")
  model = AutoModelForTokenClassification.from_pretrained("dslim/bert-base-NER")
  nlp = pipeline("ner", model=model, tokenizer=tokenizer)

  config.model = model
  config.tokenizer = tokenizer
  config.runner = nlp

def custom_job(input):
  example = "My name is Wolfgang and I live in Berlin"

  return nlp(example)

define_init(custom_init)
create_job(custom_job)

```

## How it works

Nodes:
- Queen node
- Provider node
- King node

### Queen node

A queen provider creates one or more "listings".  

### Provider node

A provider node builds on a containerized micro-service architecture. These can optionally take on hold of hardware, for example when running an ML model, the weights are kept in the GPU to reduce latency.

### King node

The king node is the central node of the blockchain, the King Node, is responsible for finalizing transactions and adding them to the blockchain. It acts as the supreme authority, ensuring the security and finality of the transactions.

### Jobs

The jobs can be further split into two categories, performance dependent and performance independent. For example the when running an ML model we have to make sure that the whole model fits into RAM and VRAM and also that the GPUs can run the model quick enough i.e. has enough FLOPS capacity.
On the other hand when running a proxy job, the main criteria is not the performance, but the IP address.

### Container architecture

A provider node runs a container environment, which is made up of a main service and other micro-services. The main service is responsible for verifying the other jobs are completed.
This architecture is as safe as possible while allowing a user to have access to their computer.

## Lisk and Optimism

## Gelato integrations

We use Gelato Relay in our GPT demo, to let users interact with chatbot up to 100 prompts by sponsoring their gas needs.

## Examples

```bash
# Create a new pool
MNEMONIC="" script pool new --template gpu --model "GPT-2" --limit 10
MNEMONIC="" script pool new --template proxy --per-user 1 # The default is 1 per user

# A job is a script that runs
MNEMONIC="" script pool new --config ./mt-config.yml --job ./job.py

script evaluate # Goes through specs RAM, CPU, # of GPUs, etc.
MNEMONIC="" script pool join POOL_ID
```

```bash
# GPT demo

# Queen node creates a new pool
MNEMONIC="" script pool new --template gpu --model "GPT-2" --limit 10

# Provider node joins the pool, filling all the required jobs
script evaluate # Goes through specs RAM, CPU, # of GPUs, etc.
MNEMONIC="" script pool join POOL_ID -n 10

```

``` js
// In our demo implementation

// Authentication is implemented in the queen node
await usePool(poolId);
const resp = await fetch("{POOL_URL}", {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    prompt,
  })
});
const data = resp.json();
```

## CLI documentation

## Token


