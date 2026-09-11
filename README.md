# MaP-WAM: Memory as Plans

### World-Action Modeling with Memory-Grounded Planning

[![Project Page](https://img.shields.io/badge/Project%20Page-2563EB.svg?logo=googlechrome&logoColor=white)](https://sizhezhao.github.io/projects/MaP-WAM/)
[![arXiv](https://img.shields.io/badge/arXiv-2609.11561-b31b1b.svg?logo=arxiv&logoColor=white)](https://arxiv.org/abs/2609.11561)
![Models](https://img.shields.io/badge/Models-Coming%20Soon-8C959F.svg?logo=huggingface&logoColor=white)

Sizhe Zhao<sup>1</sup>, [Haozhe Xie](https://haozhexie.com/about/)<sup>2</sup>,
[Weiyu Zhao](https://although-not-but.github.io/weiyu.github.io/)<sup>1</sup>,
Chenchu Zhang<sup>1</sup>, Huan Wang<sup>3</sup>,
[Chenyang Wang](https://wangchenyang.cn/)<sup>1</sup>, Qinglin Liu<sup>1</sup>,
[Shengping Zhang](https://homepage.hit.edu.cn/zhangshengping)<sup>1,4,&dagger;</sup>

<sup>1</sup>Harbin Institute of Technology &nbsp;
<sup>2</sup>Nanyang Technological University &nbsp;
<sup>3</sup>Shandong University<br />
<sup>4</sup>HIT (Weihai) Qingdao Research Institute &nbsp;
<sup>&dagger;</sup>Corresponding author

## Release Plan

- &#x2705; Repository created
- &#x2B1C; Training/Inference code
- &#x2B1C; Checkpoints

## Overview

Many long-horizon manipulation tasks are non-Markovian: the information needed
for the next action may have disappeared from the current observation. Existing
approaches often compress history into language, which can discard precise
visual evidence, or repeatedly process a growing visual window, which increases
latency and memory use.

**MaP-WAM** addresses this problem by treating memory as planning-time evidence.
It stores completed task segments as compact language-visual records, converts
the resulting episodic memory into a segment-level language and visual plan, and
executes that plan using a fixed-context **World-Action-Progress (WAP)** model.
Progress prediction, plan-observation alignment, and progress-gated transitions
close the loop between planning, execution, and memory updates.

## Method

MaP-WAM consists of three tightly connected components:

1. **Structured multimodal episodic memory.** Each completed segment is stored
   as its language instruction together with sparse visual evidence sampled from
   the real execution trajectory.
2. **Memory-grounded planning.** A vision-language model predicts the next
   segment-level language plan, and a causal world model generates corresponding
   visual guidance from the long-term episodic context.
3. **Plan-conditioned execution.** WAP jointly predicts action chunks and
   execution progress. Plan-observation alignment calibrates recursive progress
   estimates, while progress-gated transitions determine when to update memory
   and request the next plan.

The structured attention design makes completed episodic evidence and the
current plan cacheable. As a result, the executor operates with a fixed context
length even as the task history grows.

## Highlights

- Preserves fine-grained historical evidence using sparse visual memory rather
  than language-only summaries.
- Separates long-horizon reasoning from short-horizon control through
  memory-grounded planning and plan-conditioned execution.
- Models task progress as a first-class modality jointly with actions and uses
  visual-plan alignment to reduce long-horizon drift.
- Supports key-value caching in both planning and execution through structured
  causal attention.
- Maintains approximately constant per-chunk executor latency as history grows.

## Results

| Setting | Evaluation protocol | Success rate |
| --- | --- | ---: |
| RMBench | 9 memory-dependent tasks | **83.3%** |
| Real robot | 2 memory-dependent tasks | **78.0%** |

On RMBench, MaP-WAM achieves an average success rate of **83.3%**, compared with
**77.1%** for the strongest baseline. On a 7-DoF Franka Research 3
robot, it achieves an average success rate of **78.0%** across two memory-dependent tasks.

See the [project page](https://sizhezhao.github.io/projects/MaP-WAM/) for the per-task comparison and real-robot demonstrations.

## Citation

```bibtex
@article{mapwam,
  author  = {Sizhe Zhao and Haozhe Xie and Weiyu Zhao and Chenchu Zhang and
             Huan Wang and Chenyang Wang and Qinglin Liu and Shengping Zhang},
  title   = {{Memory as Plans:} World-Action Modeling with Memory-Grounded Planning},
  journal = {arXiv 2609.11561},
  year    = {2026}
}
```
