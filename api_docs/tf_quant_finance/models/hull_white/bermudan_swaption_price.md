<!--
This file is generated by a tool. Do not edit directly.
For open-source contributions the docs will be updated automatically.
-->

*Last updated: 2022-11-30.*

<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tf_quant_finance.models.hull_white.bermudan_swaption_price" />
<meta itemprop="path" content="Stable" />
</div>

# tf_quant_finance.models.hull_white.bermudan_swaption_price

<!-- Insert buttons and diff -->

<table class="tfo-notebook-buttons tfo-api" align="left">
</table>

<a target="_blank" href="https://github.com/google/tf-quant-finance/blob/master/tf_quant_finance/models/hull_white/swaption.py">View source</a>



Calculates the price of Bermudan Swaptions using the Hull-White model.

```python
tf_quant_finance.models.hull_white.bermudan_swaption_price(
    *, exercise_times, floating_leg_start_times, floating_leg_end_times,
    fixed_leg_payment_times, floating_leg_daycount_fractions,
    fixed_leg_daycount_fractions, fixed_leg_coupon, reference_rate_fn,
    mean_reversion, volatility, notional=None, is_payer_swaption=True,
    use_finite_difference=False, lsm_basis=None, num_samples=100, random_type=None,
    seed=None, skip=0, time_step=None, time_step_finite_difference=None,
    num_grid_points_finite_difference=101, dtype=None, name=None
)
```



<!-- Placeholder for "Used in" -->

A Bermudan Swaption is a contract that gives the holder an option to enter a
swap contract on a set of future exercise dates. The exercise dates are
typically the fixing dates (or a subset thereof) of the underlying swap. If
`T_N` denotes the final payoff date and `T_i, i = {1,...,n}` denote the set
of exercise dates, then if the option is exercised at `T_i`, the holder is
left with a swap with first fixing date equal to `T_i` and maturity `T_N`.

Simulation based pricing of Bermudan swaptions is performed using the least
squares Monte-carlo approach [1].

#### References:
  [1]: D. Brigo, F. Mercurio. Interest Rate Models-Theory and Practice.
  Second Edition. 2007.

#### Example
The example shows how value a batch of 5-no-call-1 and 5-no-call-2
swaptions using the Hull-White model.

````python
import numpy as np
import tensorflow.compat.v2 as tf
import tf_quant_finance as tff

dtype = tf.float64

exercise_swaption_1 = [1.0, 1.5, 2.0, 2.5, 3.0, 3.5, 4.0, 4.5]
exercise_swaption_2 = [2.0, 2.5, 3.0, 3.5, 4.0, 4.5, 5.0, 5.0]
exercise_times = [exercise_swaption_1, exercise_swaption_2]

float_leg_start_times_1y = [1.0, 1.5, 2.0, 2.5, 3.0, 3.5, 4.0, 4.5]
float_leg_start_times_18m = [1.5, 2.0, 2.5, 3.0, 3.5, 4.0, 4.5, 5.0]
float_leg_start_times_2y = [2.0, 2.5, 3.0, 3.5, 4.0, 4.5, 5.0, 5.0]
float_leg_start_times_30m = [2.5, 3.0, 3.5, 4.0, 4.5, 5.0, 5.0, 5.0]
float_leg_start_times_3y = [3.0, 3.5, 4.0, 4.5, 5.0, 5.0, 5.0, 5.0]
float_leg_start_times_42m = [3.5, 4.0, 4.5, 5.0, 5.0, 5.0, 5.0, 5.0]
float_leg_start_times_4y = [4.0, 4.5, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0]
float_leg_start_times_54m = [4.5, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0]
float_leg_start_times_5y = [5.0, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0, 5.0]

float_leg_start_times_swaption_1 = [float_leg_start_times_1y,
                                    float_leg_start_times_18m,
                                    float_leg_start_times_2y,
                                    float_leg_start_times_30m,
                                    float_leg_start_times_3y,
                                    float_leg_start_times_42m,
                                    float_leg_start_times_4y,
                                    float_leg_start_times_54m]

float_leg_start_times_swaption_2 = [float_leg_start_times_2y,
                                    float_leg_start_times_30m,
                                    float_leg_start_times_3y,
                                    float_leg_start_times_42m,
                                    float_leg_start_times_4y,
                                    float_leg_start_times_54m,
                                    float_leg_start_times_5y,
                                    float_leg_start_times_5y]
