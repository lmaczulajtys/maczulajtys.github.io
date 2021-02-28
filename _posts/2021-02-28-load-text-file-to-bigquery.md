---
title: Loading text file into BigQuery
date: 2021-02-28 20:00:00 +0100
categories: [Cloud, GCP]
tags: [cloud, gcp, bigquery]
description: The fastest and easiest way to load flat, unstructured text (txt, log) file into BigQuery table using only Google Cloud Storage and BigQuery itself.
---
How to load flat, unstructured text file into <a href="https://cloud.google.com/bigquery" target="_blank">BigQuery</a>? BigQuery supports loading files in Avro, CSV, JSON, ORC, or Parquet formats (<a href="https://cloud.google.com/bigquery/docs/loading-data" target="_blank">check documentation</a>). But when you need to load **unstructured txt or log file**, you need some simple hacks.

Beacause we can't specify data format, we need to apply the ELT (Extract Load Transform) approach.
1. Upload file to Google Cloud Storage
1. Load your data into BigQuery single column staging table
1. Parse data using BiqQuery functions and save the result in target table

Let's try to load <a href="https://github.com/elastic/examples/tree/master/Common%20Data%20Formats/apache_logs" target="_blank">example apache logs file</a>:
```
65.55.213.74 - - [17/May/2015:14:05:05 +0000] "GET /projects/solaudio HTTP/1.1" 301 332 "-" "msnbot/2.0b (+http://search.msn.com/msnbot.htm)"
81.169.149.220 - - [17/May/2015:14:05:13 +0000] "GET /favicon.ico HTTP/1.1" 200 3638 "-" "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:26.0) Gecko/20100101 Firefox/26.0"
65.55.213.74 - - [17/May/2015:14:05:25 +0000] "GET /projects/solaudio/ HTTP/1.1" 200 8301 "-" "msnbot/2.0b (+http://search.msn.com/msnbot.htm)"
65.55.213.79 - - [17/May/2015:14:05:21 +0000] "GET /about/ HTTP/1.1" 200 11474 "-" "msnbot/2.0b (+http://search.msn.com/msnbot.htm)"
65.55.213.79 - - [17/May/2015:14:05:05 +0000] "GET /articles/arp-security/ HTTP/1.1" 200 19735 "-" "msnbot/2.0b (+http://search.msn.com/msnbot.htm)"
```

## Preparing file
If your file size is lower than 10 MB, you can load it directly into BigQuery using your browser. Bigger files should be uploaded to Google Cloud Storage first.
1. <a href="https://cloud.google.com/storage/docs/creating-buckets" target="_blank">Create new bucket</a> or use existing one.
1. <a href="https://cloud.google.com/storage/docs/uploading-objects" target="_blank">Upload file.</a> I recommend using <a href="https://cloud.google.com/storage/docs/uploading-objects#gsutil" target="_blank">gsutil method</a> for large files.

## Loading file into BigQuery
1. <a href="https://cloud.google.com/bigquery/docs/datasets" target="_blank">Create dataset.</a>
1. Create table
   1. Create table from: Google Cloud Storage, choose path to your file.
   1. File Format: CSV.
   1. Table type: External table - you don't need to load temporary data to BigQuery.
   1. In schema, odd only one field "raw_data" with type STRING.
   1. Field delimiter: Custom - fill with character not present in the file. For example, for Apache Server logs, it could be `|`
   1. Click *Create table*.
1. Now you can query your data stored in Cloud Storage using BigQuery SQL.

## Parsing data
Now we are ready to parse our data and load it to target structure. <a href="https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions" target="_blank">BigQuery string functions</a> will be very helpful. Especially `REGEXP_EXTRACT`.

Use `LIMIT` testing your query to save cost and time working with large tables.

For our example file, we can write query extracting ip and date:
```sql
SELECT
    REGEXP_EXTRACT(data, r"\d+\.\d+\.\d+\.\d+") as ip,
    PARSE_DATE("%e/%b/%Y",REGEXP_EXTRACT(data, r"\[(\d+\/\w+\/\d+)")) as date
FROM our_temp_table LIMIT 100
```
with result:

| ip             | date       |
| -------------- |:----------:|
| 65.55.213.74   | 2015-05-17 |
| 81.169.149.220 | 2015-05-17 |
| 65.55.213.74   | 2015-05-17 |
| 65.55.213.79   | 2015-05-17 |
| 65.55.213.79   | 2015-05-17 |

1. When query is ready, run it on whole table.
1. Click *Save results*.
1. Choose *BigQuery table* and give table name - final destination for our structured data.

## Summary
In this short tutorial we loaded one file. If you need to parse real time data, you should consider using <a href="https://cloud.google.com/logging" target="_blank">Cloud Logging</a>. Check how I used it in article about [dealing with the pandemic with Google Cloud]({% post_url 2020-10-08-dealing-with-pandemic-google-cloud %}).
