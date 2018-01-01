# EmpiricalRiskMinimization

[![Build Status](https://travis-ci.org/reesepathak/EmpiricalRiskMinimization.jl.svg?branch=master)](https://travis-ci.org/reesepathak/EmpiricalRiskMinimization.jl)

[![Coverage Status](https://coveralls.io/repos/reesepathak/EmpiricalRiskMinimization.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/reesepathak/EmpiricalRiskMinimization.jl?branch=master)

[![codecov.io](http://codecov.io/github/reesepathak/EmpiricalRiskMinimization.jl/coverage.svg?branch=master)](http://codecov.io/github/reesepathak/EmpiricalRiskMinimization.jl?branch=master)


## Installation
Currently this package is only available via GitHub (though it soon will be submitted to
  the Pkg repository). Users add the package via:
```Julia
Pkg.clone("https://github.com/reesepathak/EmpiricalRiskMinimization.jl.git")
```


## Example usage
```Julia
using EmpiricalRiskMinimization

# data
n = 2000; d = 350;
X = randn(n, d); y = (sign.(randn(n) - 0.5) + 1)/2;

# model
model = Model(LogisticLoss(), L2Reg(0.1), fit_intercept=true);
fit!(model, X, y)
status(model)
final_risk(model)
weights = weight(model)

# predictions
sigm(u) = 1/(1 + exp(-u))
y_tild = sigm.(X*weights[1:d] + weights[d + 1])
y_pred = 1.0 * (y_tild .>= 0.5)
accuracy = sum(1.0*(y_pred .== y))/n
println("Accuracy: $accuracy")
```