float_leg_start_times = [float_leg_start_times_swaption_1,
                       float_leg_start_times_swaption_2]

float_leg_end_times = np.clip(np.array(float_leg_start_times) + 0.5, 0.0, 5.0)

fixed_leg_payment_times = float_leg_end_times
float_leg_daycount_fractions = (np.array(float_leg_end_times) -
                                np.array(float_leg_start_times))
fixed_leg_daycount_fractions = float_leg_daycount_fractions
fixed_leg_coupon = 0.011 * np.ones_like(fixed_leg_payment_times)
zero_rate_fn = lambda x: 0.01 * tf.ones_like(x, dtype=dtype)
price = bermudan_swaption_price(
    exercise_times=exercise_times,
    floating_leg_start_times=float_leg_start_times,
    floating_leg_end_times=float_leg_end_times,
    fixed_leg_payment_times=fixed_leg_payment_times,
    floating_leg_daycount_fractions=float_leg_daycount_fractions,
    fixed_leg_daycount_fractions=fixed_leg_daycount_fractions,
    fixed_leg_coupon=fixed_leg_coupon,
    reference_rate_fn=zero_rate_fn,
    notional=100.,
    mean_reversion=[0.03],
    volatility=[0.01],
    num_samples=1000000,
    time_step=0.1,
    random_type=tff.math.random.RandomType.PSEUDO_ANTITHETIC,
    seed=0,
    dtype=dtype)
# Expected value: [1.8913050118443016, 1.6618681421434984] # shape = (2,)
````

#### Args:


