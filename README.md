<h1 align="center"> WorFBench </h1>
<h3 align="center"> BENCHMARKING AGENTIC WORKFLOW GENERATION </h3>

<!-- <p align="center">
  <a href="https://arxiv.org/abs/2401.05268">📄arXiv</a> •
  <a href="https://huggingface.co/papers/2401.05268">🤗HFPaper</a> •
  <a href="https://www.zjukg.org/project/AutoAct/">🌐Web</a>
</p>

[![Awesome](https://awesome.re/badge.svg)](https://github.com/zjunlp/AutoAct) 
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
![](https://img.shields.io/github/last-commit/zjunlp/AutoAct?color=green)  -->

## Table of Contents

- 🌟[Overview](#🌟overview)
- 🔧[Installation](#🔧installation)
- ✏️[Model-Inference](#✏️model-inference)
- 📝[Workflow-Generation](#📝workflow-generation)
- 🤔[Workflow-Evaluation](#🤔workflow-evaluation)
<!-- - 🎉[Contributors](#🎉contributors) -->

---

## 🌟Overview

Large Language Models (LLMs), with their exceptional ability to handle a wide range of tasks, have driven significant advancements in tackling reasoning and planning tasks, wherein decomposing complex problems into executable workflows is a crucial step in this process. Existing workflow evaluation frameworks either focus solely on holistic performance or suffer from limitations such as restricted scenario coverage, simplistic workflow structures, and lax evaluation standards. To this end, we introduce WorFBench, a unified workflow generation benchmark with multi-faceted scenarios and intricate graph workflow structures. Additionally, we present WorFEval, a systemic evaluation protocol utilizing subsequence and subgraph matching algorithms to accurately quantify the LLM agent's workflow generation capabilities. Through comprehensive evaluations across different types of LLMs, we discover distinct gaps between the sequence planning capabilities and graph planning capabilities of LLM agents, with even GPT-4 exhibiting a gap of around 15%. We also train two open-source models and evaluate their generalization abilities on held-out tasks. Furthermore, we observe that the generated workflows can enhance downstream tasks, enabling them to achieve superior performance with less time during inference.




## 🔧Installation

```bash
git clone https://github.com/zjunlp/WorFBench
cd WorFBench
pip install -r requirements.txt
```



## ✏️Model-Inference

We use [llama-facotry](https://github.com/hiyouga/LLaMA-Factory) to deploy local model with OpenAI-style API
```bash
git clone --depth 1 https://github.com/hiyouga/LLaMA-Factory.git
cd LLaMA-Factory
pip install -e ".[torch,metrics]"
API_PORT=8000 llamafactory-cli api examples/inference/llama3_vllm.yaml
```




## 📝Workflow-Generation
Generate workflow with local llm api
```bash
tasks=(wikihow toolbench toolalpaca lumos alfworld webshop os)
model_name=your_model_name
for task in ${tasks[@]}; do
    python node_eval.py \
        --task gen_workflow \
        --model_name ${model_name} \
        --gold_path ./gold_traj/${task}/graph_eval.json \
        --pred_path ./pred_traj/${task}/${model_name}/graph_eval_two_shot.json\
        --task_type ${task} \
        --few_shot \

done
```

## 🤔Workflow-Evaluation
Evaluation the workflow in the mode of *node* or *graph*
```bash
tasks=(wikihow toolbench toolalpaca lumos alfworld webshop os)
model_name=your_model_name
for task in ${tasks[@]}; do
    Cpython node_eval.py \
        --task eval_workflow \
        --model_name ${model_name} \
        --gold_path ./gold_traj/${task}/graph_eval.json \
        --pred_path ./pred_traj/${task}/${model_name}/graph_eval_two_shot.json\
        --eval_model all-mpnet-base-v2 \
        --eval_output ./eval_result/${model_name}_${task}_graph_eval_two_shot.json \
        --eval_type node \
        --task_type ${task} \

done
```


<!-- ## 🎉Contributors

<a href="https://github.com/zjunlp/WorFBench/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=zjunlp/WorFBench" /></a>

We will offer long-term maintenance to fix bugs and solve issues. So if you have any problems, please put issues to us. -->