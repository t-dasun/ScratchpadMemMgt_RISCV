# What is a Heterogeneous Management Scheme?

Based on the provided text, a **heterogeneous management scheme** is a tailored, layer-by-layer plan for managing the on-chip memory of a Deep Learning (DL) accelerator.

Let's break that down:

* **Heterogeneous:** This means "varied" or "composed of different kinds." In this context, it signifies that the way memory is managed is **not the same** for every layer of the neural network.
* **Management Scheme:** This refers to the set of rules or the strategy ("policy") used to allocate the unified memory buffer.

---

### In Simple Terms: One Size Doesn't Fit All

Imagine you have a toolbox (the unified memory) and a series of different jobs to do (the layers of a neural network).

* A **homogeneous** (or rigid) scheme would be like using only a hammer for every single job, whether you're driving a nail, cutting wood, or turning a screw. It's inefficient and often doesn't work well.
* A **heterogeneous** scheme is like looking at each job individually and picking the best tool from the toolbox. For the nail, you use the hammer. For the wood, you use a saw. For the screw, you use a screwdriver.

---

### How it Applies to the Paper

In the context of the paper, the "Smart Memory Manager" creates this heterogeneous scheme by analyzing each layer of a model (like ResNet18) and asking:

* "For **this specific layer**, what is the biggest memory bottleneck?"
* "Is it the large number of filters? Or the massive size of the input feature map?"
* "Is our main goal to save energy (minimize memory access) or to be as fast as possible (minimize latency)?"

Based on the answers, it applies a different **"policy"** or strategy to each layer.

#### Example Scenario: ResNet18

Within the same ResNet18 model, the heterogeneous scheme might decide to:

* **For Layer 1:** Allocate 70% of the memory to storing and reusing the large **input feature map**, as this is the biggest data component.
* **For Layer 10:** Shift the strategy and allocate 60% of the memory to storing and reusing **filters (weights)**, because this layer has many more filters than the earlier ones.
* **For Layer 15:** Allocate 50% of the memory to **prefetching** the data for Layer 16, because the analysis shows this is the best way to hide latency for this particular step.

This is the core of the "heterogeneous management scheme": it's a dynamic, intelligent, and non-uniform approach that customizes memory usage for each part of the problem, leading to much greater efficiency than a fixed, "one-size-fits-all" memory partition.