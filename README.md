# Rubix ML for PHP
[![PHP from Packagist](https://img.shields.io/packagist/php-v/rubix/ml.svg?style=for-the-badge)](https://www.php.net/) [![Latest Stable Version](https://img.shields.io/packagist/v/rubix/ml.svg?style=for-the-badge)](https://packagist.org/packages/rubix/ml) [![GitHub license](https://img.shields.io/github/license/andrewdalpino/Rubix.svg?style=for-the-badge)](https://github.com/andrewdalpino/Rubix/blob/master/LICENSE.md)


Rubix ML is a library that lets you build intelligent programs that learn from data in PHP.

## Our Mission
The goal of Rubix is to bring easy to use machine learning (ML) capabilities to the [PHP](https://php.net) language. Although PHP is best known for its web potential, we believe that PHP engineers should not be left out of the ML party. We aspire to provide the tooling to facilitate small to medium sized projects, rapid prototyping, and education.

## Installation
Install Rubix using composer:
```sh
$ composer require rubix/ml
```

## Requirements
- [PHP](https://php.net) CLI 7.1.3 or above
- [GD extension](https://php.net/manual/en/book.image.php) for Image Vectorization

## License
MIT

## Documentation

### Table of Contents

 - [Introduction](#an-introduction-to-machine-learning-in-rubix)
	 - [Obtaining Data](#obtaining-data)
	 - [Choosing an Estimator](#choosing-an-estimator)
	 - [Training and Prediction](#training-and-prediction)
	 - [Evaluation](#evaluating-model-performance)
	 - [Next Steps](#what-next)
- [API Reference](#api-reference)
	- [Dataset Objects](#dataset-objects)
		- [Labeled](#labeled)
		- [Unlabeled](#unlabeled)
	- [Feature Extractors](#feature-extractors)
    	- [Count Vectorizer](#count-vectorizer)
		- [Pixel Encoder](#pixel-encoder)
	- [Estimators](#estimators)
		- [Anomaly Detectors](#anomaly-detectors)
			- [Isolation Forest](#isolation-forest)
			- [Isolation Tree](#isolation-tree)
			- [Local Outlier Factor](#local-outlier-factor)
			- [Robust Z Score](#robust-z-score)
		- [Classifiers](#classifiers)
			- [AdaBoost](#adaboost)
			- [Committee Machine](#committee-machine)
			- [Decision Tree](#decision-tree)
			- [Dummy Classifier](#dummy-classifier)
			- [K Nearest Neighbors](#k-nearest-neighbors)
			- [Logistic Regression](#logistic-regression)
			- [Multi Layer Perceptron](#multi-layer-perceptron)
			- [Naive Bayes](#naive-bayes)
			- [Random Forest](#random-forest)
			- [Softmax Classifier](#softmax-classifier)
		- [Clusterers](#clusterers)
			- [DBSCAN](#dbscan)
			- [Fuzzy C Means](#fuzzy-c-means)
			- [K Means](#k-means)
			- [Mean Shift](#mean-shift)
		- [Regressors](#regressors)
			- [Dummy Regressor](#dummy-regressor)
			- [KNN Regressor](#knn-regressor)
			- [MLP Regressor](#mlp-regressor)
			- [Regression Tree](#regression-tree)
			- [Ridge](#ridge)
		- [Online](#online)
		- [Probabilistic](#probabilistic)
		- [Persistable](#persistable)
	- [Meta-Estimators](#meta-estimators)
	- [Neural Network](#neural-network)
		- [Activation Functions](#activation-functions)
			- [ELU](#elu)
			- [Hyperbolic Tangent](#hyperbolic-tangent)
			- [Identity](#identity)
			- [PReLU](#prelu)
			- [Sigmoid](#sigmoid)
		- [Layers](#layers)
			- [Input](#input)
			- [Hidden](#hidden)
				- [Dense](#dense)
			- [Output](#output)
				- [Continuous](#continuous)
				- [Logistic](#logistic)
				- [Softmax](#softmax)
		- [Optimizers](#optimizers)
			- [AdaGrad](#adagrad)
			- [Adam](#adam)
			- [Momentum](#momentum)
			- [RMS Prop](#rms-prop)
			- [Step Decay](#step-decay)
			- [Stochastic](#stochastic)
	- [Data Preprocessing](#data-preprocessing)
		- [Pipeline](#pipeline)
		- [Transformers](#transformers)
			- [Dense and Sparse Random Projectors](#dense-and-sparse-random-projectors)
			- [L1 and L2 Regularizers](#l1-and-l2-regularizers)
			- [Min Max Normalizer](#min-max-normalizer)
			- [Missing Data Imputer](#missing-data-imputer)
			- [Numeric String Converter](#numeric-string-converter)
			- [One Hot Encoder](#one-hot-encoder)
            - [Polynomial Expander](#polynomial-expander)
            - [Robust Standardizer](#robust-standardizer)
			- [TF-IDF Transformer](#tf---idf-transformer)
			- [Variance Threshold Filter](#variance-threshold-filter)
			- [Z Scale Standardizer](#z-scale-standardizer)
	- [Cross Validation](#cross-validation)
		- [Validators](#validators)
			- [Hold Out](#hold-out)
			- [K Fold](#k-fold)
			- [Leave P Out](#leave-p-out)
		- [Metrics](#validation-metrics)
			- [Classification](#classification)
			- [Regression](#regression)
			- [Clustering](#clustering)
		- [Reports](#reports)
			- [Aggregate Report](#aggregate-report)
			- [Confusion Matrix](#confusion-matrix)
			- [Contingency Table](#contingency-table)
			- [MulticlassBreakdown](#multiclass-breakdown)
			- [Outlier Ratio](#outlier-ratio)
			- [Prediction Speed](#prediction-speed)
			- [Residual Analysis](#residual-analysis)
	- [Model Selection](#model-selection)
		- [Grid Search](#grid-search)
	- [Model Persistence](#model-persistence)
		- [Persistent Model](#persistent-model)

---
### An Introduction to Machine Learning in Rubix
Machine learning is the process by which a computer program is able to progressively improve performance on a certain task through training and data without explicitly being programmed. There are two types of learning techniques that Rubix offers out of the box, **Supervised** and **Unsupervised**.
 - **Supervised** learning is a technique to train computer models with a dataset in which the outcome of each sample data point has been *labeled* either by a human expert or another ML model prior to training. There are two types of supervised learning to consider in Rubix:
	 - **Classification** is the problem of identifying which *class* a particular sample belongs to among a set of categories. For example, one task may be in determining a particular species of Iris flower based on its sepal and petal dimensions.
	 - **Regression** involves predicting continuous *values* rather than discrete classes. An example in which a regression model is appropriate would be predicting the life expectancy of a population based on economic factors.
- **Unsupervised** learning, by contrast, uses an *unlabeled* dataset and instead relies on discovering information through just the features of the training samples.
	- **Clustering** is the process of grouping data points in such a way that members of the same group are more similar (homogeneous) than the rest of the samples. You can think of clustering as assigning a class label to an otherwise unlabeled sample. An example where clustering might be used is in differentiating tissues in PET scan images.
	- **Anomaly Detection** is the flagging of samples that do not conform to an expected pattern. Anomalous samples can often indicate adversarial activity, bad data, or exceptional performance.

### Obtaining Data
Machine learning projects typically begin with a question. For example, who of my friends are most likely to stay married to their spouse? One way to go about answering this question with machine learning would be to go out and ask a bunch of long-time married and divorced couples the same set of questions and then use that data to build a model of what a successful (or not) marriage looks like. Later, you can use that model to make predictions based on the answers from your friends.

Although this is certainly a valid way of obtaining data, in reality, chances are someone has already done the work of measuring the data for you and it is your job to find it, aggregate it, clean it, and otherwise make it usable by the machine learning algorithm. There are a number of PHP libraries out there that make extracting data from CSV, JSON, databases, and cloud services a whole lot easier, and we recommend checking them out before attempting it manually.

Having that said, Rubix will be able to handle any dataset as long as it can fit into one its predefined Dataset objects (Labeled, Unlabeled, etc.).

#### The Dataset Object
All of the machine learning algorithms (called *Estimators*) in Rubix require a Dataset object to train. Unlike standard PHP arrays, [Dataset objects](#dataset-objects) extend the basic data structure functionality with many useful features such as properly splitting, folding, and randomizing the data points.

For the following example, suppose that you went out and asked 100 couples (50 married and 50 divorced) to rate (between 1 and 5) their similarity, communication, and partner attractiveness. We can construct a [Labeled Dataset](#labeled) object from the data you collected in the following way:

```php
use \Rubix\ML\Datasets\Labeled;

$samples = [[3, 4, 2], [1, 5, 3], [4, 4, 3], [2, 1, 5], ...];

$labels = ['married', 'divorced', 'married', 'divorced', ...];

$dataset = new Labeled($samples, $labels);
```

### Choosing an Estimator

There are many different algorithms to chose from and each one is designed to handle specific (often overlapping) tasks. Choosing the right [Estimator](#estimators) for the job is crucial to building an accurate and performant computer model.

There are a couple ways that we could model our marriage satisfaction predictor. We could have asked a fourth question - that is, to rate each couple's overall marriage satisfaction and then train a [Regressor](#regressors) to predict a continuous "satisfaction score" for each new sample. But since all we have to go by for now is whether they are still married or currently divorced, a [Classifier](#classifiers) will be better suited.

In practice, one will experiment with more than one type of Classifier to find the best fit to the data, but for the purposes of this introduction we will simply demonstrate a common and intuitive algorithm called *K Nearest Neighbors*.

#### Creating the Estimator Instance

Like most Estimators, the [K Nearest Neighbors Classifier](#k-nearest-neighbors) requires a number of parameters (called *Hyperparameters*) to be chosen up front. These parameters can be chosen based on some prior knowledge of the problem space, or at random. Rubix provides a meta-Estimator called [Grid Search](#grid-search) that, given a list, searches the parameter space for the most effective combination. For the purposes of this example we will just go with our intuition and chose the parameters outright.

Here are the hyperparameters for K Nearest Neighbors:

| Param | Default | Type | Description |
|--|--|--|--|
| k | 5 | int | The number of neighboring training samples to consider when making a prediction. |
| kernel | Euclidean | object | The distance metric used to measure the distance between two sample points. |

The K Nearest Neighbors algorithm works by comparing the "distance" between a given sample and each of the training samples. It will then use the K nearest samples to base its prediction on. For example, if the 5 closest neighbors to a sample are 4 married and 1 divorced, the algorithm will output a prediction of married with a probability of 0.80.

It is important to understand the effect that each parameter has on the performance of the particular Estimator as different values can often lead to drastically different results.

To create a K Nearest Neighbors Classifier instance:
```php
use Rubix\ML\Classifiers\KNearestNeighbors;
use Rubix\ML\Kernels\Distance\Manhattan;

// Using the default parameters
$estimator = new KNearestNeighbors();

// Specifying parameters
$estimator = new KNearestNeighbors(3, new Manhattan());
```

### Training and Prediction
Now that we've chosen and instantiated an Estimator and our Dataset object is ready to go, it is time to train our model and use it to make some predictions.

Using the Labeled Dataset object we created earlier we can train the KNN estimator like such:
```php
$estimator->train($dataset);
```
For our 100 sample dataset, this will only take a few milliseconds, but larger datasets and more sophisticated Estimators can take much longer.

Once our Estimator has been trained we can feed in some new sample points to see what the model predicts. Suppose that we went out and collected  new data points from our friends using the same questions we asked the married and divorced couples. We could make a prediction on whether they look more like the class of love birds or the class of divorcees by taking their answers and doing the following:
```php
use Rubix\ML\Dataset\Unlabeled;

$samples = [[4, 1, 3], [2, 2, 1], [2, 4, 5], [5, 2, 4]];

$friends = new Unlabeled($samples);

$predictions = $estimator->predict($friends);

var_dump($predictions);
```
Outputs:
```sh
array(5) {
	[0] => 'divorced',
	[1] => 'divorced',
	[2] => 'married',
	[3] => 'married',
}
```
Note that we are not using a Labeled Dataset here because we don't know the outcomes yet. In fact, the outcome is exactly what we are trying to predict.

### Evaluating Model Performance
We can make predictions but how do we know that the predictions our model makes are accurate? The answer to that is called [Cross Validation](#cross-validation) and Rubix has a number of tools that can automate the process of evaluating a model. For the purposes of this introduction, we will use the simplest form of cross validation called [Holdout](#hold-out). The idea is to randomize and split the dataset into a training and testing set, such that a portion of the data is "held out" to be used to test the model.

We need to supply a validation metric to measure suited to our Classifier. In this case we will use the Accuracy metric, but more exist depending on your purpose.
```php
use Rubix\ML\CrossValidation\HoldOut;
use Rubix\ML\CrossValidation\Metrics\Accuracy;

$validator = new HoldOut(0.2);

$score = $validator->test($estimator, $dataset, new Accuracy());

var_dump($score);
```
##### Outputs:
```sh
float(0.945)
```
Since we are measuring accuracy, this output means that our Estimator is about 95% accurate given the data we've provided it. The parameter 0.2 instructs the [Validator](#validators) to use 20% of the dataset for testing. More data for testing means the result will have less variance however that also means we don't use as much data to train the model.

### What Next?
Now that we've gone through a brief introduction of a simple machine learning problem in Rubix, the next step is to become more familiar with the API and to experiment with some data on your own. We highly recommend reading the entire documentation, but if you're eager to get started with Rubix and are comfortable with machine learning a great place to get started is with one of the many datasets available for free on the University of California Irvine [Machine Learning repository](https://archive.ics.uci.edu/ml/datasets.html) website.

---
### API Reference
Here you will find information regarding the classes that make up the Rubix library.

### Dataset Objects
In Rubix, data is passed around using specialized data structures called Dataset objects. Dataset objects make it easy to slice and transport data in a canonical way.

Return the *first* n rows of data in a new Dataset object:
```php
public head(int $n = 10) : self
```

Return the *last* n rows of data in a new Dataset object:
```php
public tail(int $n = 10) : self
```

Remove n rows from the Dataset and return them in a new Dataset:
```php
public take(int $n = 1) : self
```

Leave n samples on the Dataset and return the rest in a new Dataset:
```php
public leave(int $n = 1) : self
```

Randomize the order of the Dataset:
```php
public randomize() : self
```

Split the Dataset into left and right subsets by a given ratio:
```php
public split(float $ratio = 0.5) : array
```

Fold the Dataset k - 1 times to form k equal size Datasets:
```php
public fold(int $k = 10) : array
```

Batch the Dataset into subsets of n rows per batch:
```php
public batch(int $n = 50) : array
```
Generate a random subset of size n:
```php
public randomSubset($n = 1) : self
```

Generate a random subset with replacement of size n:
```php
public randomSubsetWithReplacement($n = 1) : self
```

Combine an array of Dataset objects into one Dataset object:
```php
public static combine(array $datasets) : self
```

Return the 2-dimensional sample matrix:
```php
public samples() : array
```

##### Example:
```php
use Rubix\ML\Datasets\Labeled;

...
$dataset = new Labeled($samples, $labels);

// Return just the first 5 rows in a new dataset
$subset = $dataset->head(5);

// Remove the first 5 rows and return them in a new dataset
$subset = $dataset->take(5);

// Split the dataset into left and right subsets
list($left, $right) = $dataset->split(0.5);

// Fold the dataset into 8 equal size datasets
$folds = $dataset->fold(8);

// Generate a dataset of 500 random samples with replacement
$subset = $dataset->randomSubsetWithReplacement(500);

// Randomize and split the dataset into two subsets
list($left, $right) = $dataset->randomize()->split(0.8);

// Return the sample matrix
$samples = $dataset->samples();
```
---
### Labeled
For Supervised Estimators you will need to pass it a Labeled Dataset that consists of a sample matrix where each row is a sample and each column is a feature, and an accompanying array of labels that correspond to the observed outcome of the sample.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| samples | None | array | A 2-dimensional array consisting of rows of samples and columns of features. |
| labels | None | array | A 1-dimensional array of labels that correspond to the samples in the dataset. |

##### Additional Methods:
| Method | Description |
|--|--|
| `stratifiedSplit($ratio = 0.5) : array` | Split the Dataset into left and right stratified subsets with a given ratio of samples and labels. |
| `stratifiedFold($k = 10) : array` | Fold the Dataset k - 1 times to form k equal size stratified Datasets. |
| `labels() : array` | Return the 1-dimensional array of labels. |
| `label(int $index) : mixed` | Return the label at the given row index. |
| `possibleOutcomes() : array` | Return all of the possible outcomes given the labels. |

##### Example:
```php
use Rubix\ML\Datasets\Labeled;

...
$dataset = new Labeled($samples, $labels);

// Fold the dataset into 5 equal size stratified subsets
$folds = $dataset->stratifiedFold(5);

// Randomize and split the dataset into two stratified subsets
list($left, $right) = $dataset->randomize()->stratifiedSplit(0.6);

// Return the label array
$labels = $dataset->labels();

// Return the label at the 200 index
$label = $dataset->label(200);

// Return all possible unique labels
$outcomes = $dataset->possibleOutcomes();
```

### Unlabeled
Unlabeled Datasets are useful for training Unsupervised Estimators and making predictions on new samples.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| samples | None | array | A 2-dimensional feature matrix consisting of rows of samples and columns of feature values. |

##### Additional Methods:
This Dataset does not have any additional methods.

##### Example:
```php
use Rubix\ML\Datasets\Unlabeled;

$dataset = new Unlabeled($samples);
```
---
### Feature Extractors
Feature Extractors are objects that help you encode raw data into feature vectors so they can be used by an Estimator.

Extractors have an API similar to [Transformers](#transformers), however, they are designed to be used on the raw data *before* it is inserted into a Dataset Object. The output of the `extract()` method is a sample matrix that can be used to build a [Dataset Object](#dataset-objects).

Fit the Extractor to the training samples:
```php
public fit(array $samples) : void
```

Return a 2-d array of extracted sample vectors:
```php
public extract(array $samples) : array
```

##### Example:
```php
use Rubix\ML\Extractors\CountVectorizer;
use Rubix\ML\Datasets\Unlabeled;
use Rubix\ML\Datasets\Labeled;

...
$estractor = new CountVectorizer(5000);

$extractor->fit($data);

$samples = $extractor->extract($data);

$dataset = new Unlabeled($samples);

$dataset = new Labeled($samples, $labels);
```

### Count Vectorizer
Word counts are often used to represent natural language as numerical vectors. The Count Vectorizer builds a vocabulary from the training samples and transforms text into sparse vectors consisting of the number of times a vocabulary words appears in the sample.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| max vocabulary | PHP_INT_MAX | int | The maximum number of words to encode into each word vector. |
| normalize | true | bool | Should we remove extra whitespace and lowercase? |
| tokenizer | Word | object | The method of turning samples of text into individual tokens. |

##### Available Tokenizers:
| Tokenizer | Description |
|--|--|
| Whitespace | Tokens are exploded by a user-specified whitespace character. |
| Word | Tokenize strings that meet the criteria of a word. |

##### Additional Methods:
| Method | Description |
|--|--|
| `vocabulary() : array` | Returns the vocabulary array. |
| `size() : int` | Returns the size of the vocabulary in number of tokens.

##### Example:
```php
use Rubix\ML\Extractors\CountVectorizer;
use Rubix\ML\Extractors\Tokenizers\Word;

$extractor = new CountVectorizer(5000, true, new Word());

// Return the vocabulary of the vectorizer
$extractor->vocabulary();

// Return the size of the fitted vocabulary
$extractor->size();
```

### Pixel Encoder
Images must first be converted to color channel values in order to be passed to an Estimator. The Pixel Encoder takes an array of images (as [PHP Resources](http://php.net/manual/en/language.types.resource.php)) and converts them to a flat vector of color channel data. Image scaling and cropping is handled automatically by [Intervention Image](http://image.intervention.io/).

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| size | [32, 32] | array | A tuple of width and height values denoting the resolution of the encoding. |
| rgb | true | bool | True to use RGB color channel data and False to use Greyscale. |
| sharpen | 0 | int | A value between 0 and 100 indicating the amount of sharpness to add to each sample. |
| driver | 'gd' | string | The PHP extension to use for image processing ('gd' *or* 'imagick'). |

##### Additional Methods:
This Extractor does not have any additional methods.

##### Example:
```php
use Rubix\ML\Extractors\PixelEncoder;

$extractor = new PixelEncoder([28, 28], false, 'imagick');
```

---
### Estimators
Estimators are the core of the Rubix library and consist of various [Classifiers](#classifiers), [Regressors](#regressors), [Clusterers](#clusterers), and [Anomaly Detectors](#anomaly-detectors) that make *predictions* based on the data they have been trained with.

To train an Estimator pass it a training Dataset object:
```php
public train(Dataset $training) : void
```

To make predictions, pass it another dataset:
```php
public predict(Dataset $dataset) : array
```

The return array of a prediction is 0 indexed containing the predicted values indexed in the same order.

##### Example:
```php
use Rubix\ML\Classifiers\RandomForest;
use Rubix\ML\Datasets\Labeled;

...
$dataset = new Labeled($samples, $labels);

$estimator = new RandomForest(200, 0.5, 5, 3);

// Take 3 samples out of the dataset to use later
$testing = $dataset->take(3);

// Train the estimator with the labeled dataset
$estimator->train($dataset);

// Make some predictions on the holdout set
$result = $estimator->predict($testing);

var_dump($result);
```

##### Output:
```sh
array(3) {
	[0] => 'married',
	[1] => 'divorced',
	[2] => 'married',
}
```

---
### Anomaly Detectors

Anomaly detection is the process of identifying samples that do not conform to the expected pattern. They can be used in fraud prevention, intrusion detection, biology and physics, and many more areas. Detectors predict an output value of either *0* for a normal sample or *1* for an outlier.

### Isolation Forest
An [Ensemble-based](https://en.wikipedia.org/wiki/Ensemble_learning) Anomaly Detector comprised of [Isolation Trees](#isolation-tree) each trained on a different subset of the training set. The Isolation Forest works by averaging the isolation score of a sample across a user-specified number of trees.

##### Unsupervised, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| trees | 100 | int | The number of Isolation Trees to train in the ensemble. |
| ratio | 0.1 | float | The ratio of random samples to train each Isolation Tree with. |
| threshold | 0.5 | float | The threshold isolation score. i.e. the probability that a sample is an outlier. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\AnomalyDetection\IsolationForest;

$estimator = new IsolationForest(500, 0.1, 0.7);
```
### Isolation Tree
Isolation Trees separate anomalous samples from dense clusters using an extremely randomized splitting process that isolates outliers into their own nodes. **Note** this Estimator is considered a *weak learner* and is typically only used within the context of an ensemble (such as [Isolation Forest](#isolation-forest)).

##### Unsupervised, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| max depth | PHP_INT_MAX | int | The maximum depth of a branch that is allowed. |
| threshold | 0.5 | float | The minimum isolation score. i.e. the probability that a sample is an outlier. |

##### Additional Methods:
| Method | Description |
|--|--|
| `complexity() : int` | Returns the number of splits in the tree. |
| `height() : int` | Return the height of the tree. |
| `balance() : int` | Return the balance factor of the tree. |

##### Example:
```php
use Rubix\ML\AnomalyDetection\IsolationTree;

$estimator = new IsolationTree(1000, 0.65);
```

### Local Outlier Factor
The Local Outlier Factor (LOF) algorithm only considers the local region of a sample, set by the k parameter. A density estimate for each neighbor is computed by measuring the radius of the cluster centroid that the point and its neighbors form. The LOF is the ratio of the sample over the median radius of the local region.

##### Unsupervised, Probabilistic, Online, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| k | 10 | int | The k nearest neighbors that form a local region. |
| neighbors | 20 | int | The number of neighbors considered when computing the radius of a centroid. |
| threshold | 0.5 | float | The minimum density score. i.e. the probability that a sample is an outlier. |
| kernel | Euclidean | object | The distance metric used to measure the distance between two sample points. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\AnomalyDetection\LocalOutlierFactor;
use Rubix\ML\Kernels\Distance\Minkowski;

$estimator = new LocalOutlierFactor(10, 20, 0.2, new Minkowski(3.5));
```

### Robust Z Score
A quick global anomaly Detector, Robust Z Score uses a threshold to detect outliers within a Dataset. The modified Z score consists of taking the median and median absolute deviation (MAD) instead of the mean and standard deviation thus making the statistic more robust to training sets that may already contain outliers.

##### Unsupervised, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| threshold | 3.5 | float | The threshold Z score to flag an outlier. |

##### Additional Methods:
| Method | Description |
|--|--|
| `medians() : array` | Return the medians of each feature column in the training set. |
| `mads() : array` | Return the median absolute deviations (MAD) of each feature column in the training set. |

##### Example:
```php
use Rubix\ML\AnomalyDetection\RobustZScore;

$estimator = new RobustZScore(3.0);
```

---
### Classifiers
Classifiers are a type of Estimator that predict discrete outcomes such as class labels. There are two types of Classifiers in Rubix - *Binary* and *Multiclass*. Binary Classifiers can only distinguish between two classes (ex. *Male*/*Female*, *Yes*/*No*, etc.) whereas a Multiclass Classifier is able to handle two or more unique class outcomes.

### AdaBoost
Short for Adaptive Boosting, this ensemble classifier can improve the performance of an otherwise weak classifier by focusing more attention on samples that are harder to classify.

##### Supervised, Binary, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| base | None | string | The fully qualified class name of the base "weak" classifier. |
| params | [ ] | array | The parameters of the base classifer. |
| experts | 100 | int | The number of classifiers to train in the ensemble. |
| ratio | 0.1 | float | The ratio of samples to subsample from the training dataset per epoch. |
| threshold | 0.999 | float | The minimum accuracy an epoch must score before the algorithm terminates. |

##### Additional Methods:
| Method | Description |
|--|--|
| `weights() : array` | Returns the calculated weight values of the last trained dataset. |
| `influence() : array` | Returns the influence scores for each boosted "weak" classifier.


##### Example:
```php
use Rubix\ML\Classifiers\AdaBoost;
use Rubix\ML\Classifiers\DecisionTree;

$estimator = new AdaBoost(DecisionTree::class, [1, 10], 100, 0.1, 0.999);

$estimator->weights(); // [0.25, 0.35, 0.1, ...]

$estimator->influence(); // [0.7522, 0.7945, ...]
```

### Committee Machine
Ensemble classifier that aggregates the class probability predictions of a committee of user-specified, heterogeneous [Probabilistic](#probabilistic) classifiers (MultiLayerPerceptron, Random Forest, etc.).

##### Supervised, Multiclass, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| experts | None | array | The probabilistic classifier instances that comprise the committee. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Classifiers\CommitteeMachine;
use Rubix\ML\Classifiers\RandomForest;
use Rubix\ML\Classifiers\SoftmaxClassifier;
use Rubix\ML\NeuralNet\Optimizers\Adam;
use Rubix\ML\Classifiers\KNearestNeighbors;

$estimator = new CommitteeMachine([
	new RandomForest(100, 0.3, 50, 3, 1e-2),
	new SoftmaxClassifier(50, new Adam(0.001), 1e-4),
	new KNearestNeighbors(3),
]);
```

### Decision Tree
Binary Tree based algorithm that works by intelligently splitting the training data at various decision nodes until a terminating condition is met.

##### Supervised, Multiclass, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| max depth | PHP_INT_MAX | int | The maximum depth of a branch that is allowed. Setting this to 1 is equivalent to training a Decision Stump. |
| min samples | 5 | int | The minimum number of data points needed to split a decision node. |
| epsilon | 1e-2 | float | The amount of gini impurity to tolerate when choosing the best split. |

##### Additional Methods:
| Method | Description |
|--|--|
| `complexity() : int` | Returns the number of splits in the tree. |
| `height() : int` | Return the height of the tree. |
| `balance() : int` | Return the balance factor of the tree. |

##### Example:
```php
use Rubix\ML\Classifiers\DecisionTree;

$estimator = new DecisionTree(10, 3, 1e-4);

$estimator->complexity(); // 20
$estimator->height(); // 9
$estimator->balance(); // -1
```

### Dummy Classifier
A classifier based on a given [Imputer strategy](#missing-data-imputer). Used to provide sanity check and to compare performance with an actual classifier.

##### Supervised, Multiclass, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| strategy | PopularityContest | object | The imputer strategy to employ when guessing the outcome of a sample. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Classifiers\DummyClassifier;
use Rubix\ML\Transformers\Strategies\PopularityContest;

$estimator = new DummyClassifier(new PopularityContest());
```

### K Nearest Neighbors
A lazy learning algorithm that locates the K nearest samples from the training set and uses a majority vote to classify the unknown sample.

##### Supervised, Multiclass, Probabilistic, Online, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| k | 5 | int | The number of neighboring training samples to consider when making a prediction. |
| kernel | Euclidean | object | The distance metric used to measure the distance between two sample points. |

##### Additional Methods:
This Estimator does not have any additional methods.|

##### Example:
```php
use Rubix\ML\Classifiers\KNearestNeighbors;
use Rubix\ML\Kernels\Distance\Euclidean;

$estimator = new KNearestNeighbors(3, new Euclidean());
```

### Logistic Regression
A type of regression analysis that uses the logistic function to classify between two possible outcomes.

##### Supervised, Binary, Online, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| batch size | 10 | int | The number of training samples to process at a time. |
| optimizer | Adam | object | The gradient descent step optimizer used to train the underlying network. |
| alpha | 1e-4 | float | The L2 regularization term. |
| min change | 1e-4 | float | The minimum change in the weights necessary to continue training. |
| epochs | 100 | int | The maximum number of training epochs to execute. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Classifers\LogisticRegression;
use Rubix\ML\NeuralNet\Optimizers\Adam;

$estimator = new LogisticRegression(200, 10, new Adam(0.001), 1e-4);
```

### Multi Layer Perceptron
Multiclass [Neural Network](#neural-network) model that uses a series of user-defined [Hidden Layers](#hidden) as intermediate computational units. The MLP also features progress monitoring which means that it will automatically stop training when it can no longer make progress.

##### Supervised, Multiclass, Online, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| hidden | [ ] | array | An array of hidden layers of the neural network. |
| batch size | 10 | int | The number of training samples to process at a time. |
| optimizer | Adam | object | The gradient descent step optimizer used to train the underlying network. |
| alpha | 1e-4 | float | The L2 regularization term. |
| metric | Accuracy | object | The validation metric used to monitor the training progress of the network. |
| ratio | 0.2 | float | The ratio of sample data to hold out for validation during training. |
| window | 3 | int | The number of epochs to consider when determining if the algorithm should terminate or keep training. |
| epochs | PHP_INT_MAX | int | The maximum number of training epochs to execute. |

##### Additional Methods:
| Method | Description |
|--|--|
| `progress() : array` | Returns an array with the validation score at each epoch of training. |

##### Example:
```php
use Rubix\ML\Classifiers\MultiLayerPerceptron;
use Rubix\ML\NeuralNet\Layers\Dense;
use Rubix\ML\NeuralNet\ActivationFunctions\ELU;
use Rubix\ML\NeuralNet\Optimizers\Adam;
use Rubix\ML\CrossValidation\Metrics\MCC;

$hidden = [
	new Dense(10, new ELU()),
	new Dense(10, new ELU()),
	new Dense(10, new ELU()),
];

$estimator = new MultiLayerPerceptron($hidden, 10, new Adam(0.001), 1e-4, new MCC(), 0.2, 3, PHP_INT_MAX);

$estimator->progress(); // [0.45, 0.59, 0.72, 0.88, ...]
```

### Naive Bayes
Probability-based classifier that used probabilistic inference to derive the predicted class.

##### Supervised, Multiclass, Probabilistic, Persistable

##### Parameters:
This estimator does not have any hyperparameters.

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Classifiers\NaiveBayes;

$estimator = new NaiveBayes();
```

### Random Forest
Ensemble classifier that trains Decision Trees on a random subset of the training data.

##### Supervised, Multiclass, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| trees | 100 | int | The number of Decision Trees to train in the ensemble. |
| ratio | 0.1 | float | The ratio of random samples to train each Decision Tree with. |
| max depth | 10 | int | The maximum depth of a branch that is allowed. Setting this to 1 is equivalent to training a Decision Stump. |
| min samples | 5 | int | The minimum number of data points needed to split a decision node. |
| epsilon | 1e-2 | float | The amount of gini impurity to tolerate when choosing the best split. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Classifiers\RandomForest;

$estimator = new RandomForest(100, 0.2, 5, 3, 1e-2);
```

### Softmax Classifier
A generalization of logistic regression for multiple class outcomes.

##### Supervised, Multiclass, Online, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| batch size | 10 | int | The number of training samples to process at a time. |
| optimizer | Adam | object | The gradient descent step optimizer used to train the underlying network. |
| alpha | 1e-4 | float | The L2 regularization term. |
| min change | 1e-4 | float | The minimum change in the weights necessary to continue training. |
| epochs | 100 | int | The maximum number of training epochs to execute. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Classifiers\SoftmaxClassifier;
use Rubix\ML\NeuralNet\Optimizers\Momentum;

$estimator = new SoftmaxClassifier(200, 10, new Momentum(0.001), 1e-4);
```
---
### Clusterers
Clusterers take Unlabeled data points and assign them to a cluster. The return value of each prediction is the cluster number each sample was assigned to. Clustering is a common technique in data mining that focuses on grouping samples in a way such that the groups are similar.

### DBSCAN
Density-based spatial clustering of applications with noise is a clustering algorithm able to find non-linearly separable and arbitrarily-shaped clusters.

##### Unsupervised, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| radius | 0.5 | float | The maximum radius between two points for them to be considered in the same cluster. |
| min density | 5 | int | The minimum number of points within radius of each other to form a cluster. |
| kernel | Euclidean | object | The distance metric used to measure the distance between two sample points.

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Clusterers\DBSCAN;
use Rubix\ML\Kernels\Distance\Manhattan;

$estimator = new DBSCAN(4.0, 5, new Manhattan());
```

### Fuzzy C Means
Clusterer that allows data points to belong to multiple clusters if they fall within a fuzzy region.

##### Unsupervised, Probabilistic, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| c | None | int | The number of target clusters. |
| fuzz | 2.0 | float | Determines the bandwidth of the fuzzy area. |
| kernel | Euclidean | object | The distance metric used to measure the distance between two sample points. |
| threshold | 1e-4 | float | The minimum change in centroid means necessary for the algorithm to continue training. |
| epochs | PHP_INT_MAX | int | The maximum number of training rounds to execute. |

##### Additional Methods:
| Method | Description |
|--|--|
| `centroids() : array` | Returns an array of the C computed centroids of the training data. |
| `progress() : array` | Returns the progress of each epoch as the total distance between each sample and centroid. |

##### Example:
```php
use Rubix\ML\Clusterers\FuzzyCMeans;
use Rubix\ML\Kernels\Distance\Euclidean;

$estimator = new FuzzyCMeans(5, 2.5, new Euclidean(), 1e-3, 1000);

$estimator->centroids(); // [[3.149, 2.615], [-1.592, -3.444], ...]
$estimator->progress(); // [5878.01, 5200.50, 4960.28, ...]
```

### K Means
A fast centroid-based hard clustering algorithm capable of clustering linearly separable data points.

##### Unsupervised, Online, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| k | None | int | The number of target clusters. |
| kernel | Euclidean | object | The distance metric used to measure the distance between two sample points. |
| epochs | PHP_INT_MAX | int | The maximum number of training rounds to execute. |

##### Additional Methods:
| Method | Description |
|--|--|
| `centroids() : array` | Returns an array of the K computed centroids of the training data. |

##### Example:
```php
use Rubix\ML\Clusterers\KMeans;
use Rubix\ML\Kernels\Distance\Euclidean;

$estimator = new KMeans(3, new Euclidean());

$estimator->centroids(); // [[3.149, 2.615], [-1.592, -3.444], ...]
```

### Mean Shift
A hierarchical clustering algorithm that uses peak finding to locate the local maxima (Centroids) of a training set given by a radius constraint.

##### Unsupervised, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| radius | None | float | The radius of each cluster centroid. |
| kernel | Euclidean | object | The distance metric used to measure the distance between two sample points. |
| threshold | 1e-8 | float | The minimum change in centroid means necessary for the algorithm to continue training. |
| epochs | PHP_INT_MAX | int | The maximum number of training rounds to execute. |

##### Additional Methods:
| Method | Description |
|--|--|
| `centroids() : array` | Returns an array of the K computed centroids of the training data. |

##### Example:
```php
use Rubix\ML\Clusterers\MeanShift;
use Rubix\ML\Kernels\Distance\Euclidean;

$estimator = new MeanShift(3.0, new Euclidean(), 1e-6, 3000);
```

---
### Regressors
A Regressor estimates the continuous expected output value of a given sample. Regression analysis is often used to predict the outcome of an experiment where the outcome can range over a continuous spectrum of values.

### Dummy Regressor
Regressor that guesses the output values based on an [Imputer](#missing-data-imputer) strategy. Used to provide sanity check and to compare performance against actual Regressors.

##### Supervised, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| strategy | BlurryMean | object | The imputer strategy to employ when guessing the outcome of a sample. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Regressors\DummyRegressor;
use Rubix\ML\Tranformers\Strategies\BlurryMean;

$estimator = new DummyRegressor(new BlurryMean());
```

### KNN Regressor
A version of [K Nearest Neighbors](#k-nearest-neighbors) that uses the mean outcome of K nearest data points to make continuous valued predictions suitable for regression problems.

##### Supervised, Online, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| k | 5 | int | The number of neighboring training samples to consider when making a prediction. |
| kernel | Euclidean | object | The distance metric used to measure the distance between two sample points. |

##### Additional Methods:
This Estimator does not have any additional methods.

##### Example:
```php
use Rubix\ML\Regressors\KNNRegressor;
use Rubix\ML\Kernels\Distance\Minkowski;

$estimator = new KNNRegressor(2, new Minkowski(3.0));
```

### MLP Regressor
A [Neural Network](#neural-network) with a continuous output layer suitable for regression problems. The MLP also features progress monitoring which means that it will automatically stop training when it can no longer make progress.

##### Supervised, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| hidden | [ ] | array | An array of hidden layers of the neural network. |
| batch size | 10 | int | The number of training samples to process at a time. |
| optimizer | Adam | object | The gradient descent step optimizer used to train the underlying network. |
| alpha | 1e-4 | float | The L2 regularization term. |
| metric | Accuracy | object | The validation metric used to monitor the training progress of the network. |
| ratio | 0.2 | float | The ratio of sample data to hold out for validation during training. |
| window | 3 | int | The number of epochs to consider when determining if the algorithm should terminate or keep training. |
| epochs | PHP_INT_MAX | int | The maximum number of training epochs to execute. |

##### Additional Methods:
| Method | Description |
|--|--|
| `progress() : array` | Returns an array with the validation score at each epoch of training. |

##### Example:
```php
use Rubix\ML\Regressors\MLPRegressor;
use Rubix\ML\NeuralNet\Layers\Dense;
use Rubix\ML\NeuralNet\ActivationFunctions\HyperbolicTangent;
use Rubix\ML\NeuralNet\ActivationFunctions\PReLU;
use Rubix\ML\NeuralNet\Optimizers\RMSProp;
use Rubix\ML\CrossValidation\Metrics\RSquared;

$hidden = [
	new Dense(30, new HyperbolicTangent()),
	new Dense(50, new PReLU()),
];

$estimator = new MLPRegressor($hidden, 10, new RMSProp(0.001), 1e-2, new RSquared(), 0.2, 3, PHP_INT_MAX);

$estimator->progress(); // [0.66, 0.88, 0.94, 0.95, ...]
```

### Regression Tree
A binary tree learning algorithm that performs greedy splitting by minimizing the variance between decision node splits.

##### Supervised, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| max depth | PHP_INT_MAX | int | The maximum depth of a branch that is allowed. Setting this to 1 is equivalent to training a Decision Stump. |
| min samples | 5 | int | The minimum number of data points needed to split a decision node. |
| epsilon | 1e-2 | float | The amount of variance to tolerate when the best split. |

##### Additional Methods:
| Method | Description |
|--|--|
| `complexity() : int` | Returns the number of splits in the tree. |
| `height() : int` | Return the height of the tree. |
| `balance() : int` | Return the balance factor of the tree. |

##### Example:
```php
use Rubix\ML\Regressors\RegressionTree;

$estimator = new RegressionTree(50, 1, 0.0);

$estimator->complexity(); // 36
$estimator->height(); // 18
$estimator->balance(); // 0
```

### Ridge
L2 penalized least squares regression whose predictions are based on the closed-form solution to the training data.

##### Supervised, Persistable

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| alpha | 1.0 | float | The L2 regularization term. |

##### Additional Methods:
| Method | Description |
|--|--|
| `intercept() : float` | Returns the y intercept of the computed regression line. |
| `coefficients() : array` | Return an array containing the computed coefficients of the regression line. |

##### Example:
```php
use Rubix\ML\Regressors\Ridge;

$estimator = new Ridge(2.0);

$estimator->intercept(); // 5.298226
$estimator->coefficients(); // [2.023, 3.122, 5.401, ...]
```

---
### Online

Certain [Estimators](#estimators) that implement the *Online* interface can be trained in batches. Estimators of this type are great for when you either have a continuous stream of data or a dataset that is too large to fit into memory.

You can partially train an Online Estimator as such:
```php
public partial(Dataset $dataset) : void
```

##### Example:
```php
...
$datasets = $dataset->fold(3);

$estimator->partial($dataset[0]);

$estimator->partial($dataset[1]);

$estimator->partial($dataset[2]);
```
---
### Probabilistic

Some [Estimators](#estimators) may implement the *Probabilistic* interface, in which case, they will have an additional method that returns an array of probability scores of each class, cluster, etc.

Calculate probability estimates:
```php
public proba(Dataset $dataset) : array
```

##### Example:
```php
// Return the probabilities of the outcomes of the first 2 samples in the dataset
$result = $estimator->proba($dataset->head(2));  

var_dump($result);
```

##### Output:
```sh
array(3) {
	[0] => array(2) {
		['married'] => 0.975,
		['divorced'] => 0.025,
	}
	[1] => array(2) {
		['married'] => 0.200,
		['divorced'] => 0.800,
	}
}
```

---
### Meta-Estimators
Meta-Estimators allow you to progressively enhance your models by adding additional functionality such as [data preprocessing](#data-preprocessing) and [persistence](#model-persistence). Each Meta-Estimator wraps a base Estimator and you can even wrap certain Meta-Estimators with other Meta-Estimators. Some examples of Meta-Estimators in Rubix are [Pipeline](#pipeline), [Grid Search](#grid-search), and [Persistent Model](#persistent-model).

You can call any of the base Estimator(s)' methods from the Meta-Estimator simply by invoking its signature.

##### Example:
```php
use Rubix\ML\Pipeline;
use Rubix\ML\GridSearch;
use Rubix\ML\CrossValidation\Metrics\MCC;
use Rubix\ML\CrossValidation\KFold;
use Rubix\ML\Classifiers\DecisionTree;
use Rubix\ML\Transformers\NumericStringConverter;

...
$params = [[10, 30, 50, 100], [1, 3, 5]];

$estimator = new Pipeline(new GridSearch(DecisionTree::class, $params, new MCC(), new KFold(10)));

$estimator->train($dataset); // Train a Decision Tree with preprocessing and grid search

$estimator->complexity(); // Call complexity() method on Decision Tree from Pipeline
```

---
### Neural Network
A number of the Estimators in Rubix are implemented as a computational graph commonly referred to as a Neural Network due to its inspiration from the human brain. Neural Nets are trained using an iterative process called Gradient Descent and use Backpropagation (sometimes called Reverse Mode Autodiff) to calculate the error of each parameter in the network.

### Activation Functions
The input to every neuron is passed through an Activation Function which determines its output. There are different properties of Activation Functions that make them more or less desirable depending on your problem.

### ELU
Exponential Linear Units are a type of rectifier that soften the transition from non-activated to activated using the exponential function.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| alpha | 1.0 | float | The value at which leakage will begin to saturate. Ex. alpha = 1.0 means that the output will never be more than -1.0 when inactivated. |

##### Example:
```php
use Rubix\ML\NeuralNet\ActivationFunctions\ELU;

$activationFunction = new ELU(5.0);
```

### Hyperbolic Tangent
S-shaped function that squeezes the input value into an output space between -1 and 1 centered at 0.

##### Parameters:
This Activation Function does not have any parameters.

##### Example:
```php
use Rubix\ML\NeuralNet\ActivationFunctions\HyperbolicTangent;

$activationFunction = new HyperbolicTangent();
```

### Identity
The Identity function (sometimes called Linear Activation Function) simply outputs the value of the input.

##### Parameters:
This Activation Function does not have any parameters.

##### Example:
```php
use Rubix\ML\NeuralNet\ActivationFunctions\Identity;

$activationFunction = new Identity();
```

### PReLU
Parametric Rectified Linear Units are functions that output x when x > 0 or a small leakage value when x < 0. The amount of leakage is controlled by the user-specified parameter.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| leakage | 0.01 | float | The amount of leakage as a ratio of the input value. |

##### Example:
```php
use Rubix\ML\NeuralNet\ActivationFunctions\PReLU;

$activationFunction = new PReLU(0.001);
```

### Sigmoid
A bounded S-shaped function (specifically the Logistic function) with an output value between 0 and 1.

##### Parameters:
This Activation Function does not have any parameters.

##### Example:
```php
use Rubix\ML\NeuralNet\ActivationFunctions\Sigmoid;

$activationFunction = new Sigmoid();
```

---
### Layers
Every network is made up of layers of computational units called neurons. Each layer processes and transforms the input from the previous layer.

There are three types of Layers that form a network, **Input**, **Hidden**, and **Output**. A network can have as many Hidden layers as the user specifies, however, there can only be 1 Input and 1 Output layer per network.

##### Example:
```php
use Rubix\ML\NeuralNet\Network;
use Rubix\ML\NeuralNet\Layers\Input;
use Rubix\ML\NeuralNet\Layers\Dense;
use Rubix\ML\NeuralNet\Layers\Softmax;
use Rubix\ML\NeuralNet\ActivationFunctions\ELU;
use Rubix\ML\NeuralNet\Optimizers\Adam;

$network = new Network(new Input(784), [
	new Dense(200, new ELU()),
	new Dense(100, new ELU()),
	new Dense(100, new ELU()),
	new Dense(50, new ELU()),
], new Softmax([
	'dog', 'cat', 'frog', 'car',
], 1e-4), new Adam(0.001));
```

### Input
The Input Layer is simply a placeholder layer that represents the value of a sample or batch of samples. The number of placeholder nodes should be equal to the number of feature columns of a sample.

### Hidden
In multilayer networks, Hidden layers perform the bulk of the computation. They are responsible for transforming the input space in such a way that can be linearly separable by the Output layer. The more complex the problem space is, the more Hidden layers and neurons will be necessary to handle the complexity.

### Dense
Dense layers are fully connected Hidden layers, meaning each neuron is connected to each other neuron in the previous layer. Dense layers are able to employ a variety of [Activation Functions](#activation-functions) that modify the output of each neuron in the layer.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| neurons | None | int | The number of neurons in the layer. |
| activation fn | None | object | The activation function to use. |

##### Example:
```php
use Rubix\ML\NeuralNet\Layers\Dense;
use Rubix\ML\NeuralNet\ActivationFunctions\PReLU;

$layer = new Dense(100, new PReLU(0.05));
```

### Output
Activations are read directly from the Output layer when it comes to making a prediction. The type of Output layer used will determine the type of Estimator the Neural Net can power (Binary Classifier, Multiclass Classifier, or Regressor). The different types of Output layers are listed below.

### Continuous
The Continuous Output Layer consists of a single linear neuron that outputs a scalar value useful for Regression problems.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| alpha | 1e-4 | float | The L2 regularization penalty. |

##### Example:
```php
use Rubix\ML\NeuralNet\Layers\Continuous;

$layer = new Continuous(1e-5);
```

### Logistic
The Logistic Output Layer consists of a single sigmoid neuron capable of distinguishing between two classes.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| classes | None | array | The unique class labels of the binary classification problem. |
| alpha | 1e-4 | float | The L2 regularization penalty. |

##### Example:
```php
use Rubix\ML\NeuralNet\Layers\Logistic;

$layer = new Logistic(['yes', 'no'], 1e-5);
```

### Softmax
A generalization of the Logistic Layer, the Softmax Output Layer gives a joint probability estimate of a multiclass classification problem.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| classes | None | array | The unique class labels of the multiclass classification problem. |
| alpha | 1e-4 | float | The L2 regularization penalty. |

##### Example:
```php
use Rubix\ML\NeuralNet\Layers\Softmax;

$layer = new Softmax(['yes', 'no', 'maybe'], 1e-6);
```

---
### Optimizers
Gradient Descent is an algorithm that takes iterative steps towards minimizing an objective function. There have been many papers that describe enhancements to the standard Stochastic Gradient Descent algorithm whose methods are encapsulated in pluggable Optimizers by Rubix. More specifically, Optimizers control the amount of Gradient Descent step to take for each parameter in the network upon each training iteration.

### AdaGrad
Short for *Adaptive Gradient*, the AdaGrad Optimizer speeds up the learning of parameters that do not change often and slows down the learning of parameters that do enjoy heavy activity.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| rate | 0.001 | float | The learning rate. i.e. the master step size. |

##### Example:
```php
use Rubix\ML\NeuralNet\Optimizers\AdaGrad;

$optimizer = new AdaGrad(0.035);
```

### Adam
Short for *Adaptive Momentum Estimation*, the Adam Optimizer uses both Momentum and RMS properties to achieve a balance of velocity and stability.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| rate | 0.001 | float | The learning rate. i.e. the master step size. |
| momentum | 0.9 | float | The decay rate of the Momentum property. |
| rms | 0.999 | float | The decay rate of the RMS property. |
| epsilon | 1e-8 | float | The smoothing constant used for numerical stability. |

##### Example:
```php
use Rubix\ML\NeuralNet\Optimizers\Adam;

$optimizer = new Adam(0.0001, 0.9, 0.999, 1e-8);
```

### Momentum
Momentum adds velocity to each step until exhausted. It does so by accumulating momentum from past updates and adding a factor to the current step.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| rate | 0.001 | float | The learning rate. i.e. the master step size. |
| decay | 0.9 | float | The Momentum decay rate. |

##### Example:
```php
use Rubix\ML\NeuralNet\Optimizers\Momentum;

$optimizer = new Momentum(0.001, 0.925);
```

### RMS Prop
An adaptive gradient technique that divides the current gradient over a rolling window of magnitudes of recent gradients.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| rate | 0.001 | float | The learning rate. i.e. the master step size. |
| decay | 0.9 | float | The RMS decay rate. |

##### Example:
```php
use Rubix\ML\NeuralNet\Optimizers\RMSProp;

$optimizer = new RMSProp(0.01, 0.9);
```

### Step Decay
A learning rate decay stochastic optimizer that reduces the learning rate by a factor of the decay parameter when it reaches a new floor (takes k steps).

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| rate | 0.001 | float | The learning rate. i.e. the master step size. |
| k | 10 | int | The size of every floor in steps. i.e. the number of steps to take before applying another factor of decay. |
| decay | 1e-5 | float | The decay factor to decrease the learning rate by every k steps. |

##### Example:
```php
use Rubix\ML\NeuralNet\Optimizers\StepDecay;

$optimizer = new StepDecay(0.001, 15, 1e-5);
```

### Stochastic
A constant learning rate Optimizer.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| rate | 0.001 | float | The learning rate. i.e. the master step size. |

##### Example:
```php
use Rubix\ML\NeuralNet\Optimizers\Stochastic;

$optimizer = new Stochastic(0.001);
```

---
### Data Preprocessing
Often, additional processing of input data is required to deliver correct predictions and/or accelerate the training process. In this section, we'll introduce the Pipeline meta-Estimator and the various [Transformers](#transformers) that it employs to fit the input data to suit the requirements and preferences of the [Estimator](#estimator) that it feeds.

### Pipeline
A Pipeline is an Estimator that wraps another Estimator with additional functionality. For this reason, a Pipeline is called a *meta-Estimator*. As arguments, Pipeline accepts a base Estimator and a list of Transformers to apply to the input data before it is fed to the learning algorithm. Under the hood, the Pipeline will automatically fit the training set upon training and transform any [Dataset object](#dataset-objects) supplied as an argument to one of the base Estimator's methods, including `predict()`.

##### Example:
```php
use Rubix\ML\Pipeline;
use Rubix\ML\Classifiers\SoftmaxClassifier;
use Rubix\ML\Transformers\MissingDataImputer;
use Rubix\ML\Transformers\OneHotEncoder;

$estimator = new Pipeline(new SoftmaxClassifier(200, 50), [
	new MissingDataImputer(),
	new OneHotEncoder(),
]);

$estimator->train($dataset); // Dataset objects are fit and
$estimator->predict($samples); // transformed automatically.
```

Transformer middleware will process in the order given when the Pipeline was built and cannot be reordered without instantiating a new one. Since Tranformers are run sequentially, the order in which they run *matters*. For example, a Transformer near the end of the stack may depend on a previous Transformer to convert all categorical features into continuous ones before it can run.

In practice, applying transformations can drastically improve the performance of your model by cleaning, scaling, expanding, compressing, and normalizing the input data. Below is a list of the available Transformers in Rubix.

### Transformers
Transformers are generally designed to be used by the [Pipeline](#pipeline) meta-Estimator, however they can be used manually as well.

The fit method will allow the transformer to compute any necessary information from the training set in order to carry out its transformations. You can think of *fitting* a Transformer like *training* an Estimator. Not all Transformers need to be fit to the training set, when in doubt do it anyways, it won't hurt.
```php
public fit(Dataset $dataset) : void
```

The transform method on the Transformer is not meant to be called directly. This is done to force transformations to be done on the Dataset object in place so to deal with large datasets. To transform a dataset you can simply pass the instantiated transformer to a Dataset object's `transform()` method.

##### Example:
```php
use Rubix\ML\Transformers\MinMaxNormalizer;

$transformer = new MinMaxNormalizer();

$dataset->transform($transformer);
```

### Dense and Sparse Random Projectors
A Random Projector is a dimensionality reducer based on the [Johnson-Lindenstrauss lemma](https://en.wikipedia.org/wiki/Johnson-Lindenstrauss_lemma "Johnson-Lindenstrauss lemma") that uses a random matrix to project a feature vector onto a user-specified number of dimensions. It is faster than most non-randomized dimensionality reduction techniques and offers similar performance.

The difference between the Dense and Sparse Random Projectors are that the Dense version uses a dense random guassian distribution and the Sparse version uses a sparse matrix (mostly 0's).

##### Continuous *Only*

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| dimensions | None | int | The number of target dimensions to project onto. |

##### Additional Methods:
This Transformer does not have any additional methods.

##### Example:
```php
use Rubix\ML\Transformers\DenseRandomProjector;
use Rubix\ML\Transformers\SparseRandomProjector;

$transformer = new DenseRandomProjector(50);
$transformer = new SparseRandomProjector(100);
```

### L1 and L2 Regularizers
Augment the input vector of each sample such that each feature is divided over the L1 or L2 norm (or "magnitude") of the feature vector.

##### Continuous *Only*

##### Parameters:
This Transformer does not have any parameters.

##### Additional Methods:
This Transformer does not have any additional methods.

##### Example:
```php
use Rubix\ML\Transformers\L1Regularizer;
use Rubix\ML\Transformers\L2Regularizer;

$transformer = new L1Regularizer();
$transformer = new L2Regularizer();
```

### Min Max Normalizer
Often used as an alternative to [Standard Scaling](#z-scale-standardizer), the Min Max Normalization scales the input features from a range of 0 to 1 by dividing the feature value over the maximum value for that feature column.

##### Continuous

##### Parameters:
This Transformer does not have any parameters.

##### Additional Methods:
This Transformer does not have any additional methods.

##### Example:
```php
use Rubix\ML\Transformers\MinMaxNormalizer;

$transformer = new MinMaxNormalizer();
```

### Missing Data Imputer
In the real world, it is common to have to deal with datasets that are missing a few values here and there. The Missing Data Imputer searches for any missing value placeholders and replaces it with a guess based on a given imputer *Strategy*.

##### Categorical or Continuous

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| placeholder | '?' | string or numeric | The placeholder that denotes a missing value. |
| continuous strategy | BlurryMean | object | The imputer strategy to employ for continuous feature columns. |
| categorical strategy | PopularityContest | object | The imputer strategy to employ for categorical feature columns. |

##### Additional Methods:
This Transformer does not have any additional methods.

##### Example:
```php
use Rubix\ML\Transformers\MissingDataImputer;
use Rubix\ML\Transformers\Strategies\BlurryMean;
use Rubix\ML\Transformers\Strategies\PopularityContest;

$transformer = new MissingDataImputer('?', new BlurryMean(0.2), new PopularityContest());
```

### Numeric String Converter
This handy Transformer will convert all numeric strings into their integer or float counterparts. Useful for when extracting from a source that only recognizes data as string types.

##### Categorical

##### Parameters:
This Transformer does not have any parameters.

##### Additional Methods:
This Transformer does not have any additional methods.

##### Example:
```php
use Rubix\ML\Transformers\NumericStringConverter;

$transformer = new NumericStringConverter();
```

### One Hot Encoder
The One Hot Encoder takes a column of categorical features, such as the country a person was born in, and produces a one-hot vector of n-dimensions where n is equal to the number of unique categories of the feature column.

##### Categorical

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| columns | Null | array | The user-specified columns to encode indicated by numeric index starting at 0. |

##### Additional Methods:
This Transformer does not have any additional methods.

##### Example:
```php
use Rubix\ML\Transformers\OneHotEncoder;

$transformer = new OneHotEncoder([0, 3, 5, 7, 9]);
```

### Polynomial Expander
This Transformer will generate polynomial features of specified degrees. Polynomial expansion is often used to fit data to a non-linear curve using standard linear regression.

##### Continuous *Only*

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| degree | 2 | int | The highest degree polynomial to generate from each feature vector. |

##### Additional Methods:
This Transformer does not have any additional methods.

##### Example:
```php
use Rubix\ML\Transformers\PolynomialExpander;

$transformer = new PolynomialExpander(3);
```

### Robust Standardizer
This Transformer standardizes continuous features by removing the median and dividing over the median absolute deviation (MAD), a value referred to as robust z score. The use of robust statistics makes this standardizer more immune to outliers than the [Z Scale Standardizer](#z-scale-standardizer).

##### Continuous

##### Parameters:
This Transformer does not have any parameters.

##### Additional Methods:
| Method | Description |
|--|--|
| `medians() : array` | Return the medians calculated by fitting the training set. |
| `mads() : array` | Return the median absolute deviations calculated during fitting. |

##### Example:
```php
use Rubix\ML\Transformers\RobustStandardizer;

$transformer = new RobustStandardizer();
```

### TF-IDF Transformer
Term Frequency - Inverse Document Frequency is the measure of how important a word is to a document. The TF-IDF value increases proportionally with the number of times a word appears in a document and is offset by the frequency of the word in the corpus. This Transformer makes the assumption that the input is made up of word frequency vectors such as those created by the [Count Vectorizer](#count-vectorizer).

##### Continuous *Only*

##### Parameters:
This Transformer does not have any parameters.

##### Additional Methods:
This Transformer does not have any additional methods.

##### Example:
```php
$transformer = new TfIdfTransformer();
```

### Variance Threshold Filter
A type of feature selector that removes all columns that have a lower variance than the threshold. Variance is computed as the population variance of all the values in the feature column.

##### Categorical and Continuous

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| threshold | 0.0 | float | The threshold at which lower scoring columns will be dropped from the dataset. |

##### Additional Methods:
| Method | Description |
|--|--|
| `selected() : array` | An array containing the columns that were selected during fitting. |

##### Example:
```php
use Rubix\ML\Transformers\VarianceThresholdFilter;

$transformer = new VarianceThresholdFilter(500);
```

### Z Scale Standardizer
A way of centering and scaling an input vector by computing the Z Score for each continuous feature.

##### Continuous

##### Parameters:
This Transformer does not have any parameters.

##### Additional Methods:
| Method | Description |
|--|--|
| `means() : array` | Return the means calculated by fitting the training set. |
| `stddevs() : array` | Return the standard deviations calculated during fitting. |

##### Example:
```php
use Rubix\ML\Transformers\ZScaleStandardizer;

$transformer = new ZScaleStandardizer();
```

---
### Cross Validation
Cross validation is the process of testing the generalization performance of a computer model using various techniques. Rubix has a number of classes that run cross validation on an instantiated Estimator for you. Each Validator outputs a scalar score based on the chosen metric.

### Validators
Validators take an [Estimator](#estimators) instance, [Labeled Dataset](#labeled) object, and validation [Metric](#validation-metrics) and return a validation score that measures the generalization performance of the model using one of various cross validation techniques. There is no need to train the Estimator beforehand as the Validator will automatically train it on subsets of the dataset chosen by the testing algorithm.

```php
public test(Estimator $estimator, Labeled $dataset, Validation $metric) : float
```

##### Example:
```php
use Rubix\ML\CrossValidation\KFold;
use Rubix\ML\CrossValidation\Metrics\Accuracy;

...
$validator = new KFold(10);

$score = $validator->test($estimator, $dataset, new Accuracy());

var_dump($score);
```

##### Outputs:
```sh
float(0.869)
```
Below describes the various Cross Validators available in Rubix.

### Hold Out
Hold Out is the simplest form of cross validation available in Rubix. It uses a *hold out* set equal to the size of the given ratio of the entire training set to test the model. The advantages of Hold Out is that it is quick, but it doesn't allow the model to train on the entire training set.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| ratio | 0.2 | float | The ratio of samples to hold out for testing. |

##### Example:
```php
use Rubix\ML\CrossValidation\HoldOut;
use Rubix\ML\CrossValidation\Metrics\Accuracy;

$validator = new HoldOut(0.25);
```

### K Fold
K Fold is a technique that splits the training set into K individual sets and for each training round uses 1 of the folds to measure the validation performance of the model. The score is then averaged over K. For example, a K value of 10 will train and test 10 versions of the model using a different testing set each time.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| k | 10 | int | The number of times to split the training set into equal sized folds. |

##### Example:
```php
use Rubix\ML\CrossValidation\KFold;

$validator = new KFold(5);
```

### Leave P Out
Leave P Out tests the model with a unique holdout set of P samples for each round until all samples have been tested. Note that this process can become slow with large datasets and small values of P.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| p | 10 | int | The number of samples to leave out each round for testing. |

##### Example:
```php
use Rubix\ML\CrossValidation\LeavePOut;

$validator = new LeavePOut(30);
```

### Validation Metrics

Validation metrics are for evaluating the performance of an Estimator given some ground truth such as class labels. The output of the Metric's `score()` method is a scalar score. You can output a tuple of minimum and maximum scores with the `range()` method.

To compute a validation score on an Estimator with a Labeled Dataset:
```php
public score(Estimator $estimator, Labeled $testing) : float
```

To output the range of values the metric can take on:
```php
public range() : array
```

##### Example:
```php
use Rubix\ML\CrossValidation\Metrics\MeanAbsoluteError;

...
$metric = new MeanAbsoluteError();

$score = $metric->score($estimator, $testing);

var_dump($score);
var_dump($metric->range());
```

##### Outputs:
```sh
float(0.99846070553066)

array(2) {
  [0]=>
  float(-INF)
  [1]=>
  int(0)
}
```

There are different metrics for different types of Estimators listed below.

### Classification
| Metric | Range |  Description |
|--|--|--|
| Accuracy | (0, 1) | A quick metric that computes the average accuracy over the entire testing set. |
| F1 Score | (0, 1) | A metric that takes the precision and recall of each class outcome into consideration. |
| Informedness | (0, 1) | Measures the probability of making an informed prediction by looking at the sensitivity and specificity of each class outcome. |
| MCC | (-1, 1) | Matthews Correlation Coefficient is a coefficient between the observed and predicted binary classifications. It returns a value between −1 and +1. A coefficient of +1 represents a perfect prediction, 0 no better than random prediction, and −1 indicates total disagreement between prediction and label. |

### Regression
| Metric | Range | Description |
|--|--|--|
| Mean Absolute Error | (-INF, 0) | The average absolute difference between the actual and predicted values. |
| Mean Squared Error | (-INF, 0) | The average magnitude or squared difference between the actual and predicted values. |
| RMS Error | (-INF, 0) | The root mean squared difference between the actual and predicted values. |
| R-Squared | (-INF, 1) | The R-Squared value, or sometimes called coefficient of determination is the proportion of the variance in the dependent variable that is predictable from the independent variable(s). |

### Clustering
| Metric | Range | Description |
|--|--|--|
| Completeness | (0, 1) | A measure of the class outcomes that are predicted to be in the same cluster. |
| Homogeneity | (0, 1) | A measure of the cluster assignments that are known to be in the same class. |
| V Measure | (0, 1) | The harmonic mean between Homogeneity and Completeness. |

##### Example:
```php
use Rubix\ML\CrossValidation\Metrics\Accuracy;

$metric =  new Accuracy();
```

### Reports
Reports offer deeper insight into the performance of an Estimator than a standard scalar metric. To generate a report, pass an Estimator and a Labeled Dataset to the Report's `generate()` method. The output is an array that can be used to generate visualizations or other useful statistics.

To generate a report:
```php
public generate(Estimator $estimator, Labeled $dataset) : array
```
##### Example:
```php
use Rubix\ML\Classifiers\KNearestNeighbors;
use Rubix\ML\CrossValidation\Reports\ConfusionMatrix;
use Rubix\ML\Datasets\Labeled;

...
$dataset = new Labeled($samples, $labels);

list($training, $testing) = $dataset->randomize()->split(0.8);

$estimator = new KNearestNeighbors(7);

$report = new ConfusionMatrix(['Iris-versicolor', 'Iris-virginica', 'Iris-setosa']);

$estimator->train($training);

$result = $report->generate($estimator, $testing);

var_dump($result);
```

##### Outputs:
```sh
  array(3) {
    ["Iris-versicolor"]=>
    array(3) {
      ["Iris-versicolor"]=>
      int(9)
      ["Iris-virginica"]=>
      int(2)
      ["Iris-setosa"]=>
      int(0)
    }
    ["Iris-virginica"]=>
    array(3) {
      ["Iris-versicolor"]=>
      int(1)
      ["Iris-virginica"]=>
      int(8)
      ["Iris-setosa"]=>
      int(0)
    }
    ["Iris-setosa"]=>
    array(3) {
      ["Iris-versicolor"]=>
      int(0)
      ["Iris-virginica"]=>
      int(0)
      ["Iris-setosa"]=>
      int(10)
    }
  }
```

### Aggregate Report
A Report that aggregates the results of multiple reports.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| reports | None | array | An array of Report objects to aggregate. |

##### Example:
```php
use Rubix\ML\CrossValidation\Reports\AggregateReport;
use Rubix\ML\CrossValidation\Reports\ConfusionMatrix;
use Rubix\ML\CrossValidation\Reports\MulticlassBreakdown;

$report = new AggregateReport([
	new ConfusionMatrix(),
	new MulticlassBreakdown(),
]);
```

### Confusion Matrix
A Confusion Matrix is a table that visualizes the true positives, false, positives, true negatives, and false negatives of a Classifier. The name stems from the fact that the matrix makes it easy to see the classes that the Classifier might be confusing.

##### Classification

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| classes | All | array | The classes to compare in the matrix. |

##### Example:
```php
use Rubix\ML\CrossValidation\Reports\ConfusionMatrix;

...
$report = new ConfusionMatrix(['dog', 'cat', 'turtle']);

$result = $report->generate($estimator, $testing);

var_dump($result);
```

##### Outputs:

```sh
  array(3) {
    ["dog"]=>
    array(2) {
      ["dog"]=> int(842)
      ["cat"]=> int(5)
      ["turtle"]=> int(0)
    }
    ["cat"]=>
    array(2) {
      ["dog"]=> int(0)
      ["cat"]=> int(783)
      ["turtle"]=> int(3)
    }
    ["turtle"]=>
    array(2) {
      ["dog"]=> int(31)
      ["cat"]=> int(79)
      ["turtle"]=> int(496)
    }
  }
```

### Contingency Table

A Contingency Table is used to display the frequency distribution of class labels among a clustering of samples.

##### Clustering

##### Parameters:
This Report does not have any parameters.

##### Example:
```php
use Rubix\ML\CrossValidation\Reports\ContingencyTable;

...
$report = new ContingencyTable();

$result = $report->generate($estimator, $testing);

var_dump($result);
```

##### Outputs:
```sh
array(3) {
    [1]=>
    array(3) {
      [1]=> int(13)
      [2]=> int(0)
      [3]=> int(2)
    }
    [2]=>
    array(3) {
      [1]=> int(1)
      [2]=> int(0)
      [3]=> int(12)
    }
    [0]=>
    array(3) {
      [1]=> int(0)
      [2]=> int(14)
      [3]=> int(0)
    }
  }
```

### Multiclass Breakdown
A Report that drills down in to each unique class outcome. The report includes metrics such as Accuracy, F1 Score, MCC, Precision, Recall, Cardinality, Miss Rate, and more.

##### Classification

##### Parameters:
This Report does not have any parameters.

##### Example:
```php
use Rubix\ML\CrossValidation\Reports\MulticlassBreakdown;

...
$report = new MulticlassBreakdown();

$result = $report->generate($estimator, $testing);

var_dump($result);
```

##### Outputs:
```sh
...
    array(2) {
      ["benign"]=>
      array(15) {
        ["accuracy"]=> int(1)
        ["precision"]=> float(0.99999999998723)
        ["recall"]=> float(0.99999999998723)
        ["specificity"]=> float(0.99999999998812)
        ["miss_rate"]=> float(1.2771450563775E-11)
        ["fall_out"]=> float(1.1876499783625E-11)
        ["f1_score"]=> float(0.99999999498723)
        ["mcc"]=> float(0.99999999999998)
        ["informedness"]=> float(0.99999999997535)
        ["true_positives"]=> int(783)
        ["true_negatives"]=> int(842)
        ["false_positives"]=> int(0)
        ["false_negatives"]=> int(0)
        ["cardinality"]=> int(783)
        ["density"]=> float(0.48184615384615)
      }
...
```

### Outlier Ratio
Outlier Ratio is the proportion of outliers to inliers in an [Anomaly Detection](#anomaly-detectors) problem.

##### Anomaly Detection

##### Parameters:
This Report does not have any parameters.

##### Example:
```php
use Rubix\ML\CrossValidation\Reports\OutlierRatio;

...
$report = new OutlierRatio();

$result = $report->generate($estimator, $testing);

var_dump($result);
```

##### Outputs:
```sh
  array(4) {
    ["outliers"]=> int(13)
    ["inliers"]=> int(307)
    ["ratio"]=> float(0.042345276871585)
    ["cardinality"]=> int(320)
  }
```

### Prediction Speed
This Report measures the number of predictions an Estimator can make per second as given by the PPS (predictions per second) score.

##### Classification, Regression, Clustering, Anomaly Detection

##### Parameters:
This Report does not have any parameters.

##### Example:
```php
use Rubix\ML\CrossValidation\Reports\PredictionSpeed;

...
$report = new PredictionSpeed();

$result = $report->generate($estimator, $testing);

var_dump($result);
```

##### Outputs:
```sh
  array(3) {
    ["pps"]=> float(2998.8272123503)
    ["average_sec"]=> float(0.00033346366287096)
    ["total_sec"]=> float(0.10704183578491)
  }
```

### Residual Analysis
Residual Analysis is a type of Report that measures the total differences between the predicted and actual values of a Regression.

##### Regression

##### Parameters:
This Report does not have any parameters.

##### Example:
```php
use Rubix\ML\CrossValidation\Reports\ResidualAnalysis;

...
$report = new ResidualAnalysis();

$result = $report->generate($estimator, $testing);

var_dump($result);
```

##### Outputs:
```sh
  array(4) {
    ["mean_absolute_error"]=> float(0.60076863576779)
    ["mean_squared_error"]=> float(0.6170496114073)
    ["rms_error"]=> float(0.78552505460189)
    ["r_squared"]=> float(0.52063215677)
  }
```

---
### Model Selection
Model selection is the task of selecting a version of a model with a hyperparameter combination that maximizes performance on a specific validation metric. Rubix provides the *Grid Search* meta-Estimator that performs an exhaustive search over all combinations of parameters given as possible arguments.

### Grid Search
Grid Search is an algorithm that optimizes hyperparameter selection. From the user's perspective, the process of training and predicting is the same, however, under the hood, Grid Search trains one [Estimator](#estimators) per combination of parameters and predictions are made using the best Estimator. You can access the scores for each parameter combination by calling the `results()` method on the trained Grid Search meta-Estimator or you can get the best parameters by calling `best()`.

##### Parameters:
| Param | Default | Type | Description |
|--|--|--|--|
| base | None | string | The fully qualified class name of the base Estimator. |
| params | [ ] | array | An array containing n-tuples (represented as PHP arrays) of parameters to search across for a given constructor argument position. |
| metric | None | object | The validation metric used to score each set of parameters. |
| validator | None | object | An instance of a Validator object (HoldOut, KFold, etc.) that will be used to test each parameter combination. |

##### Additional Methods:
| Method | Description |
|--|--|
| `results() : array` | An array containing the score of each combination of parameters. |
| `best() : array` | The parameters with the highest validation score. |
| `estimator() : Estimator` | The underlying estimator trained with the best parameter combination. |

##### Example:
```php
use Rubix\ML\GridSearch;
use Rubix\ML\Classifiers\KNearestNeighbors;
use Rubix\ML\Kernels\Distance\Euclidean;
use Rubix\ML\Kernels\Distance\Manhattan;
use Rubix\ML\CrossValidation\Metrics\Accuracy;
use Rubix\ML\CrossValidation\KFold;

...
$params = [
	[1, 3, 5, 10], [new Euclidean(), new Manhattan()],
];

$estimator = new GridSearch(KNearestNeightbors::class, $params, new Accuracy(), new KFold(10));

$estimator->train($dataset);

var_dump($estimator->best()); // Return the best parameters
```

##### Outputs:
```sh
array(2) {
  [0]=> int(5)
  [1]=> object(Rubix\ML\Kernels\Distance\Euclidean)#15 (0) {
  }
}
```

---
### Model Persistence
Model persistence is the practice of saving a trained model to disk so that it can be restored later, on a different machine, or used in an online system. Rubix persists your models using built in object serialization (similar to pickling in Python). Most Estimators are persistable, but some are not allowed due to their poor storage complexity.

### Persistent Model
It is possible to persist a model to disk by wrapping the Estimator instance in a Persistent Model meta-Estimator. The Persistent Model class gives the Estimator two additional methods `save()` and `restore()` that serialize and unserialize to and from disk. In order to be persisted the Estimator must implement the Persistable interface.

```php
public save(string $path) : bool
```
Where path is the location of the directory where you want the model saved. `save()` will return true if the model was successfully persisted and false if it failed.

```php
public static restore(string $path) : self
```
The restore method will return an instantiated model from the save path.

##### Example:
```php
use Rubix\ML\PersistentModel;
use Rubix\ML\Classifiers\RandomForest;

$estimator = new PersistentModel(new RandomForest(100, 0.2, 10, 3));

$estimator->save('path/to/models/folder/');

$estimator->save(); // Saves to current working directory

$estimator = PersistentModel::restore('path/to/persisted/model');
```
