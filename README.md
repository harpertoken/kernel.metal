Metal SAXPY Compute Kernel
===========================

Performing large-scale vector operations on the CPU can be slow and inefficient for data-intensive tasks. Utilizing the GPU via Metal compute shaders on Apple Silicon enables parallel processing, significantly accelerating computations like SAXPY (Single-precision A*X + Y).

Recommendation
==============

Use Metal compute kernels for parallelizable operations to leverage GPU performance. Ensure proper thread mapping and buffer management for optimal results.

Example
=======

This project performs SAXPY on vectors of 1,000,000 floats (X[i] = i, Y[i] = 2*i, a = 2.0) to produce Y[i] = 2.0*i + 2*i = 4*i.

Kernel Code (kernel.metal)
--------------------------
```metal
#include <metal_stdlib>
using namespace metal;

kernel void saxpy(
    constant float &a [[buffer(0)]],
    device const float* X [[buffer(1)]],
    device float* Y [[buffer(2)]],
    constant uint &count [[buffer(3)]],
    uint id [[thread_position_in_grid]]
) {
    if (id >= count) return;
    Y[id] = a * X[id] + Y[id];
}
```

Host Code (main.swift)
----------------------
```swift
// ... (full code as provided)
```

Sample Output
-------------
(Shows first 5 and last 5 elements for brevity; full array is 1,000,000 elements)
```
Elapsed (s): 0.001
Sample results:
Y[0] = 0.0
Y[1] = 4.0
Y[2] = 8.0
Y[3] = 12.0
Y[4] = 16.0
...
Y[999995] = 3999980.0
Y[999996] = 3999984.0
Y[999997] = 3999988.0
Y[999998] = 3999992.0
Y[999999] = 3999996.0
```

Local Build and Run
===================

To build and run locally on Apple Silicon (M1/M2):

1. Install dependencies: `./install.sh`
2. Compile: `swiftc main.swift -framework Metal -o vector_add`
3. Run: `./vector_add`

This performs vector addition on 1,000,000 floats using GPU compute.

CI/CD
=====

- **GitHub Actions**: Runs linting on ubuntu-latest and syncs to GitLab.
- **GitLab CI**: Runs full build and test on macOS for Apple Silicon compatibility.
- **CircleCI**: Runs validation on Ubuntu (file checks, actionlint).
- **Local CI**: Use `act` for GitHub Actions simulation, `gitlab-ci-local` for GitLab CI, or CircleCI local CLI.

> **⚠️ Important:** GitHub's hosted macOS runners are Intel-based and do not support Metal compute shaders. Full build and test must be performed locally on Apple Silicon hardware or using dedicated macOS runners (e.g., via GitLab CI). Only linting and syncing run on GitHub's hosted runners.

References
==========

- [Apple Metal Documentation](https://developer.apple.com/metal/)
- [Metal Shading Language Guide](https://developer.apple.com/metal/Metal-Shading-Language-Specification.pdf)
- [GPU Compute Best Practices](https://developer.apple.com/documentation/metal/performing_calculations_on_a_gpu)


