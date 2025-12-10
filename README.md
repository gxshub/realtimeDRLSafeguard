# Realtime DRL Switch

This GitHub project codes a real-time HighwayEnv DRL environment and the scripts that run the evaluations 
for the three Research Questions (RQs) of our paper "Fault-Tolerant Design and Multi-Objective Model Checking for Real-Time Deep Reinforcement
Learning Systems" (under review).


## Instructions
### Training and Evaluating DRL Agents
Navigate to the folder `./scripts`.
Set `LD_LIBRARY_PATH` environment variable:
```shell
export LD_LIBRARY_PATH=$(cd .. && pwd)/venv/lib/python3.8/site-packages/nvidia/nvjitlink/lib:$LD_LIBRARY_PATH
```

Train an agent:
```shell
python training.py \
configs/HighwayEnv/env_continuous_v2.json \
configs/HighwayEnv/agents/sb3/ddpg_2.json \
--total-timesteps=1000 \
--processes=2 \
--trial-mode
```
where `env_continuous_v2.json` is the HighwayEnv environment configuration and
`ddpg_2.json` is the DDPG configuration, which are used in our case study.

Evaluate a trained agent:
```shell
python evaluation.py \
configs/HighwayEnv/env_continuous_v2.json \
configs/HighwayEnv/agents/sb3/ddpg_2.json \
out/highway_env_continuous_actions/stable_baselines3.ddpg.ddpg/2025-02-12_18_16_34/best_model.zip \
--test
```

To disable depreciation warnings by Gymnasium in the console, run the following command before the evaluation or inference.
```shell
 export PYTHONWARNINGS=ignore
```

### Pre-Trained DDPG Agent

A pre-trained DDPG agent is available in this Hugging Face model repository:
[https://huggingface.co/gxshf/highway_v0_ddpg](https://huggingface.co/gxshf/highway_v0_ddpg)


```shell
# Install the Hugging Face Hub library, which includes the CLI tool
pip install -U "huggingface_hub"
huggingface-cli download \
    <Model_ID> \
    --local-dir <Local_Target_Path>
```
where `<Model_ID>` is `gxshf/highway_v0_ddpg`.

### Generation of Simulation Data:

```shell
python experiment.py \
configs/HighwayEnv/env_continuous_v2.json \
-w configs/HighwayEnv/switch/switch_dc0.yaml \
-a configs/HighwayEnv/agents/sb3/ddpg.json \
-c out/highway_env_continuous_actions/stable_baselines3.ddpg.ddpg/2025-02-12_18_16_34/best_model.zip \
-s configs/HighwayEnv/controller/ttc_based.yaml \
-t
```
Pre-defined Shell scripts to run examples are:
[`run_exeperiment_1a.sh`](./scripts/run_experiment_1a.sh), 
[`run_exeperiment_1b.sh`](./scripts/run_experiment_1b.sh),
[`run_exeperiment_2a.sh`](./scripts/run_experiment_2a.sh),
[`run_exeperiment_2b.sh`](./scripts/run_experiment_2b.sh),
[`run_exeperiment_3.sh`](./scripts/run_experiment_3.sh)

### Pre-Generated Simulation Data

Simulation data used in the case study is available in the following Hugging Face dataset repository:
[https://huggingface.co/datasets/gxshf/realtime_drl_switch_simulation_data](https://huggingface.co/datasets/gxshf/realtime_drl_switch_simulation_data)

### Jupyter Notebooks
The Jupyter Notebook for our evaluations of RQ1 and RQ2 in our paper is `./scripts/case_study/Case_Study.ipynb`, 
and those for the tool evaluation (RQ3) are included in the folder `./scripts/tool_evaluation`.
Note that, to run the tool evaluation notebooks, MOPMC, Storm and PRISM must be installed first.
See the MOPMC GitHub repository for more information: [https://github.com/gxshub/mopmc](https://github.com/gxshub/mopmc).

