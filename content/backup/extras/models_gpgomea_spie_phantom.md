# Machine learning for automatic construction of pseudo-realistic pediatric abdominal phantoms

## Examples of models found by GP-GOMEA

Examples of the mathematical expression models found by GP-GOMEA for the automatic construction of pediatric abdominal phantoms are reported below.
Each model pertains to a particular metric, and has been manually re-arranged to aid readability. 
The features and the target metrics are normalized (as described in Sec. 2.3.1 of the article).
To apply the model in practice, each feature needs to be de-normalized, i.e., scaled by the standard deviation and translated by the mean. Furthermore, the output of the model needs to be de-normalized as well, i.e., scaled and translated by the standard deviation and the mean of the target variable, respectively (this information is available in Table 1 of the article).
We do not include these de-normalization coefficients in the models for the sake of readability. Also, since normalized features are approximately in the same scale, the magnitude of the coefficients included in the model can be associated with feature relevance.


## The models
For the meaning of the abbreviations, see Table 1 in the article.

* **Body S** : <code> 0.420 * (ADAP + ADLR + SCIS) </code>
* **Liver LR** : <code> (GEND + ADAP) * sqrt((0.100 + RDLR^2) / (8.264 + 0.100 * RDLR^2)) </code>
* **Liver AP** : <code> (0.169 * ADAP) / sqrt(0.100 + (AGE - HEIG)^2) </code>
* **Liver IS** : <code> 0.179 * ADLR + GEND * RDIS </code>
* **Liver S** : <code> (ADLR + SCIS) / sqrt(0.100 + (4.389 + LDLR)^2) </code>
* **Spleen LR** : <code> -0.821 * LDLR / sqrt(LDLR^2 + 0.100 * ICSC^2 + 0.010) </code>
* **Spleen AP** : <code> -0.285 * ICSC / sqrt(0.100 + (AGE + ADAP)^2) </code>
* **Spleen IS** : <code> AGE / sqrt((0.100 + HEIG^2) * (9.673 - 6.188 * WEIG + WEIG^2)) </code>
* **Spleen S** : <code> 2.718^AGE * 0.057 * SCIS</code>


## Examples of interpretation
For the prediction of which body segmentation to retrieve (i.e., Body S), the model is particularly simple: it is the sum of the abdominal diameters (ADAP and ADLR) and of the spinal cord length (SCIS), scaled by a constant.
Interestingly, these features were already found to be among the most relevant in a study about the feasibility of modeling notions of anatomical similarity using random forest (see Virgolin et al. 2018). Arguably, the model is very reasonable: it predicts which body to retrieve based on its dimensions along the three axes: left-right (LR), anterior-posterior (AP), and inferior-superior (IS).

All the other models found by GP-GOMEA are non-linear. The recurrence of square roots (`sqrt`) and squaring terms (`^2`) are due to the adoption of the analytic quotient, to protect from diving by zero. 
For the sake of intuition, `x / sqrt(0.100 + y^2)` can be considered as approximately `x / |y|`, where `|.|` takes the absolute value.

As further examples, consider the models that predict the LR position of the liver and of the spleen. 
The model for liver LR will always return a positive number, whereas the one for the spleen will always return a negative number.
This is reasonable because the center of mass the liver is normally on the right of the reference point we used to determine the organ position, i.e., the 2nd lumbar vertebra, and the opposite holds for the spleen.
Moreover, for the prediction of the liver position along LR, the model combines the AP direction, by means of the abdominal diameter in AP (ADAP), and the LR direction, by means of the right diaphragm length along LR (RDLR). 
Note that, since GEND can either have value 0 (female) or 1 (male), the left multiplicand term is bigger for males than females. Thus, for males, the model predicts bigger shifts in LR. 
Differently from the liver, the LR position of the spleen relies on the LR size of the *left* diaphragm rather than on the *right* diaphragm. 
This aspect reflects the fact that the liver and spleen are respectively placed below the right and the left diaphragm.

