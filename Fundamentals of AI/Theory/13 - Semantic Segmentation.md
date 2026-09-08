Semantic Segmentation aims ot labeling **each pixel in the image with a category**
**label.** It does not differentiate different instances.

# Metrics
For quantify the performance of a semantic segmentation model, we need a **metric that measures the classification at pixel level.**

It is a Supervised Learning algorithm, so we will have a ground truth image that will be compared with the prediction of the model:
![[13 - Semantic Segmentation-1788786479635.webp]]
## Pixel Accuracy
It is the ratio between the correctly classified pixels and the total number of pixels:
$$
PA = \frac{TP + TN}{TP+TN+FP+FN}
$$
It has the problem that **it does not consider class imbalance**.

## Intersection over Union (IoU)
For each class, it measures the ratio between the intersection between prediction and ground truth, and the combined area of the prediction and the ground truth:
$$
IoU_c = \frac{TP_c}{TP_c + FP_c + FN_c}
$$
![[13 - Semantic Segmentation-1788786935707.webp]]

### Mean IoU
Measures the average IoU among all classes:
$$
mIoU = \sum_{c=1}^C IoU_c
$$
![[13 - Semantic Segmentation-1788787078141.webp]]

# Semantic Segmentation end-to-end with CNNs
## Sliding Window
Watching a single pixel of an image is not enough to understand what it is:
![[13 - Semantic Segmentation-1788787187725.webp]]

Because of this, semantic segmentation introduces the idea of sliding windows in order to **extract context**:
![[13 - Semantic Segmentation-1788787265126.webp]]

**Problem:** computationally inefficient because we do not reuse shared features between overlapping patches.

## CNNs
We want to use a CNN to directly output a semantic mask (dense prediction) from a input image of the same size:
![[13 - Semantic Segmentation-1788787559315.webp]]

### Workflow
1. We can start with a CNN and then modify the architecture to get dense predictions
2. Because of the formula$$W_o = \frac{W_1 - K + 2P}{S} + 1$$ we can keep invariate the size of the feature maps by choosing the right set of convolutional layers
3. Then, we remove the fully connected layer that flatten the feature map

This network is called **Fully Convolutional Network**.
![[13 - Semantic Segmentation-1788787901738.webp]]

### Output
Each position of the output tensor, that is called **logits**, can be interpreted as a $C$ dimensional vector of scores each associated with a semantic category.

Passing the logits through a **softmax** function yields probability-like values.
![[13 - Semantic Segmentation-1788788012762.webp]]
### Cross-Entropy Loss
The training of this model is similar to a **multi-class classification problem for each pixel** so, like in the classification, we can use a cross-entropy loss applied to each pixel:
$$
L_{CE} = -\sum_{i=1}^N \sum_{c=1}^C y_{i, c} \log(\tilde y_{i,c})
$$
where:
- $y_{i, c}$ is called **indicator function** and assumes 1 if $x_i$ correct label is $c$, 0 otherwise
- $\tilde y_{i,c}$ is the softmax

### Cross-entropy loss Variations
Other variations that deals better with imbalance classes can be:
- **Weighted Cross-entropy loss:** we add a weight to give more importance to less frequent classes
$$
L_{WCE} = -\sum_{i=1}^N \sum_{c=1}^C \textcolor{red}{\beta_c} y_{i, c} \log(\tilde y_{i,c})
$$
+ **Focal loss:** we add a terms that gives more importance to misclassified elements $$L_{Focal} = -\sum_{i=1}^N \sum_{c=1}^C \textcolor{red}{(1-\tilde y_{i,c})^\gamma} y_{i, c} \log(\tilde y_{i,c})$$
