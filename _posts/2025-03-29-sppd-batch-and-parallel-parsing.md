---
---
layout: post
title: "SPPD: Batch and Parallel Parsing"
author: "Alvaro Carranza"
categories: blog
tags: [blog]
image: data-processing.png
---

In this blog post (before heading off for holidays!), I want to delve into the **batch and parallel parsing** techniques I recently implemented in `dl_parser`, the first SPPD module designed to download and parse Spanish public procurement data. These enhancements are pivotal for efficiently handling large volumes of ATOM files, especially when processing data spanning multiple years or months.
{: style="text-align: justify"}

### Why Batch and Parallel Parsing?

The initial version of `dl_parser` performed well for downloading and parsing earlier datasets (2012–2019). However, as the volume of public procurement data grew in recent years (each entry represents an update to a tender, meaning one tender can span multiple rows across multiple files), the previous implementation became increasingly **slow** (up to an hour for parsing) and **unstable** (frequent runtime errors due to memory leaks). To address these challenges, I explored **splitting the workload into smaller, manageable chunks** (batch processing) and **executing them simultaneously** (parallel processing).
{: style="text-align: justify"}

### Implementation Highlights

After evaluating several options—including **Dask** for distributed computing and **Polars** for its memory efficiency over Pandas—I settled on the following approaches:
{: style="text-align: justify"}

1. **Batch Processing**:  
    Regardless of the selected period (now supporting multiple periods simultaneously), processing is performed in **batches of 100 files**. Each batch is processed independently and saved as temporary files, reducing memory usage and improving modularity.
{: style="text-align: justify"}

2. **Parallel Parsing**:  
    Leveraging Python's `concurrent.futures.ProcessPoolExecutor`, I implemented a **process pool** to parse multiple batches concurrently. This approach utilizes **multiple CPU cores**, significantly reducing processing time compared to thread-based methods, particularly for CPU-intensive tasks like parsing large datasets.
{: style="text-align: justify"}

3. **Error Handling**:  
    **Robust error-handling mechanisms** ensure that failures in one batch or process do not disrupt the entire workflow. Failed batches are logged and can be retried independently.
{: style="text-align: justify"}

### Results

By combining these techniques, I achieved **substantial improvements** in parsing speed and stability, ensuring `dl_parser` is scalable for future demands. For instance, parsing the 2024 public procurement dataset (over 19 GB of ATOM files) was reduced from **an hour to just 7 minutes** (parsing phase only, tested on my laptop setup).
{: style="text-align: justify"}

### Challenges and Next Steps

While the current implementation is a significant step forward, there are still areas for improvement:
- **Optimizing I/O Operations**: File read/write operations remain a bottleneck in certain scenarios.
- **Code Refactoring**: Simplifying the logic to enhance maintainability and extensibility.
{: style="text-align: justify"}

With `dl_parser` now in a stable state, I plan to shift focus to `db_init`, the module responsible for **initializing the database**. This module will integrate data from `dl_parser` as the fact table and enrich it with dimensional tables derived from <a href="https://contrataciondelestado.es/codice/cl/" target="_blank">official codice dictionaries</a>, providing full context for the codes in the fact table.

If you’re curious about the technical details, feel free to explore the code in the <a href="https://github.com/Alvaro2c/sppd" target="_blank">repository</a> or reach out with any questions!
{: style="text-align: justify"}