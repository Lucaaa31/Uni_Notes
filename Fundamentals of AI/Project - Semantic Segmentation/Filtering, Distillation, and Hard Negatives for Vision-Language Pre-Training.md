## Experiments

### Experiments setup
***The training dataset*** used is a 2.1B caption subset of the LAION-5B dataset. Before the training the subset has been preprocessed by removing the Not Safe For Work images or with a watermark probability greater than the 50%. After the preprocess the dataset has 1.9B images.

***The architecture*** is a Contrastive Language-Image Pre-training (CLIP), that utilizes a Visual Transformer for the images and a Text Transformer for the captions. They have tried 3 different architecture: B/32, B/16 and L/14 where the numbers represents the input image patch size.
The ***optimizer*** used is **Adam** with a decoupled weight decay. 
All the ***hyperparameters*** are selected by training B/32 on a small scale setup.

The models are evaluated on a zero-shot benchmark of 29 tasks:
1. 17 of  image classification
2. 10 of cross-modal retrieval
3. 2 of visual question answering

### Zero-shot benchmarks
![[Filtering, Distillation, and Hard Negatives for Vision-Language Pre-Training-1779192525629.webp|500]]
When trained on LAION-CAT or LAION-2B, DiHT wins on 20/29 benchmark tasks.
Impressively, when trained on PMD, DiHT wins on 28/29 benchmarks tasks, usually with a very large margin.

## Few-shot linear probing 
The ideal scenario for using the zero-shot recognition models is to warm-start the task without training data and then improve the performance via few-shot learning as more data are seen. 
The problem is that, in practice, few-shot models performs worse than zero-shot model in the low-data regime.
In the paper is presented an alternative approach to do few-shot with prompt-based initialization. 

***Key idea:*** initialize the the classifier with the zero-shot text prompts for each class, but to also ensure that the final weights do not drift much from the prompt using projected gradient descent (PGD).
While few-shot models have been initialized with prompt priors in the past with naive $L_2$ penalties for weight to prevent catastrophic forgetting, these approaches do not improve performance and the model simply ignores the supervision.
$$
\min_{\|\mathbf{W}\|_2 \le \delta, \|\boldsymbol{b}\|_2 \le \delta_b} \sum_{i=1}^{n} \mathcal{L}_{\text{CE}} \left(y_i, \boldsymbol{x}_i^{\top} (\mathbf{W} + \mathbf{W}_0) + \boldsymbol{b}\right).
$$
We observe that our approach is able to bridge the gap between zero-shot and 1-shot classification, a common issue in prior linear probe evaluations.
Hyperparameters δ and δb for our approach ,and weight decay for the baseline approach of training linear probes from scratch are found using grid-search.
Note that compared to the baseline, our method performs substantially better at very low values of k and maintains the performance continuum from zero-shot to 1-shot, and so on. At large k values, both approaches perform similarly, since there are sufficient data samples to render the zero-shot initialization ineffective.