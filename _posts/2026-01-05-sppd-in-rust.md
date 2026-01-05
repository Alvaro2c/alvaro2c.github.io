---
layout: post
title: "SPPD: A Rust CLI for Data Ingestion"
author: "Alvaro Carranza"
categories: blog
tags: [blog]
image: rust-cli.jpg
---

Following my previous work on Spanish Public Procurement Data (SPPD), where I moved from initial exploration to building an MVP website and optimising parsing, I decided to take the next step. I wanted to learn Rust while building something practical, so I rewrote the data ingestion pipeline as a standalone tool.
{: style="text-align: justify"}

The result is sppd-cli, a command-line tool designed to download, extract, and convert raw procurement data into Parquet format.
{: style="text-align: justify"}

**The Evolution**
{: style="text-align: justify"}
My earlier posts covered various stages of this project in its previous Python version, including batch and parallel parsing. This new project focuses on creating a robust, 100% Rust-based pipeline. It streamlines the process that was previously handled through various Python scripts into a single efficient workflow.
{: style="text-align: justify"}

**How it Works**
{: style="text-align: justify"}
The tool follows a clear four-stage architecture
1. Downloader: Fetches ZIP archives from the source
2. Extractor: Unzips the archives
3. Parser: Processes the XML data into a DataFrame
4. Parquet: Saves the final structured data for analysis
{: style="text-align: justify"}

**Usage**
{: style="text-align: justify"}
You can run it manually via the CLI or use a TOML configuration file for automated runs. I have built this tool with the intention of eventually using it as a main component of a larger ETL pipeline. It supports both public tenders and minor contracts, allowing you to specify date ranges by year or month.
{: style="text-align: justify"}

For example, to process data via a config file:

```bash
cargo run -- toml config/prod.toml
```

The tool also handles batch sizes, concurrent downloads, and retries automatically, ensuring the data collection is as resilient as possible.
{: style="text-align: justify"}

**Resources**
{: style="text-align: justify"}
You can find the project and the README, which is also available in Spanish, on <a href="https://github.com/Alvaro2c/sppd-cli" target="_blank"><b>GitHub</b></a>, and code documentation on  <a href="https://alvaro2c.github.io/sppd-cli/sppd_cli/" target="_blank"><b>this Github Pages</b></a>.
{: style="text-align: justify"}
