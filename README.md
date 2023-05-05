# Zero-Shot End-to-End Spoken Language Understanding via Cross-Modal Selective Self-Training
This paper considers the task of zero-shot, end-to-end spoken language understanding (SLU), where an end-to-end SLU system is trained without speech-semantics pairs. We propose new benchmarks (MiniPS2SLURP, VoxPopuli2SLUE) and demonstrate a baseline system using self-training. We then focus on the real-world setting of found speech, where a large automatic speech recognition (ASR) corpus is collected independently from natural language understanding (NLU) tasks. This corpus is likely dominated by speech whose text is not representative of the NLU task, resulting in imbalanced and noisy semantic pseudolabels. Hence, we propose \textit{cross-modal selective self-training} (CMSST), a sample-efficient adaptively filtered approach that uses a novel \textit{multi-view clustering-based sample selection} (MCSS) to mitigate imbalance and a novel \textit{cross-modal SelectiveNet} (CMSN) to reduce the impact of noise. We validate that CMSST achieves comparable performances of the baseline but uses significantly reduced sample size and training time. Ablation study shows that each component independently improves performance.
## Configure Enviroment
To configure enviroment, we should make sure that `CUDA>=11.1` and then run below commands,
```
conda env create -f env_speechbrain4.yaml
conda activate speechbrain4
```

## Build Data
All data applied in our project is publicly released. 
Please first build a `dataset` folder in anywhere. Then, we list our used four datasets and their organization in our project as below.

1. [SLUE-Voxpopuli](https://github.com/asappresearch/slue-toolkit/blob/main/README.md)

Please install the [slue-toolkit](https://github.com/asappresearch/slue-toolkit/blob/main/README.md) in the `dataset/slue` folder at first. 
Then under the `slue-toolkit` folder, please run  below
```
bash scripts/download_datasets.sh
```
After that, the organization of SLUE-Voxpopuli is `datasets/slue/slue-toolkit/data/slue-voxpopuli/`.

2. [Voxpopuli](https://github.com/facebookresearch/voxpopuli)

Please install the `voxpopuli.git` according to the instruction on [Voxpopuli](https://github.com/facebookresearch/voxpopuli).
Then, to get the Voxpoluli-English data, please run below
```
python -m voxpopuli.download_audios --root [ROOT] --subset asr
```
where `[ROOT]` is the folder to save your data. Plus, segment these audios and align them with transcripts via
```
python -m voxpopuli.get_asr_data --root [ROOT] --lang en
```
After that, the organization of Voxpopuli-English is `datasets/slue/voxpopuli/voxpopuli/slue-voxpopuli-full/transcribed_data/en/`.


3. [SLURP](https://github.com/pswietojanski/slurp)

Please first download the textual annotation from its `dataset/slurp/`.
For the corresponding acoustic data, you can download it according the instruction from its Github, or skip it, which will be downloaded by our project code.
It will be easier to have the same dataset organization to ours by our project code.

After that, the organization of SLURP is `datasets/SLURP/`， where four files (`test.jsonl`, `train_real.jsonl`, `train_synthetic.jsonl` and `devel.jsonl`) and three folders (`slurp_real/`, `slurp_split/` and `slurp_synth/`) are available.

4. [PeolpleSpeech](https://mlcommons.org/en/peoples-speech/)

It is the mini-set of PeopleSpeech.
We will release its mini-set of [PeolpleSpeech](https://mlcommons.org/en/peoples-speech/) after publication, please download it according to its [official]((https://mlcommons.org/en/peoples-speech/)) license.

After that, the organization of peoplespeech is `datasets/peoplespeech/`, where it has a folder `subset/` and a file `flac_train_manifest.jsonl`.

## Models on SLURP 

### Training
We will release its related code once our work is accepted. For now, we only release the data and evaluate code.

### Testing
For testing any results from SLUE-Voxpopuli dataset, we can run below,
```
project_path="/speechbrain/recipes/SLURP/Zero_shot_cross_modal/"
test_jsonal=${project_path}"processed_data/slue-voxpopuli/slue-voxpopuli_test_blind.csv"
function_path="/slurp_metrics/scripts/evaluation"
cd ${function_path}
predict_jsonal=${project_path}"results/298312/slue-voxpopuli/slue-voxpopuli_slue-voxpopuli-full/(nlu_)gb_inference.jsonl"
python evaluate_slue.py -g ${test_jsonal} -p ${predict_jsonal}
```
where each parameter of the testing command is the same as those used in *Testing of $\Theta^{A \shortrightarrow L, t} $, and ${\Theta}^{A \shortrightarrow L, t}$ on SLURP*.

**Important** Becuase the original implementation of [slurp evaluation toolkit](https://github.com/pswietojanski/slurp/tree/master/scripts/evaluation) does not consider SLUE-Voxpopuli.
Please add by our `eval/evalute_slue.py` to `scripts/evaluation/evaluate_slue.py` in the folder of installed [slurp evaluation toolkit](https://github.com/pswietojanski/slurp/tree/master/scripts/evaluation).

For more command of evaluating results on SLUE-Voxpopuli dataset, please refer to `script/evaluate_slue_T1_T2.sh`.



