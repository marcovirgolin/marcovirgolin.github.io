# Time complexity for different machine learning algorithms

Let **D** be an **n** times **m** dimensional supervised learning dataset, with **n** the number of examples, and **m** the number of features.

### Naive Bayes Classifier
A (Gaussian) Naive Bayes classifier takes **O(nm)** to train. The prediction of a new sample takes **O(mc)**, with **c** the number of classes.

### Ordinary Least Squares
Training by ordinary least squares take **O(nm^2)**, while prediction for a new sample takes **O(m)**.

### Support Vector Machines
Training time complexity depends on the training set and on hyper-parameters, and takes between **O(n^2m)** and **O(n^3m)**. Let **v** be the number of support vectors obtained by training. Then prediction time for a new sample is **O(vm)**.

### Random Forest
Assuming trees are free to grow to maximum height **O(*log*n)**, training takes **O(tun*log*n)**, where **t** is the number of trees and **u** is the number of features considered for splitting. The prediction of a new sample takes **O(t*log*n)**.

### eXtreme Gradient Boosting ###
Training with XGBoost takes **O(tdx*log*n)**, where **t** is the number of trees, **d** is the height of the trees, and **x** is the number of non-missing entries in the training data. Prediction for a new sample takes **O(td)**.
