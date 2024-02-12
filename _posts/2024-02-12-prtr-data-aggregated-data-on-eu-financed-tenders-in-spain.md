---
layout: post
title: "PRTR Data: Dashboard with aggregated data on EU-financed tenders in Spain"
author: "Alvaro Carranza"
categories: blog
tags: [blog]
image: prtr-banner.png
---

Lately I have been working in two projects in parallel: SPPD (Spanish Public Procurement Database) and PRTR Data, a dashboard with aggregated data on EU-financed (Recovery and Resilience Facility) tenders in Spain. In both cases, I found the need to have access to aggregated data but found the available options lackluster, thus deciding to work on my own solution.
{: style="text-align: justify"}

After a lot of work, I finally have a first version of the <a href="https://prtr-data.streamlit.app/" target="_blank">dashboard</a> (in Spanish, soon in English too!).
{: style="text-align: justify"}

The dashboard aggregates data from the <a href="https://planderecuperacion.gob.es/" target="_blank">official website</a> of the Spanish government and the latest data update was on February 2nd 2024. It is made on Looker Studio and connects to BigQuery to get all the data, extracted and transformed with Python scripts. The data pipeline for future updates will be orchestrated with Airflow.
{: style="text-align: justify"}

If you have any suggestions or comments about it don't hesitate to reach out to me!
{: style="text-align: justify"}
