<!--
This file is generated by a tool. Do not edit directly.
For open-source contributions the docs will be updated automatically.
-->

*Last updated: 2022-11-30.*

<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tf_quant_finance.math.qmc.random_scrambling_vectors" />
<meta itemprop="path" content="Stable" />
</div>

# tf_quant_finance.math.qmc.random_scrambling_vectors

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/math/qmc/lattice_rule.py">View source</a>



Returns a `Tensor` drawn from a uniform distribution.

```python
tf_quant_finance.math.qmc.random_scrambling_vectors(
    dim, seed, validate_args=False, dtype=None, name=None
)
```



<!-- Placeholder for "Used in" -->

The returned `Tensor` can be can be added to the specified
`generating_vectors` in order to randomize it.

#### Examples

```python
import tf_quant_finance as tff

# Example: Creating random vectors which can scramble 2D generating vectors.

dim = 2
seed = (2, 3)

tff.math.qmc.random_scrambling_vectors(dim, seed=seed)
# ==> tf.Tensor([0.17481351, 0.9780868], shape=(2,), dtype=float32)
```

#### Args:


* <b>`dim`</b>: Positive scalar `Tensor` of integers with rank 0. The event size of
  points which can be sampled from the generating vectors to scramble.
* <b>`seed`</b>: Positive scalar `Tensor` with shape [2] and dtype `int32` used as seed
  for the random generator.
* <b>`validate_args`</b>: Python `bool` indicating whether to validate arguments.
  Default value: `False`.
* <b>`dtype`</b>: Optional `dtype`. The `dtype` of the output `Tensor` (either
  `float32` or `float64`).
  Default value: `None` which maps to `tf.float32`.
* <b>`name`</b>: Python `str` name prefixed to ops created by this function.
  Default value: `None` which maps to `random_scrambling_vectors`.


#### Returns:

A `Tensor` of real values between 0 (incl.) and 1 (excl.) with `shape`
`(dim,)`.