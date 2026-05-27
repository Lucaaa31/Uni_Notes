The ***Learning Rate*** determines the step length that the model do to find the minimum of the loss function. The problem is that, if the step is always the same, the model could not be able to find the exact point.
***Cosine Schedule*** try to solve this problem by making the learning rate change following the cosine curve.

How it works:
1. ***Linear Warmup:*** in the first steps the LR starts from 0 and quickly grows till the max value
2. ***Cosine decay:*** LR starts fall following the cosine curve. So, the fall is low at the beginning, then accelerate in the middle and, in the end, it slows again
3. ***Soft final:*** The lasts steps are very small, this is because the model performs micro-regularizations (fine-tuning) in order to stabilize at the minimum point