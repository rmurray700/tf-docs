

page_type: reference


<!-- DO NOT EDIT! Automatically generated file. -->


# tf.distributions.Multinomial

## Class `Multinomial`

Inherits From: [`Distribution`](../../tf/distributions/Distribution)

### Aliases:

* Class `tf.contrib.distributions.Multinomial`
* Class `tf.distributions.Multinomial`



Defined in [`tensorflow/python/ops/distributions/multinomial.py`](https://www.github.com/tensorflow/tensorflow/blob/r1.3/tensorflow/python/ops/distributions/multinomial.py).

See the guide: [Statistical Distributions (contrib) > Multivariate distributions](../../../../api_guides/python/contrib.distributions#Multivariate_distributions)

Multinomial distribution.

This Multinomial distribution is parameterized by `probs`, a (batch of)
length-`K` `prob` (probability) vectors (`K > 1`) such that
`tf.reduce_sum(probs, -1) = 1`, and a `total_count` number of trials, i.e.,
the number of trials per draw from the Multinomial. It is defined over a
(batch of) length-`K` vector `counts` such that
`tf.reduce_sum(counts, -1) = total_count`. The Multinomial is identically the
Binomial distribution when `K = 2`.

#### Mathematical Details

The Multinomial is a distribution over `K`-class counts, i.e., a length-`K`
vector of non-negative integer `counts = n = [n_0, ..., n_{K-1}]`.

The probability mass function (pmf) is,

```none
pmf(n; pi, N) = prod_j (pi_j)**n_j / Z
Z = (prod_j n_j!) / N!
```

where:
* `probs = pi = [pi_0, ..., pi_{K-1}]`, `pi_j > 0`, `sum_j pi_j = 1`,
* `total_count = N`, `N` a positive integer,
* `Z` is the normalization constant, and,
* `N!` denotes `N` factorial.

Distribution parameters are automatically broadcast in all functions; see
examples for details.

#### Pitfalls

The number of classes, `K`, must not exceed:
- the largest integer representable by `self.dtype`, i.e.,
  `2**(mantissa_bits+1)` (IEE754),
- the maximum `Tensor` index, i.e., `2**31-1`.

In other words,

```python
K <= min(2**31-1, {
  tf.float16: 2**11,
  tf.float32: 2**24,
  tf.float64: 2**53 }[param.dtype])
```

Note: This condition is validated only when `self.validate_args = True`.

#### Examples

Create a 3-class distribution, with the 3rd class is most likely to be drawn,
using logits.

```python
logits = [-50., -43, 0]
dist = Multinomial(total_count=4., logits=logits)
```

Create a 3-class distribution, with the 3rd class is most likely to be drawn.

```python
p = [.2, .3, .5]
dist = Multinomial(total_count=4., probs=p)
```

The distribution functions can be evaluated on counts.

```python
# counts same shape as p.
counts = [1., 0, 3]
dist.prob(counts)  # Shape []

# p will be broadcast to [[.2, .3, .5], [.2, .3, .5]] to match counts.
counts = [[1., 2, 1], [2, 2, 0]]
dist.prob(counts)  # Shape [2]

# p will be broadcast to shape [5, 7, 3] to match counts.
counts = [[...]]  # Shape [5, 7, 3]
dist.prob(counts)  # Shape [5, 7]
```

Create a 2-batch of 3-class distributions.

```python
p = [[.1, .2, .7], [.3, .3, .4]]  # Shape [2, 3]
dist = Multinomial(total_count=[4., 5], probs=p)

counts = [[2., 1, 1], [3, 1, 1]]
dist.prob(counts)  # Shape [2]
```

## Properties

<h3 id="allow_nan_stats"><code>allow_nan_stats</code></h3>

Python `bool` describing behavior when a stat is undefined.

Stats return +/- infinity when it makes sense. E.g., the variance of a
Cauchy distribution is infinity. However, sometimes the statistic is
undefined, e.g., if a distribution's pdf does not achieve a maximum within
the support of the distribution, the mode is undefined. If the mean is
undefined, then by definition the variance is undefined. E.g. the mean for
Student's T for df = 1 is undefined (no clear way to say it is either + or -
infinity), so the variance = E[(X - mean)**2] is also undefined.

#### Returns:

* <b>`allow_nan_stats`</b>: Python `bool`.

<h3 id="batch_shape"><code>batch_shape</code></h3>

Shape of a single sample from a single event index as a `TensorShape`.

May be partially defined or unknown.

The batch dimensions are indexes into independent, non-identical
parameterizations of this distribution.

#### Returns:

* <b>`batch_shape`</b>: `TensorShape`, possibly unknown.

<h3 id="dtype"><code>dtype</code></h3>

The `DType` of `Tensor`s handled by this `Distribution`.

<h3 id="event_shape"><code>event_shape</code></h3>

Shape of a single sample from a single batch as a `TensorShape`.

May be partially defined or unknown.

#### Returns:

* <b>`event_shape`</b>: `TensorShape`, possibly unknown.

<h3 id="logits"><code>logits</code></h3>

Vector of coordinatewise logits.

<h3 id="name"><code>name</code></h3>

Name prepended to all ops created by this `Distribution`.

<h3 id="parameters"><code>parameters</code></h3>

Dictionary of parameters used to instantiate this `Distribution`.

<h3 id="probs"><code>probs</code></h3>

Probability of drawing a `1` in that coordinate.

<h3 id="reparameterization_type"><code>reparameterization_type</code></h3>

Describes how samples from the distribution are reparameterized.

Currently this is one of the static instances
`distributions.FULLY_REPARAMETERIZED`
or `distributions.NOT_REPARAMETERIZED`.

#### Returns:

  An instance of `ReparameterizationType`.

<h3 id="total_count"><code>total_count</code></h3>

Number of trials used to construct a sample.

<h3 id="validate_args"><code>validate_args</code></h3>

Python `bool` indicating possibly expensive checks are enabled.



## Methods

<h3 id="__init__"><code>__init__</code></h3>

``` python
__init__(
    total_count,
    logits=None,
    probs=None,
    validate_args=False,
    allow_nan_stats=True,
    name='Multinomial'
)
```

Initialize a batch of Multinomial distributions.

#### Args:

* <b>`total_count`</b>: Non-negative floating point tensor with shape broadcastable
    to `[N1,..., Nm]` with `m >= 0`. Defines this as a batch of
    `N1 x ... x Nm` different Multinomial distributions. Its components
    should be equal to integer values.
* <b>`logits`</b>: Floating point tensor representing unnormalized log-probabilities
    of a positive event with shape broadcastable to
    `[N1,..., Nm, K]` `m >= 0`, and the same dtype as `total_count`. Defines
    this as a batch of `N1 x ... x Nm` different `K` class Multinomial
    distributions. Only one of `logits` or `probs` should be passed in.
* <b>`probs`</b>: Positive floating point tensor with shape broadcastable to
    `[N1,..., Nm, K]` `m >= 0` and same dtype as `total_count`. Defines
    this as a batch of `N1 x ... x Nm` different `K` class Multinomial
    distributions. `probs`'s components in the last portion of its shape
    should sum to `1`. Only one of `logits` or `probs` should be passed in.
* <b>`validate_args`</b>: Python `bool`, default `False`. When `True` distribution
    parameters are checked for validity despite possibly degrading runtime
    performance. When `False` invalid inputs may silently render incorrect
    outputs.
* <b>`allow_nan_stats`</b>: Python `bool`, default `True`. When `True`, statistics
    (e.g., mean, mode, variance) use the value "`NaN`" to indicate the
    result is undefined. When `False`, an exception is raised if one or
    more of the statistic's batch members are undefined.
* <b>`name`</b>: Python `str` name prefixed to Ops created by this class.

<h3 id="batch_shape_tensor"><code>batch_shape_tensor</code></h3>

``` python
batch_shape_tensor(name='batch_shape_tensor')
```

Shape of a single sample from a single event index as a 1-D `Tensor`.

The batch dimensions are indexes into independent, non-identical
parameterizations of this distribution.

#### Args:

* <b>`name`</b>: name to give to the op


#### Returns:

* <b>`batch_shape`</b>: `Tensor`.

<h3 id="cdf"><code>cdf</code></h3>

``` python
cdf(
    value,
    name='cdf'
)
```

Cumulative distribution function.

Given random variable `X`, the cumulative distribution function `cdf` is:

```none
cdf(x) := P[X <= x]
```

#### Args:

* <b>`value`</b>: `float` or `double` `Tensor`.
* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`cdf`</b>: a `Tensor` of shape `sample_shape(x) + self.batch_shape` with
    values of type `self.dtype`.

<h3 id="copy"><code>copy</code></h3>

``` python
copy(**override_parameters_kwargs)
```

Creates a deep copy of the distribution.

Note: the copy distribution may continue to depend on the original
initialization arguments.

#### Args:

  **override_parameters_kwargs: String/value dictionary of initialization
    arguments to override with new values.


#### Returns:

* <b>`distribution`</b>: A new instance of `type(self)` initialized from the union
    of self.parameters and override_parameters_kwargs, i.e.,
    `dict(self.parameters, **override_parameters_kwargs)`.

<h3 id="covariance"><code>covariance</code></h3>

``` python
covariance(name='covariance')
```

Covariance.

Covariance is (possibly) defined only for non-scalar-event distributions.

For example, for a length-`k`, vector-valued distribution, it is calculated
as,

```none
Cov[i, j] = Covariance(X_i, X_j) = E[(X_i - E[X_i]) (X_j - E[X_j])]
```

where `Cov` is a (batch of) `k x k` matrix, `0 <= (i, j) < k`, and `E`
denotes expectation.

Alternatively, for non-vector, multivariate distributions (e.g.,
matrix-valued, Wishart), `Covariance` shall return a (batch of) matrices
under some vectorization of the events, i.e.,

```none
Cov[i, j] = Covariance(Vec(X)_i, Vec(X)_j) = [as above]
```

where `Cov` is a (batch of) `k' x k'` matrices,
`0 <= (i, j) < k' = reduce_prod(event_shape)`, and `Vec` is some function
mapping indices of this distribution's event dimensions to indices of a
length-`k'` vector.

#### Args:

* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`covariance`</b>: Floating-point `Tensor` with shape `[B1, ..., Bn, k', k']`
    where the first `n` dimensions are batch coordinates and
    `k' = reduce_prod(self.event_shape)`.

<h3 id="entropy"><code>entropy</code></h3>

``` python
entropy(name='entropy')
```

Shannon entropy in nats.

<h3 id="event_shape_tensor"><code>event_shape_tensor</code></h3>

``` python
event_shape_tensor(name='event_shape_tensor')
```

Shape of a single sample from a single batch as a 1-D int32 `Tensor`.

#### Args:

* <b>`name`</b>: name to give to the op


#### Returns:

* <b>`event_shape`</b>: `Tensor`.

<h3 id="is_scalar_batch"><code>is_scalar_batch</code></h3>

``` python
is_scalar_batch(name='is_scalar_batch')
```

Indicates that `batch_shape == []`.

#### Args:

* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`is_scalar_batch`</b>: `bool` scalar `Tensor`.

<h3 id="is_scalar_event"><code>is_scalar_event</code></h3>

``` python
is_scalar_event(name='is_scalar_event')
```

Indicates that `event_shape == []`.

#### Args:

* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`is_scalar_event`</b>: `bool` scalar `Tensor`.

<h3 id="log_cdf"><code>log_cdf</code></h3>

``` python
log_cdf(
    value,
    name='log_cdf'
)
```

Log cumulative distribution function.

Given random variable `X`, the cumulative distribution function `cdf` is:

```none
log_cdf(x) := Log[ P[X <= x] ]
```

Often, a numerical approximation can be used for `log_cdf(x)` that yields
a more accurate answer than simply taking the logarithm of the `cdf` when
`x << -1`.

#### Args:

* <b>`value`</b>: `float` or `double` `Tensor`.
* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`logcdf`</b>: a `Tensor` of shape `sample_shape(x) + self.batch_shape` with
    values of type `self.dtype`.

<h3 id="log_prob"><code>log_prob</code></h3>

``` python
log_prob(
    value,
    name='log_prob'
)
```

Log probability density/mass function.


Additional documentation from `Multinomial`:

For each batch of counts, `value = [n_0, ...
,n_{k-1}]`, `P[value]` is the probability that after sampling `self.total_count`
draws from this Multinomial distribution, the number of draws falling in class
`j` is `n_j`. Since this definition is [exchangeable](
https://en.wikipedia.org/wiki/Exchangeable_random_variables); different
sequences have the same counts so the probability includes a combinatorial
coefficient.

Note: `value` must be a non-negative tensor with dtype `self.dtype`, have no
fractional components, and such that
`tf.reduce_sum(value, -1) = self.total_count`. Its shape must be broadcastable
with `self.probs` and `self.total_count`.

#### Args:

* <b>`value`</b>: `float` or `double` `Tensor`.
* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`log_prob`</b>: a `Tensor` of shape `sample_shape(x) + self.batch_shape` with
    values of type `self.dtype`.

<h3 id="log_survival_function"><code>log_survival_function</code></h3>

``` python
log_survival_function(
    value,
    name='log_survival_function'
)
```

Log survival function.

Given random variable `X`, the survival function is defined:

```none
log_survival_function(x) = Log[ P[X > x] ]
                         = Log[ 1 - P[X <= x] ]
                         = Log[ 1 - cdf(x) ]
```

Typically, different numerical approximations can be used for the log
survival function, which are more accurate than `1 - cdf(x)` when `x >> 1`.

#### Args:

* <b>`value`</b>: `float` or `double` `Tensor`.
* <b>`name`</b>: The name to give this op.


#### Returns:

  `Tensor` of shape `sample_shape(x) + self.batch_shape` with values of type
    `self.dtype`.

<h3 id="mean"><code>mean</code></h3>

``` python
mean(name='mean')
```

Mean.

<h3 id="mode"><code>mode</code></h3>

``` python
mode(name='mode')
```

Mode.

<h3 id="param_shapes"><code>param_shapes</code></h3>

``` python
param_shapes(
    cls,
    sample_shape,
    name='DistributionParamShapes'
)
```

Shapes of parameters given the desired shape of a call to `sample()`.

This is a class method that describes what key/value arguments are required
to instantiate the given `Distribution` so that a particular shape is
returned for that instance's call to `sample()`.

Subclasses should override class method `_param_shapes`.

#### Args:

* <b>`sample_shape`</b>: `Tensor` or python list/tuple. Desired shape of a call to
    `sample()`.
* <b>`name`</b>: name to prepend ops with.


#### Returns:

  `dict` of parameter name to `Tensor` shapes.

<h3 id="param_static_shapes"><code>param_static_shapes</code></h3>

``` python
param_static_shapes(
    cls,
    sample_shape
)
```

param_shapes with static (i.e. `TensorShape`) shapes.

This is a class method that describes what key/value arguments are required
to instantiate the given `Distribution` so that a particular shape is
returned for that instance's call to `sample()`. Assumes that the sample's
shape is known statically.

Subclasses should override class method `_param_shapes` to return
constant-valued tensors when constant values are fed.

#### Args:

* <b>`sample_shape`</b>: `TensorShape` or python list/tuple. Desired shape of a call
    to `sample()`.


#### Returns:

  `dict` of parameter name to `TensorShape`.


#### Raises:

* <b>`ValueError`</b>: if `sample_shape` is a `TensorShape` and is not fully defined.

<h3 id="prob"><code>prob</code></h3>

``` python
prob(
    value,
    name='prob'
)
```

Probability density/mass function.

#### Args:

* <b>`value`</b>: `float` or `double` `Tensor`.
* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`prob`</b>: a `Tensor` of shape `sample_shape(x) + self.batch_shape` with
    values of type `self.dtype`.

<h3 id="quantile"><code>quantile</code></h3>

``` python
quantile(
    value,
    name='quantile'
)
```

Quantile function. Aka "inverse cdf" or "percent point function".

Given random variable `X` and `p in [0, 1]`, the `quantile` is:

```none
quantile(p) := x such that P[X <= x] == p
```

#### Args:

* <b>`value`</b>: `float` or `double` `Tensor`.
* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`quantile`</b>: a `Tensor` of shape `sample_shape(x) + self.batch_shape` with
    values of type `self.dtype`.

<h3 id="sample"><code>sample</code></h3>

``` python
sample(
    sample_shape=(),
    seed=None,
    name='sample'
)
```

Generate samples of the specified shape.

Note that a call to `sample()` without arguments will generate a single
sample.

#### Args:

* <b>`sample_shape`</b>: 0D or 1D `int32` `Tensor`. Shape of the generated samples.
* <b>`seed`</b>: Python integer seed for RNG
* <b>`name`</b>: name to give to the op.


#### Returns:

* <b>`samples`</b>: a `Tensor` with prepended dimensions `sample_shape`.

<h3 id="stddev"><code>stddev</code></h3>

``` python
stddev(name='stddev')
```

Standard deviation.

Standard deviation is defined as,

```none
stddev = E[(X - E[X])**2]**0.5
```

where `X` is the random variable associated with this distribution, `E`
denotes expectation, and `stddev.shape = batch_shape + event_shape`.

#### Args:

* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`stddev`</b>: Floating-point `Tensor` with shape identical to
    `batch_shape + event_shape`, i.e., the same shape as `self.mean()`.

<h3 id="survival_function"><code>survival_function</code></h3>

``` python
survival_function(
    value,
    name='survival_function'
)
```

Survival function.

Given random variable `X`, the survival function is defined:

```none
survival_function(x) = P[X > x]
                     = 1 - P[X <= x]
                     = 1 - cdf(x).
```

#### Args:

* <b>`value`</b>: `float` or `double` `Tensor`.
* <b>`name`</b>: The name to give this op.


#### Returns:

  `Tensor` of shape `sample_shape(x) + self.batch_shape` with values of type
    `self.dtype`.

<h3 id="variance"><code>variance</code></h3>

``` python
variance(name='variance')
```

Variance.

Variance is defined as,

```none
Var = E[(X - E[X])**2]
```

where `X` is the random variable associated with this distribution, `E`
denotes expectation, and `Var.shape = batch_shape + event_shape`.

#### Args:

* <b>`name`</b>: The name to give this op.


#### Returns:

* <b>`variance`</b>: Floating-point `Tensor` with shape identical to
    `batch_shape + event_shape`, i.e., the same shape as `self.mean()`.


