# AI Tools Deep Dive: MLOps & Infrastructure

**Published: February 3, 2026**  
**Author: Vigilant Meme Team**  
**Series: [AI Tools Landscape 2026](ai-tools-landscape-2026.md)**

---

## 🧒 ELI5 — Explain Like I'm 5

> **What is MLOps?**  
> Imagine you baked an amazing cake at home (that's like training an AI model). But now you want to sell cakes to thousands of people every day! You need a big kitchen, workers, delivery trucks, and ways to make sure every cake is just as good. MLOps is all the stuff that helps you go from "one great cake" to "a cake factory that runs smoothly."

---

![Infrastructure and deployment concept](https://images.unsplash.com/photo-1551288049-bebda4e38f71?auto=format&fit=crop&w=1200&q=80)

*Image: The infrastructure powering AI at scale*

---

## Overview

MLOps covers the entire lifecycle: training models, versioning experiments, deploying to production, and monitoring performance. This guide covers tools for each stage, from experimentation to scale.

---

## The MLOps Landscape

```
┌─────────────────────────────────────────────────────────────┐
│                    MODEL DEVELOPMENT                         │
├─────────────────────────────────────────────────────────────┤
│  Experiment Tracking  │  Model Registries  │  Fine-tuning   │
│  W&B, MLflow         │  HF Hub, MLflow    │  HF, OpenAI    │
├─────────────────────────────────────────────────────────────┤
│                    DEPLOYMENT & SERVING                      │
├─────────────────────────────────────────────────────────────┤
│  Inference Servers   │  Serverless GPU   │  Edge Deployment │
│  vLLM, TGI, Triton  │  Modal, Replicate │  ONNX, CoreML   │
├─────────────────────────────────────────────────────────────┤
│                    INFRASTRUCTURE                            │
├─────────────────────────────────────────────────────────────┤
│  GPU Clouds          │  Containers       │  Orchestration   │
│  Lambda, RunPod     │  Docker, K8s      │  Ray, Anyscale  │
└─────────────────────────────────────────────────────────────┘
```

---

## Model Hubs & Discovery

### Hugging Face Hub

The GitHub of machine learning. Essential for any ML work.

| Feature | Details |
|---------|---------|
| **Models** | 500K+ models |
| **Datasets** | 100K+ datasets |
| **Spaces** | Demo applications |
| **Pricing** | Free tier, Pro $9/mo |

**Quick Start:**

```python
from transformers import AutoModel, AutoTokenizer

# Load any model from the Hub
model = AutoModel.from_pretrained("microsoft/DialoGPT-medium")
tokenizer = AutoTokenizer.from_pretrained("microsoft/DialoGPT-medium")

# Or use pipelines
from transformers import pipeline
classifier = pipeline("sentiment-analysis")
result = classifier("I love this product!")
```

**Key Features:**
- ✅ Largest model repository
- ✅ Version control for models
- ✅ Easy fine-tuning integrations
- ✅ Free model hosting (Inference API)
- ✅ Spaces for demos

🔗 [Hugging Face Hub](https://huggingface.co)

---

## Experiment Tracking

### Weights & Biases (W&B)

Industry standard for ML experiment tracking.

| Feature | Details |
|---------|---------|
| **Experiment tracking** | Metrics, hyperparameters, artifacts |
| **Visualization** | Interactive dashboards |
| **Collaboration** | Team features |
| **Pricing** | Free for individuals, Teams $50/user/mo |

**Quick Start:**

```python
import wandb

# Initialize
wandb.init(project="my-llm-project")

# Log metrics
wandb.log({
    "loss": 0.5,
    "accuracy": 0.85,
    "learning_rate": 0.001
})

# Log artifacts
wandb.log_artifact("model.pt", type="model")

# Finish
wandb.finish()
```

**Best for:** Research teams, experiment comparison, hyperparameter sweeps.

🔗 [Weights & Biases](https://wandb.ai)

---

### MLflow

Open-source ML lifecycle platform.

| Feature | Details |
|---------|---------|
| **Tracking** | Experiments, metrics, artifacts |
| **Registry** | Model versioning |
| **Deployment** | Serving models |
| **Pricing** | Free (open source) |

**Quick Start:**

```python
import mlflow

# Start experiment
mlflow.set_experiment("my-experiment")

with mlflow.start_run():
    # Log parameters
    mlflow.log_param("learning_rate", 0.001)
    
    # Log metrics
    mlflow.log_metric("accuracy", 0.92)
    
    # Log model
    mlflow.sklearn.log_model(model, "model")
```

**Best for:** Open-source preference, full lifecycle management.

🔗 [MLflow Documentation](https://mlflow.org/docs/latest/index.html)

---

## Fine-Tuning Platforms

### OpenAI Fine-Tuning

Fine-tune GPT models on your data.

```python
from openai import OpenAI
client = OpenAI()

# Upload training file
file = client.files.create(
    file=open("training_data.jsonl", "rb"),
    purpose="fine-tune"
)

# Create fine-tuning job
job = client.fine_tuning.jobs.create(
    training_file=file.id,
    model="gpt-4o-mini-2024-07-18"
)

# Check status
status = client.fine_tuning.jobs.retrieve(job.id)
```

**Training data format:**
```json
{"messages": [{"role": "system", "content": "You are helpful"}, {"role": "user", "content": "Hi"}, {"role": "assistant", "content": "Hello!"}]}
```

🔗 [OpenAI Fine-Tuning Guide](https://platform.openai.com/docs/guides/fine-tuning)

---

### Hugging Face Training

Fine-tune open-source models.

```python
from transformers import Trainer, TrainingArguments

training_args = TrainingArguments(
    output_dir="./results",
    num_train_epochs=3,
    per_device_train_batch_size=4,
    learning_rate=2e-5,
    logging_steps=100,
    save_strategy="epoch"
)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset
)

trainer.train()
```

**Popular techniques:**

| Technique | Memory | Quality | Speed |
|-----------|--------|---------|-------|
| Full fine-tune | High | Best | Slow |
| LoRA | Low | Good | Fast |
| QLoRA | Very Low | Good | Fast |
| Prefix tuning | Low | OK | Fast |

🔗 [Hugging Face Training](https://huggingface.co/docs/transformers/training)

---

## Inference Serving

### vLLM

High-throughput LLM serving. The production standard.

| Feature | Details |
|---------|---------|
| **Throughput** | 2-24x vs Transformers |
| **Memory** | PagedAttention (efficient) |
| **API** | OpenAI-compatible |
| **Pricing** | Free (open source) |

**Quick Start:**

```bash
# Install
pip install vllm

# Start server
python -m vllm.entrypoints.openai.api_server \
    --model meta-llama/Llama-3.1-8B-Instruct \
    --port 8000
```

```python
# Use like OpenAI
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1")

response = client.chat.completions.create(
    model="meta-llama/Llama-3.1-8B-Instruct",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

**Best for:** Production serving, high-throughput needs.

🔗 [vLLM Documentation](https://docs.vllm.ai)

---

### Text Generation Inference (TGI)

Hugging Face's inference server.

```bash
# Run with Docker
docker run --gpus all -p 8080:80 \
    ghcr.io/huggingface/text-generation-inference:latest \
    --model-id meta-llama/Llama-3.1-8B-Instruct
```

**Best for:** Hugging Face ecosystem, easy deployment.

🔗 [TGI Documentation](https://huggingface.co/docs/text-generation-inference)

---

### Ollama

Local LLM running made easy.

```bash
# Install (macOS)
brew install ollama

# Run a model
ollama run llama3.1

# Serve API
ollama serve
```

```python
# Use via API
import requests
response = requests.post("http://localhost:11434/api/generate", json={
    "model": "llama3.1",
    "prompt": "Hello!"
})
```

**Best for:** Local development, privacy, demos.

🔗 [Ollama](https://ollama.ai)

---

## Serverless GPU Platforms

### Modal

Serverless Python for ML. Pay per second.

```python
import modal

app = modal.App("my-app")

# Define GPU container
@app.function(gpu="A100")
def run_inference(prompt: str):
    from vllm import LLM
    llm = LLM(model="meta-llama/Llama-3.1-8B-Instruct")
    return llm.generate(prompt)

# Deploy as web endpoint
@app.function(gpu="A100")
@modal.web_endpoint()
def api(prompt: str):
    return run_inference(prompt)
```

**Pricing:** ~$0.0011/sec for A100 (40GB)

🔗 [Modal Documentation](https://modal.com/docs)

---

### Replicate

Run ML models via API. Simple pricing.

```python
import replicate

# Run any model
output = replicate.run(
    "meta/llama-3.1-8b-instruct",
    input={"prompt": "Hello, how are you?"}
)

# Stream output
for event in replicate.stream(
    "meta/llama-3.1-8b-instruct",
    input={"prompt": "Write a poem"}
):
    print(event, end="")
```

**Best for:** Quick deployment, trying models.

🔗 [Replicate](https://replicate.com)

---

### Together AI

Inference API for open models.

```python
from together import Together
client = Together()

response = client.chat.completions.create(
    model="meta-llama/Llama-3.1-8B-Instruct-Turbo",
    messages=[{"role": "user", "content": "Hello!"}]
)
```

**Pricing:** From $0.18/1M tokens (Llama 3.1 8B)

🔗 [Together AI](https://www.together.ai)

---

## GPU Cloud Providers

### Comparison Table

| Provider | A100 40GB/hr | H100/hr | Min billing | Best for |
|----------|--------------|---------|-------------|----------|
| **Lambda Labs** | $1.10 | $2.49 | Per second | Training |
| **RunPod** | $1.19 | $3.29 | Per minute | Flexibility |
| **Vast.ai** | ~$0.80 | ~$2.50 | Per hour | Budget |
| **AWS** | $4.10 | $6.98 | Per second | Enterprise |
| **GCP** | $3.67 | $5.67 | Per second | Enterprise |
| **Azure** | $3.67 | $5.12 | Per second | Enterprise |

### Spot/Preemptible Instances

Save 60-90% with interruptible instances:

| Provider | Savings | Reliability |
|----------|---------|-------------|
| AWS Spot | 60-90% | Low (can interrupt) |
| GCP Preemptible | 60-91% | Low |
| Lambda Labs On-Demand | Fixed pricing | High |

---

## Edge & Mobile Deployment

### ONNX Runtime

Cross-platform ML inference.

```python
import onnxruntime as ort

# Load model
session = ort.InferenceSession("model.onnx")

# Run inference
outputs = session.run(None, {"input": input_data})
```

**Optimization:**
```bash
# Convert and optimize
python -m onnxruntime.transformers.optimizer \
    --model model.onnx \
    --output optimized.onnx
```

🔗 [ONNX Runtime](https://onnxruntime.ai)

---

### llama.cpp

Run LLMs on CPU/edge devices.

```bash
# Build
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp && make

# Run
./main -m models/llama-3.1-8b.gguf -p "Hello" -n 100
```

**Quantization levels:**

| Quant | Size | Quality | Speed |
|-------|------|---------|-------|
| Q8_0 | ~8GB | Best | Slower |
| Q5_K_M | ~5GB | Good | Medium |
| Q4_K_M | ~4GB | OK | Faster |
| Q2_K | ~3GB | Lower | Fastest |

🔗 [llama.cpp](https://github.com/ggerganov/llama.cpp)

---

## Orchestration

### Ray

Distributed computing for ML.

```python
import ray

ray.init()

@ray.remote
def process_batch(data):
    return model.predict(data)

# Parallel processing
futures = [process_batch.remote(batch) for batch in batches]
results = ray.get(futures)
```

**Ray Serve for deployment:**

```python
from ray import serve

@serve.deployment
class LLMDeployment:
    def __init__(self):
        self.model = load_model()
    
    async def __call__(self, request):
        return self.model.generate(request.json()["prompt"])

serve.run(LLMDeployment.bind())
```

🔗 [Ray Documentation](https://docs.ray.io)

---

## Tool Selection Guide

### By Stage

| Stage | Tool | Why |
|-------|------|-----|
| Experimentation | W&B or MLflow | Track everything |
| Fine-tuning | HF Transformers | Best ecosystem |
| Local dev | Ollama | Easy setup |
| Production serving | vLLM | Performance |
| Serverless | Modal | Cost-effective |
| Edge | ONNX / llama.cpp | Optimization |

### By Team Size

| Team | Stack |
|------|-------|
| Solo/small | Ollama → Replicate → Modal |
| Startup | vLLM on RunPod → Lambda |
| Enterprise | vLLM on K8s → Cloud GPUs |

---

## Cost Optimization

### Training Costs

| Approach | Cost Reduction |
|----------|---------------|
| Spot instances | 60-90% |
| Mixed precision | 2x throughput |
| Gradient checkpointing | Fit larger batch |
| LoRA instead of full | 10-100x cheaper |

### Inference Costs

| Approach | Savings |
|----------|---------|
| Quantization | 2-4x cheaper |
| Batching | 5-10x throughput |
| Caching | Variable |
| Smaller models | Linear |

---

## Production Checklist

- [ ] **Model versioning** — Track what's deployed
- [ ] **Health checks** — Know when things break
- [ ] **Autoscaling** — Handle traffic spikes
- [ ] **Monitoring** — Latency, errors, costs
- [ ] **Rollback plan** — Quick recovery
- [ ] **Cost alerts** — No surprises

---

## Further Reading

- [Hugging Face Course](https://huggingface.co/learn)
- [MLOps Guide](https://ml-ops.org)
- [Full Stack Deep Learning](https://fullstackdeeplearning.com)

---

*Part of the [AI Tools Landscape 2026](ai-tools-landscape-2026.md) series.*