* <b>`exercise_times`</b>: A real `Tensor` of any shape `[batch_size, num_exercise]`
  `and real dtype. The times corresponding to exercise dates of the
  swaptions. `num_exercise` corresponds to the number of exercise dates for
  the Bermudan swaption. The shape of this input determines the number (and
  shape) of Bermudan swaptions to be priced and the shape of the output.
* <b>`floating_leg_start_times`</b>: A real `Tensor` of the same dtype as
  `exercise_times`. The times when accrual begins for each payment in the
  floating leg upon exercise of the option. The shape of this input should
  be `exercise_times.shape + [m]` where `m` denotes the number of floating
  payments in each leg of the underlying swap until the swap maturity.
* <b>`floating_leg_end_times`</b>: A real `Tensor` of the same dtype as
  `exercise_times`. The times when accrual ends for each payment in the
  floating leg upon exercise of the option. The shape of this input should
  be `exercise_times.shape + [m]` where `m` denotes the number of floating
  payments in each leg of the underlying swap until the swap maturity.
* <b>`fixed_leg_payment_times`</b>: A real `Tensor` of the same dtype as
  `exercise_times`. The payment times for each payment in the fixed leg.
  The shape of this input should be `exercise_times.shape + [n]` where `n`
  denotes the number of fixed payments in each leg of the underlying swap
  until the swap maturity.
* <b>`floating_leg_daycount_fractions`</b>: A real `Tensor` of the same dtype and
  compatible shape as `floating_leg_start_times`. The daycount fractions
  for each payment in the floating leg.
* <b>`fixed_leg_daycount_fractions`</b>: A real `Tensor` of the same dtype and
  compatible shape as `fixed_leg_payment_times`. The daycount fractions
  for each payment in the fixed leg.
* <b>`fixed_leg_coupon`</b>: A real `Tensor` of the same dtype and compatible shape
  as `fixed_leg_payment_times`. The fixed rate for each payment in the
  fixed leg.
* <b>`reference_rate_fn`</b>: A Python callable that accepts expiry time as a real
  `Tensor` and returns a `Tensor` of either shape `input_shape` or
  `input_shape`. Returns the continuously compounded zero rate at
  the present time for the input expiry time.
* <b>`mean_reversion`</b>: A real positive scalar `Tensor` or a Python callable. The
  callable can be one of the following:
  (a) A left-continuous piecewise constant object (e.g.,
  `tff.math.piecewise.PiecewiseConstantFunc`) that has a property
  `is_piecewise_constant` set to `True`. In this case the object should
  have a method `jump_locations(self)` that returns a `Tensor` of shape
  `[num_jumps]`. The return value of `mean_reversion(t)` should return a
  `Tensor` of shape `t.shape`, `t` is a rank 1 `Tensor` of the same `dtype`
  as the output. See example in the class docstring.
  (b) A callable that accepts scalars (stands for time `t`) and returns a
  scalar `Tensor` of the same `dtype` as `strikes`.
  Corresponds to the mean reversion rate.
* <b>`volatility`</b>: A real positive `Tensor` of the same `dtype` as
  `mean_reversion` or a callable with the same specs as above.
  Corresponds to the long run price variance.
* <b>`notional`</b>: An optional `Tensor` of same dtype and compatible shape as
  `strikes`specifying the notional amount for the underlying swap.
   Default value: None in which case the notional is set to 1.
* <b>`is_payer_swaption`</b>: A boolean `Tensor` of a shape compatible with `expiries`.
  Indicates whether the swaption is a payer (if True) or a receiver
  (if False) swaption. If not supplied, payer swaptions are assumed.
* <b>`use_finite_difference`</b>: A Python boolean specifying if the valuation should
  be performed using the finite difference and PDE.
  Default value: `False`, in which case valuation is performed using least
  squares monte-carlo method.
* <b>`lsm_basis`</b>: A Python callable specifying the basis to be used in the LSM
  algorithm. The callable must accept a `Tensor`s of shape
  `[num_samples, dim]` and output `Tensor`s of shape `[m, num_samples]`
  where `m` is the nimber of basis functions used. This input is only used
  for valuation using LSM.
  Default value: `None`, in which case a polynomial basis of order 2 is
  used.
* <b>`num_samples`</b>: Positive scalar `int32` `Tensor`. The number of simulation
  paths during Monte-Carlo valuation. This input is only used for valuation
  using LSM.
  Default value: The default value is 100.
* <b>`random_type`</b>: Enum value of `RandomType`. The type of (quasi)-random
  number generator to use to generate the simulation paths. This input is
  only used for valuation using LSM.
  Default value: `None` which maps to the standard pseudo-random numbers.
* <b>`seed`</b>: Seed for the random number generator. The seed is only relevant if
  `random_type` is one of
  `[STATELESS, PSEUDO, HALTON_RANDOMIZED, PSEUDO_ANTITHETIC,
    STATELESS_ANTITHETIC]`. For `PSEUDO`, `PSEUDO_ANTITHETIC` and
  `HALTON_RANDOMIZED` the seed should be an Python integer. For
  `STATELESS` and  `STATELESS_ANTITHETIC `must be supplied as an integer
  `Tensor` of shape `[2]`. This input is only used for valuation using LSM.
  Default value: `None` which means no seed is set.
* <b>`skip`</b>: `int32` 0-d `Tensor`. The number of initial points of the Sobol or
  Halton sequence to skip. Used only when `random_type` is 'SOBOL',
  'HALTON', or 'HALTON_RANDOMIZED', otherwise ignored. This input is only
  used for valuation using LSM.
  Default value: `0`.
* <b>`time_step`</b>: Scalar real `Tensor`. Maximal distance between time grid points
  in Euler scheme. Relevant when Euler scheme is used for simulation.
  This input is only used for valuation using LSM.
  Default value: `None`.
* <b>`time_step_finite_difference`</b>: Scalar real `Tensor`. Spacing between time
  grid points in finite difference discretization. This input is only
  relevant for valuation using finite difference.
  Default value: `None`, in which case a `time_step` corresponding to 100
  discrete steps is used.
* <b>`num_grid_points_finite_difference`</b>: Scalar real `Tensor`. Number of spatial
  grid points for discretization. This input is only relevant for valuation
  using finite difference.
  Default value: 100.
* <b>`dtype`</b>: The default dtype to use when converting values to `Tensor`s.
  Default value: `None` which means that default dtypes inferred by
  TensorFlow are used.
* <b>`name`</b>: Python string. The name to give to the ops created by this function.
  Default value: `None` which maps to the default name
  `hw_bermudan_swaption_price`.


#### Returns:

A `Tensor` of real dtype and shape  `[batch_size]` containing the
computed swaption prices.



#### Raises:

(a) `ValueError` if exercise_times.rank is less than
floating_leg_start_times.rank - 1, which would mean exercise times are not
specified for all swaptions.
(b) `ValueError` if `time_step` is not specified for Monte-Carlo
simulations.