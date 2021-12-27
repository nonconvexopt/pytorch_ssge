# pytorch-SSGE
## Introduction
- Unofficial PyTorch Reimplementation of the paper "A Spectral Approach to Gradient Estimation for Implicit Distributions" (https://arxiv.org/abs/1806.02925), Shi et. al.
- Compatiable to use the kernel modules in [GPyTorch](https://gpytorch.ai/) and supports optimization with respect to kernel hyperparameters.

## Example

```python
from ssge import SSGE
import gpytorch

# Distribution to generate samples for testing
dist = torch.distributions.multivariate_normal.MultivariateNormal(
    torch.zeros(10),
    torch.eye(10),
)

# Use 'gpytorch.kernels.Kernel'
kernel = gpytorch.kernels.ScaleKernel(
    gpytorch.kernels.MaternKernel(
        ard_num_dims = 10
    )
)

# Initialize estimator class
estimator = SSGE(
    gpytorch.kernels.ScaleKernel(
        gpytorch.kernels.MaternKernel(
            ard_num_dims = 10
        )
    )
)

# Fit Context Samples.
sample = dist.sample([100])
sample.requires_grad_(True)
estimator.fit(sample)

# Estimate gradient of target samples.
test_sample = dist.sample([100])
test_sample.requires_grad_(True)
estimator.forward(test_sample)
```

## References
```
@misc{shi2018spectral,
      title={A Spectral Approach to Gradient Estimation for Implicit Distributions}, 
      author={Jiaxin Shi and Shengyang Sun and Jun Zhu},
      year={2018},
      eprint={1806.02925},
      archivePrefix={arXiv},
      primaryClass={stat.ML}
}
@misc{gardner2021gpytorch,
      title={GPyTorch: Blackbox Matrix-Matrix Gaussian Process Inference with GPU Acceleration}, 
      author={Jacob R. Gardner and Geoff Pleiss and David Bindel and Kilian Q. Weinberger and Andrew Gordon Wilson},
      year={2021},
      eprint={1809.11165},
      archivePrefix={arXiv},
      primaryClass={cs.LG}
}
```
