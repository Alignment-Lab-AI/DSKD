# Dual-Space Knowledge Distillation for Large Language Models

<small>Songming Zhang, Xue Zhang, Zengkui Sun, Yufeng Chen*, Jinan Xu</small>

<a href="https://arxiv.org/abs/2406.17328"><img src="https://img.shields.io/badge/Paper-arXiv:2406.17328-Green"></a>
<a href=#bibtex><img src="https://img.shields.io/badge/Paper-BibTex-yellow"></a>

Some of our code follows MiniLLM and [Distillm](https://github.com/jongwooko/distillm/tree/master).

## Data
The processed data used in our paper can be downloaded [here](https://drive.google.com/drive/folders/1ZUsNVgWevACV9D-AHVNi9C7PX_2itzb8?usp=sharing).

## Training
### SFT for teacher models
For Qwen1.5-1.8B (full fine-tuning), run:
```bash
bash scripts/gpt2/sft_teacher_qwen.sh
```

For LLaMA2-7B (LoRA), run:
```bash
bash scripts/tinyllama/sft_teacher_llama2.sh
```

For Mistral-7B (LoRA), run:
```bash
bash scripts/tinyllama/sft_teacher_mistral.sh
```

### SFT for student models
For GPT2-base (full fine-tuning), run:
```bash
bash scripts/gpt2/sft_gpt2_base.sh
```

For TinyLLaMA-1.1B (LoRA), run:
```bash
bash scripts/tinyllama/sft_tinyllama.sh
```

### KD for the Same Vocabulary
#### Vanilla KD framework
For GPT2-base, run:
```bash
bash scripts/gpt2/vanilla_kd_gpt2_base.sh
```

For TinyLLaMA-1.1B, run:
```bash
bash scripts/tinyllama/vanilla_kd_tinyllama.sh
```

You can change the distance functions (e.g., KL Divergence, Reverse KL Divergence, JS Divergence, etc.) using `KD_OBJ` in the above scripts.

#### Dual-Space KD framework
For GPT2-base, run:
```bash
bash scripts/gpt2/dskd_gpt2_base.sh
```

For TinyLLaMA-1.1B, run:
```bash
bash scripts/tinyllama/dskd_tinyllama.sh
```

Also, you can change the distance functions using `KD_OBJ` in the above scripts.

### KD for different vocabularies
#### Logits Alignment by Minimum Edit Distance ([paper](https://arxiv.org/abs/2401.10491), [original implementation](https://github.com/fanqiwan/FuseAI))
The original implementation in this [repo](https://github.com/fanqiwan/FuseAI) pre-processes the logit alignment before distillation, while we re-implement this method by faster calculating alignment during distillation in [code/criterions/min_edit_dis_kld.py](https://github.com/songmzhang/DSKD/blob/1fc215196ea473aab971eea3b765ade57bbfb21b/code/criterions/min_edit_dis_kld.py).

For GPT2-base, run:
```bash
bash scripts/gpt2/minedit_gpt2_base.sh
```

For TinyLLaMA-1.1B, run:
```bash
bash scripts/tinyllama/minedit_tinyllama.sh
```

#### Universal Logit Distillation ([paper](https://arxiv.org/abs/2402.12030), [original implementation](https://github.com/Nicolas-BZRD/llm-recipes))
We also re-implement this method in [code/criterions/universal_logit_distillation.py](https://github.com/songmzhang/DSKD/blob/1fc215196ea473aab971eea3b765ade57bbfb21b/code/criterions/universal_logit_distillation.py).

For GPT2-base, run:
```bash
bash scripts/gpt2/uld_gpt2_base.sh
```

For TinyLLaMA-1.1B, run:
```bash
bash scripts/tinyllama/uld_tinyllama.sh
```

#### Our Dual-Space KD with Cross-Model Attention (CMA)
For GPT2-base, run:
```bash
bash scripts/gpt2/dskd_cma_gpt2_base.sh
```

For TinyLLaMA-1.1B, run:
```bash
bash scripts/tinyllama/dskd_cma_tinyllama.sh
```

## Evaluation
### Evaluate Full Fine-tuning Checkpoints
```bash
bash scripts/gpt2/run_eval.sh ${CKPT_PATH} ${EVAL_BATCH_SIZE}
```

### Evaluate LoRA Fine-tuning Checkpoints
```bash
bash scripts/tinyllama/run_eval_lora.sh ${LORA_ADAPTER_PATH} ${EVAL_BATCH_SIZE}
```
Please note to change `MODEL_PATH` for different base models (TinyLLaMA, LLaMA2, Mistral).
