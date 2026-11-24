# Multi-Scale Feature Distillation for Image Classification

[Full Report](src/DC_Report.pdf)

Work done under Design Credit project supervised by Dr. Pratik Mazumder, implementing multi-scale feature distillation from a finetuned EfficientNet-B0 teacher into a 266,833 parameter CBAM depthwise separable CNN trained from scratch on a 7 class uncontrolled condition potato leaf disease dataset (total 3,076 images, stratified 80/20 split).


## Results

Evaluated on a 616-image stratified hold-out set (50 epochs, identical split across all models). Parameter counts reflect inference size (excluding training only 1x1 projection heads).

| Model | Total Params | Trainable Params | Best Acc. (%) | Epoch-50 Acc. (%) | Macro F1 (Seed 42) |
| --- | ---: | ---: | ---: | ---: | ---: |
| Teacher (EfficientNet-B0) | 4,016,515 | 4,016,515 | 89.16 ± 0.31 | 87.99 ± 0.87 | 0.8587 |
| LCE-Net (reproduced) | 4,029,413 | 2,048,213 | 82.27 ± 0.45 | 81.92 ± 0.63 | 0.8232 |
| **Student (distilled)** | **266,833** | **266,833** | **81.17 ± 1.56** | **80.75 ± 1.73** | **0.8072** |
| Student (CE only) | 266,833 | 266,833 | 76.72 ± 0.77 | 75.84 ± 0.87 | 0.7499 |

- The distilled model gains 4.45 points best epoch accuracy (4.91 at epoch 50) over the identical CE only student, improving every seed.

- 15.1x fewer deployed parameters than EfficientNet-B0 and LCE-Net (7.7x fewer than LCE-Net's trainable parameters), retaining 91.0% of teacher best-epoch accuracy (91.8% at epoch 50) and sitting within 1.10 pp of reproduced LCE-Net.

- The distilled model uses cosine feature alignment at 28x28, 14x14, and 7x7 spatial scales via training-only 1x1 convolutions (zero inference overhead).

## Dataset

N. H. Shabrina et al., *Potato Leaf Disease Dataset in Uncontrolled Environment*, Mendeley Data, v1, 2023 (`doi:10.17632/ptz377bwb8.1`, CC BY 4.0).
Available on [Kaggle](https://www.kaggle.com/datasets/nirmalsankalana/potato-leaf-disease-dataset)
