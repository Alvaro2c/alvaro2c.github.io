---
layout: post
title: "SPPD: Spanish Public Procurement Data - First step"
author: "Alvaro Carranza"
categories: blog
tags: [blog]
image: sppd-banner.jpg
---

Last year, before joining eDreams as an analyst, I started a personal project called **SPPD** (Spanish Public Procurement Data). I recognized the need for access to aggregated procurement data but found the <a href="https://estadisticas.contrataciondelsectorpublico.gob.es/" target="_blank">available options</a> limited—understandably so—particularly when it came to accessing data from specific companies, sectors, or contractors. So, I decided to build my own solution.
{: style="text-align: justify"}

In a [previous entry of this blog](/xml-scraping-101-elementtree-and-namespaces) (drier than I would have liked—though I did and learned a lot last year!) covers the first version of a script to parse XML/ATOM files, which is the format this data is currently available in, into parquet files.
{: style="text-align: justify"}

Long story short, I decided to start from scratch last week and work on a repository to build Python tools to: 1) Download and parse this data, 2) Build a database with it and 3) Create a Streamlit app to interact with the data.
{: style="text-align: justify"}

I recently finished the first step! Inside this <a href="https://github.com/Alvaro2c/sppd" target="_blank">repository</a>, there’s a module named dl_parser that allows users to download data from any available period (previous years, year-to-date, or specific months of the current year), and parse it into a Parquet file.
{: style="text-align: justify"}

This project has helped me revisit some of the **fundamentals of programming**: testing, project organization, CI/CD (with GitHub Actions), and code quality, among others. The current codebase has plenty of room for improvement (mainly optimizing parsing speed), but I’m excited to keep building on it.
{: style="text-align: justify"}

The project’s README has more information about its current state. Of course, I’m happy to answer any questions anyone might have about it!
{: style="text-align: justify"}

Hopefully, the next blog entry will be about the **database**.
{: style="text-align: justify"}
