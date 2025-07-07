---
layout: post
title: "SPPD: Open tenders MVP website is live"
author: "Alvaro Carranza"
categories: blog
tags: [blog]
image: sppd-ot.jpg
---

After some time away, I’m happy to be back working on the **SPPD project** — and to share the first major milestone: the release of **SPPD Licitaciones Abiertas**, our **MVP platform** for **open procurement data** in Spain.

This site is a small but meaningful step toward one of our core goals: making **Spanish public procurement** more **transparent** and **accessible**. Built around **open data** from <a href="https://contrataciondelestado.es/" target="_blank"><b>Ministerio de Hacienda</b></a> in Spain, the portal gives anyone the ability to **explore active tenders** currently open for submission.

The interface is intentionally **simple**. Visitors can browse a **table of public tenders** updated during the current month or the two previous ones, and **download the dataset** in **CSV** or **JSON** formats. There are also some **basic charts** in place, with more **data visualizations** on the way.

The project also marks a personal milestone — it’s my **first fully coded website**, built with plain **HTML**, **CSS**, and **JavaScript**. On the backend, I’ve set up a lightweight **ETL pipeline** using **Python** and **Polars**, running on **GitHub Actions** for now. The plan is to migrate to **Apache Airflow** soon to support **daily data updates** and more robust scheduling.

The **MVP is live**, and while it’s still a work in progress, the **core functionality** is solid. If you're curious about **public procurement**, **data infrastructure**, or **open government**, take a look and let me know what you think.

Explore the site here: <a href="https://alvaro2c.github.io/sppd-ot/" target="_blank"><b>SPPD Licitaciones Abiertas</b></a><br><br>

Backend code: <a href="https://github.com/Alvaro2c/sppd" target="_blank"><b>SPPD Backend</b></a><br>
Frontend code: <a href="https://github.com/Alvaro2c/sppd-ot" target="_blank"><b>SPPD Frontend</b></a>
