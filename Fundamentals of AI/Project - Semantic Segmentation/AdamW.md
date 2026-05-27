The ***weight decay*** is a technique used in order to regularize the model, it is performed penalizing the heaviest weights by adding a sanction to their lost function.
In the classical optimizer, like SGD, this is performed with the $L_2$ regularization.
With Adam the $L_2$ crashes, so the weight decay was implemented inside the methods during the calculus.

AdamW uses the decouples weight decay, with the idea of decouple the decay  and apply it at the end of the step.