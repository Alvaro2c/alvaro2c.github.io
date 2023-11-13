---
layout: post
title: "XML Scraping 101: ElementTree and namespaces"
author: "Alvaro Carranza"
categories: blog
tags: [blog]
image: scraping.jpg
---

I am currently working on building a database with <a href="https://contrataciondelestado.es/wps/portal/!ut/p/b1/rZDNboMwEISfpQ8QeTFg7KP5N-K3gAtcEImiKmmACkVEvH1JhHpLeuneRvpmZ2dRg-od1QkzKFBAFWqGbj59dtfTOHSXu25Ia-aUclPhAGpJgZMss5gNGJi6AvUKwJPh8PBrTmJZro-B5urqC-2yJP5d4s3_Avgj_wM1D0RXLU0GMiW58ACE79phqejgYbIBL06sV8BouXQyLpgKifm-Lg_SqEg9rAAQJIdx6tdvFKgCrc3PMEXh3k4K37kuzEvDuMTL1_di30awzuIWzmkQDyKIqyDI90dFOTBFupnDo8NEZ_62Bf62SqhjgtAlsWMjwhi0fw-M_bE_or65uEzs6rkPtR-_0jIw/dl4/d5/L2dJQSEvUUt3QS80SmtFL1o2X0JTODhBQjFBMDAzVTgwQTZRUUM5RDAyMDkz/?nID=Z6_BS88AB1A003U80A6QQC9D02093&cID=Z6_BS88AB1A003U80A6QQC9D02093" target="_blank">publicly available data</a> on public procurement in Spain. I decided to work on this to make it easier to get insights on aggregated data, have access to data visualizations and other features on an easy-on-the-eyes dashboard, as the current data access is quite clunky.
{: style="text-align: justify"}

To do so, I have started my Airflow journey with <a href="https://www.udemy.com/course/the-ultimate-hands-on-course-to-master-apache-airflow/" target="_blank">Marc Lamberti's The Hands-On Guide</a> and continued to hone my SQL skills with <a href="https://pgexercises.com/" target="_blank">PostgreSQL Exercises</a>. However, it all starts with actually getting the data.
{: style="text-align: justify"}

**Scraping is a great way to collect data when you don't have access to structured sources (such as CSV files) or APIs.** In this case, after trying to do some web scraping directly from the search tab of the website, I found the information on XML files (actually in ATOM, XML language used for web feeds): today's blog entry will tackle XML scraping.
{: style="text-align: justify"}

As can be seen after a <a href="https://www.google.com/search?q=namespaces+beautifulsoup+site:stackoverflow.com&client=firefox-b-d&sca_esv=581256897&sxsrf=AM9HkKmitJ8jToWMOB8Rx9kHOqUiDqKGOw:1699635049860&sa=X&ved=2ahUKEwii4JP78bmCAxVCdqQEHR4IAaAQrQIoBHoECCEQBQ&biw=1366&bih=625&dpr=1#ip=1" target="_blank">quick google search</a> on "namespaces beautifulsoup" in Stack Overflow, it seems that many users of BeautifulSoup, Python package for parsing HTML and thus useful for web scraping, struggle with namespaces while using it to parse an XML file. **XML namespaces are used for providing uniquely named elements and attributes in an XML document.** An XML instance may contain element or attribute names from more than one XML vocabulary. If each vocabulary is given a namespace, the ambiguity between identically named elements or attributes can be resolved. They can be found at the beginning of the file.
{: style="text-align: justify"}

![image](https://i.ibb.co/dtgCw7y/namespaces.jpg)

*Namespaces found at the beginning of the XML file*
{: style="text-align: center"}

While I am sure there is a way to do it with BS, after some research I decided to use <a href="https://docs.python.org/3/library/xml.etree.elementtree.html" target="_blank">ElementTree (ET)</a>, a library that allows you to parse and navigate a XML document by breaking it down in a tree structure that is easy to work with. Namespaces can be taken care of by registering them and creating a dictionary to
call the appropriate namespace key in each .find() method so as to get the proper namespace value.
{: style="text-align: justify"}

In the following code snippet I show this and an example of for loop to get the general information I found on the XML files for the previously mentioned project.
{: style="text-align: justify"}

```python
import os
import pandas as pd
import xml.etree.ElementTree as ET

# Replace with your file name or path
file_name = 'test.atom'

# Get the absolute path of the file
file_path = os.path.abspath(file_name)

# Parse the tree and get the root
tree = ET.parse(file_path)
root = tree.getroot()

# Register all namespaces found at the beginning of the XML file

ET.register_namespace('cbc-place-ext', 'urn:dgpe:names:draft:codice-place-ext:schema:xsd:CommonBasicComponents-2')
ET.register_namespace('cbc', 'urn:dgpe:names:draft:codice:schema:xsd:CommonBasicComponents-2')
ET.register_namespace('cac', 'urn:dgpe:names:draft:codice:schema:xsd:CommonAggregateComponents-2')
ET.register_namespace('ca-place-ext', 'urn:dgpe:names:draft:codice-place-ext:schema:xsd:CommonAggregateComponents-2')
ET.register_namespace('at', 'http://purl.org/atompub/tombstones/1.0')

# Create dictionary of namespaces

ns = {'cbc-place-ext': 'urn:dgpe:names:draft:codice-place-ext:schema:xsd:CommonBasicComponents-2',
      'cbc': 'urn:dgpe:names:draft:codice:schema:xsd:CommonBasicComponents-2',
      'cac': 'urn:dgpe:names:draft:codice:schema:xsd:CommonAggregateComponents-2',
      'cac-place-ext': 'urn:dgpe:names:draft:codice-place-ext:schema:xsd:CommonAggregateComponents-2',
      'at': 'http://purl.org/atompub/tombstones/1.0'
     }

# Creating a Pandas dataframe with general info from the entries

ids = []
titles = []
summaries = []
links = []
statuses = []


for entry in root.findall('{http://www.w3.org/2005/Atom}entry'):
    # General info
    title = entry.find('{http://www.w3.org/2005/Atom}title').text
    summary = entry.find('{http://www.w3.org/2005/Atom}summary').text
    link = entry.find("{http://www.w3.org/2005/Atom}link").get("href")
    details = entry.find('cac-place-ext:ContractFolderStatus', ns)
    id = details.find('cbc:ContractFolderID', ns).text
    status = details.find('cbc-place-ext:ContractFolderStatusCode', ns).text

    ids.append(id)
    titles.append(title)
    summaries.append(summary)
    links.append(link)
    statuses.append(status)


general_data = {'id': ids,
                'title': titles,
                'summary': summaries,
                'link': links,
                'status': statuses
                }

general_df = pd.DataFrame(general_data)

general_df
```

This dataframe (only using Pandas for intermediary work of course!) looks something like this (some Spanish text incoming):
{: style="text-align: justify"}

![image](https://i.ibb.co/7JqgSPj/df-example.jpg)

I am currently finishing the last details to have all the information I am interested in and being able to scale it to the data of +10 years (33.5 gb). The actual code will most likely have tons of try clauses because trees evolve and don't have the same branches/structure from one file to the other (and even from one entry to the other): **data is indeed a beautiful soup**.
{: style="text-align: justify"}
