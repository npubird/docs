---
id: guides-job-limits
title: Controlling the number of Active Crawl jobs
sidebar_label: Controlling the number of Active Crawl jobs
---

# Managing Crawl Job Numbers

 Our system supports an upper limit of 1,000 jobs (Crawl and/or Bulk) per account/token. If you exceed this limit and try to create a new job, you will receive a 505 "Too many collections" error, as follows:

```
{
    "errorCode":505,
    "error":"Too many collections for token"
}
```

 It is important to keep this 1000-job limit in mind when managing your jobs. Below are some recommendations to consider, when thinking about ways to limit the number of total jobs on your token.

&nbsp; 

## Combining/Consolidating Jobs with Default Settings

Domains/Seed URLs which are being crawled with only the default job settings can be combined into a single job, or a few jobs.  Since these jobs have all of the same (default) settings, all of the seed URLs for these jobs can be consolidated into a much smaller number of jobs. The results for different domains can then be filtered out after job completion by using the Search API, as described at [https://docs.diffbot.com/docs/en/api-search](https://docs.diffbot.com/docs/en/api-search) .

 A "best practice" recommendation here is to consolidate as many URLs as possible into one or a few jobs with default settings, unless/until it is determined that one of those URLs requires its own customization/job settings and should be handled differently. If it is determined that a particular domain requires a particular setting which might not be suitable for all of the Seeds, that domain can be migrated to its own job with its own settings.

### Concurrency Across Seeds

Crawlbot supports a maximum of 30 Seeds (and their associated chain of links) running at the same time.  If there are more than 30 Seeds on a job, some of these will be queued up.  If you have more than 30 seeds you need to Crawl, and it is absolutely necessary that the results for these Seeds are returned as quickly as possible, you may consider spreading these Seeds across a larger number of jobs, 30 per job.

&nbsp;  

## Cleaning up Finished Jobs
Once a job has finished and has no Repeat rounds remaining, the job metadata and results can be downloaded and the job deleted. This can be done programmatically, as follows:
  1. Download the entire list of crawls on your token, as well as their associated metadata, by making a GET request to [https://api.diffbot.com/v3/crawl?token](https://api.diffbot.com/v3/crawl?token)={{TOKEN}}. The API returns a JSON array of jobs.
  2. Iterate over each job in the array, checking the job.jobStatus.status field for the integer value 9 ("Job has completed and no repeat is scheduled").
  3. When Status 9 is found for a job, perform a GET request against the URLs in the **downloadJson** and **downloadUrls** fields to download the JSON results and URL Report, respectively.
  4. After successfully downloading the results/URL Report for a job, you can delete the job with a POST request, as described at [https://docs.diffbot.com/docs/en/api-crawlbot-api](https://docs.diffbot.com/docs/en/api-crawlbot-api) under the heading, "Pausing, Restarting or Deleting Crawls".