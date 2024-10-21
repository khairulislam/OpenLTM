# OpenLTM (Open-Source Large Time-Series Models)

Large time-series models, pre-training datasets, adaptation techniques, and benchmarks.

> [!NOTE]
> OpenLTM is a open codebase intending to explore the design philosophy of large time-series models. It is not intended to be completely compatiable with official codebases and existing checkpoints. 
> We aim to provide a neat pipeline to develop and evaluate large time-series models, which covers three milestone applications: **supervised training**, **large-scale pre-training**, and **model adaptation**.

> For deep time series models and task-specific benchmarks, we strongly recommend [Time-Series-Library](https://github.com/thuml/Time-Series-Library) and this comprehensive [Suvery](https://arxiv.org/abs/2407.13278).

:triangular_flag_on_post: **News** (2024.10) We include four large time-series models, release pre-training logic, and provide scripts.

## What is LTM

LTM (**L**arge **T**ime-Series **M**odel) is a series of scalable deep model built on foundation backbones (e.g. Transformers) and large-scale pre-training, which will be applied to a variety of heterogenerous time series and diverse downstream tasks. For more information, please refer to [Large-Time-Series-Model](https://github.com/thuml/Large-Time-Series-Model).


## Model Checklist

- [x] **Moirai** - Unified Training of Universal Time Series Forecasting Transformers. [[ICML 2024]](https://arxiv.org/abs/2402.02592), [[Code]](https://github.com/SalesforceAIResearch/uni2ts)
- [x] **Moment** - MOMENT: A Family of Open Time-series Foundation Model. [[ICML 2024]](https://arxiv.org/abs/2402.03885), [[Code]](https://github.com/moment-timeseries-foundation-model/moment)
- [x] **Timer** - Timer: Generative Pre-trained Transformers Are Large Time Series Models. [[ICML 2024]](https://arxiv.org/abs/2402.02368), [[Code]](https://github.com/thuml/Large-Time-Series-Model)
- [x] **Timer-XL** - Timer-XL: Long-Context Transformer for Unified Time Series Forecasting. [[arxiv 2024]](https://arxiv.org/abs/2410.04803), [[Code]](https://github.com/thuml/Timer-XL)

> We will update the following models to the checklist after a comprehensive evaluation. Welcome to give your suggestion about any interesting works 🤗

- [ ] AutoTimes: Autoregressive Time Series Forecasters via Large Language Models. [[NeurIPS 2024]](https://arxiv.org/abs/2402.02370), [[Code]](https://github.com/thuml/AutoTimes)
- [ ] Chronos: Learning the Language of Time Series. [[arxiv 2024]](https://arxiv.org/abs/2403.07815), [[Code]](https://github.com/amazon-science/chronos-forecasting)
- [ ] Time-MoE: Billion-Scale Time Series Foundation Models With Mixture Of Experts. [[arxiv 2024]](https://arxiv.org/abs/2409.16040), [[Code]](https://github.com/Time-MoE/Time-MoE)
- [ ] A Decoder-Only Foundation Model for Time-Series Forecasting. [[arxiv]](https://arxiv.org/abs/2310.10688), [[Code]](https://github.com/google-research/timesfm)


## Usage

1. Install Python 3.10. For convenience, execute the following command.

```
pip install -r requirements.txt
```

1. Place downloaded data in the folder ```./dataset```. Here is a [dataset summary](./figures/datasets.png).

- For pre-training:
  * [UTSD (1 billiion scale)](https://huggingface.co/datasets/thuml/UTSD) for domain-universal pre-training. A larger version is on the way!
  * [ERA5-Familiy (40-year span, thousands of variables)](https://www.ecmwf.int/en/forecasts/dataset/ecmwf-reanalysis-v5) for domain-specific model (will soon be released).
- For supervised training / adaptation: Datasets from [TSLib](https://github.com/thuml/Time-Series-Library), accessible in [[Google Drive]](https://drive.google.com/drive/folders/13Cg1KYOlzM5C7K8gK8NfC-F3EYxkM3D2?usp=sharing), [[Baidu Drive]](https://pan.baidu.com/s/1r3KhGd0Q9PJIUZdfEYoymg?pwd=i9iy)

2. We provide pre-training and adaptation scripts under the folder `./scripts/`. You can conduct experiments using the following examples:

```
# Supervised training
# (a) one-for-one forecasting
bash ./scripts/supervised/forecast/moirai_ecl.sh
# (b) one-for-all (rolling) forecasting
bash ./scripts/supervised/rolling_forecast/timer_xl_ecl.sh

# Large-scale pre-training
# (a) pre-training on UTSD
bash ./scripts/pretrain/timer_xl_utsd.sh
# (b) pre-training on ERA5
bash ./scripts/pretrain/timer_xl_era5.sh

# Model adaptation
# (a) full-shot fine-tune
bash ./scripts/adaptation/full_shot/timer_xl_etth1.sh
# (b) few-shot fine-tune
bash ./scripts/adaptation/few_shot/timer_xl_etth1.sh
# (c) zero-shot generalization
bash ./scripts/adaptation/zero_shot/timer_xl_etth1.sh
```

3. Develop your large time-series model.

- Add the model file to the folder `./models`. You can follow the `./models/timer_xl.py`.
- Include the newly added model in the `Exp_Basic.model_dict` of  `./exp/exp_basic.py`.
- Create the corresponding scripts under the folder `./scripts`.

## Leaderboard of Large Time-Series Models

| Model Ranking | Univariate Forecasting                       | Multivariate Forecasting                         | Rolling Forecasting                             | Forecasting with Covariates                  | Variable Generalization                          | Zero-Shot Generalization                     |
| ------------- | -------------------------------------------- | ------------------------------------------------ | ----------------------------------------------- | -------------------------------------------- | ------------------------------------------------ | -------------------------------------------- |
| 🥇 1st         | [Timer-XL](https://arxiv.org/abs/2410.04803) | [Timer-XL](https://arxiv.org/abs/2410.04803)     | [AutoTimes](https://github.com/thuml/AutoTimes) | [Timer-XL](https://arxiv.org/abs/2410.04803) | [Timer-XL](https://arxiv.org/abs/2410.04803)     | [Timer-XL](https://arxiv.org/abs/2410.04803) |
| 🥈 2nd         | [Timer](https://arxiv.org/abs/2402.02368)    | [Moirai](https://arxiv.org/abs/2402.02592)     | [Timer-XL](https://arxiv.org/abs/2410.04803)    | [TimeXer](https://arxiv.org/abs/2402.19072)  | [iTransformer](https://arxiv.org/abs/2310.06625) | [Time-MoE](https://arxiv.org/abs/2409.16040) |
| 🥉 3rd         | [PatchTST](https://arxiv.org/abs/2211.14730) | [iTransformer](https://arxiv.org/abs/2310.06625) | [PatchTST](https://arxiv.org/abs/2211.14730)    | [iTransformer](https://arxiv.org/abs/2310.06625)     | [PatchTST](https://arxiv.org/abs/2211.14730)     | [Timer](https://arxiv.org/abs/2402.02368)    |

For the first four [forecasting tasks](./figures/forecasting.png), in addition to supervised training (current leaderboard), a LTM can also be evaluated on full-shot and few-shot tasks, depending on downstream data availability and whether or not a pre-trained model is used. For other two [generalization tasks](./figures/generalization.png), please see our [paper](https://arxiv.org/abs/2410.04803) for details.

> [!NOTE]
> We compare LTSMs currently implemented or to be implemented in this repository. Model rank is based on officially reported results. We expect to see more large models included in this leaderboard!

## Efficiency

We present a [theoretical proof](./figures/efficiency.png) of the computational complexity of Time-Series Transformers. See our [paper](https://arxiv.org/abs/2410.04803) for details.

> [!NOTE]
> LTMs are still small in scale compared to large models of other modalities. We prefer to include and implement models requiring affordable training resources as efficiently as possible (for example, using several RTX 4090s or a single A100 40G).

## Citation

If you find this repo helpful, please cite our paper. 

```
@inproceedings{liutimer,
  title={Timer: Generative Pre-trained Transformers Are Large Time Series Models},
  author={Liu, Yong and Zhang, Haoran and Li, Chenyu and Huang, Xiangdong and Wang, Jianmin and Long, Mingsheng},
  booktitle={Forty-first International Conference on Machine Learning}
}

@article{liu2024timer,
  title={Timer-XL: Long-Context Transformers for Unified Time Series Forecasting},
  author={Liu, Yong and Qin, Guo and Huang, Xiangdong and Wang, Jianmin and Long, Mingsheng},
  journal={arXiv preprint arXiv:2410.04803},
  year={2024}
}
```

## Acknowledgment

We appreciate the following GitHub repos a lot for their valuable code and efforts:
- Time-Series-Library (https://github.com/thuml/Time-Series-Library)
- Large-Time-Series-Model (https://github.com/thuml/Large-Time-Series-Model)
- AutoTimes (https://github.com/thuml/AutoTimes)

## Contributors

If you have any questions or want to use the code, feel free to contact:
* Yong Liu (liuyong21@mails.tsinghua.edu.cn)
* Guo Qin (qinguo24@mails.tsinghua.edu.cn)
