# stagerag

Retrieval-augmented answers can still contain unsupported claims. stagerag lets you query a JSONL knowledge base, choose between a shorter or longer generation pipeline, and inspect confidence scores before using an answer.

## Features

- Switch between a 3-step Speed pipeline and a 4-step Precision pipeline.
- Build vector indices from conversation-format JSONL data and retrieve context for queries.
- Score answers using retrieval quality, answer structure, relevance, and uncertainty detection.
- Run Llama 3.2 1B and 3B models with optional 4-bit quantization.
- Inspect cache statistics and test retrieval from the interactive demo.

## Installation

Use Python 3.8 or later. From the project root, create a virtual environment and install the dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
pip install -e .
```

On Windows, activate the environment with `.venv\Scripts\activate` instead.

Request access to both models on Hugging Face and accept their license terms:

- https://huggingface.co/meta-llama/Llama-3.2-1B-Instruct
- https://huggingface.co/meta-llama/Llama-3.2-3B-Instruct

Then authenticate with a Hugging Face token that has access to the models:

```bash
pip install huggingface-hub
huggingface-cli login
```

An internet connection is needed for the initial model download. A CUDA-capable GPU is recommended; the demo also accepts CPU execution. Model access and use are subject to the models' own license terms.

## Usage

Create `data/data.jsonl` with one conversation object per line. Each record should contain a question and its reference answer:

```jsonl
{"conversations": [{"role": "user", "content": "How do I request leave?"}, {"role": "assistant", "content": "Submit a leave request through the employee portal for manager approval."}]}
{"conversations": [{"role": "user", "content": "Where can I find my payslip?"}, {"role": "assistant", "content": "Open the Payroll section of the employee portal to download your payslip."}]}
```

Start the interactive demo on CPU:

```bash
python demo/interactive_demo.py --rag_dataset data/data.jsonl
```

For a CUDA GPU with 4-bit quantization:

```bash
python demo/interactive_demo.py --rag_dataset data/data.jsonl --use_4bit --device cuda
```

Inside the demo, enter a question or use these commands:

```text
mode speed
mode precision
search How do I request leave?
cache stats
quit
```

Use `mode speed` for the 3-step pipeline or `mode precision` for the 4-step pipeline. Actual latency depends on hardware and generation settings.

For a non-interactive example:

```bash
python demo/basic_usage.py --rag_dataset data/data.jsonl
```

**Confidence scores are signals, not guarantees of factual accuracy.** Check retrieved material and validate the scores against your own data before using them to accept or reject answers.

## License

stagerag is licensed under the MIT License and is based on [darrencxl0301/StageRAG](https://github.com/darrencxl0301/StageRAG). Retain the upstream copyright and MIT permission notice in copies or substantial portions of the software. See `LICENSE` for the full terms.
