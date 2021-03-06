---
id: explain-diffbot-api-timeout
title: How long can a single request take / what is the Diffbot API timeout?
sidebar_label: How long can a single request take / what is the Diffbot API timeout?
---

A Diffbot API request — call to a [Custom](api-basics-custom) or [Automatic](api-basics-index) API — can take, in theory, a maximum of 180 seconds (three minutes). If a request reaches this length it will be automatically timed-out and an error returned.

In practice: few if any requests will take anywhere close to this length of time, because Diffbot APIs have a default internal timeout  of only **30 seconds** for the retrieval of third-party content. That is, the download and rendering of the requested web page can take a maximum of 30 seconds before being truncated and an error returned.

The subsequent Diffbot processing of a rendered page is measured in milliseconds, so only in rare exceptions can a response take more than 30 seconds.

For multipage articles that are [automatically or manually concatenated](guides-multi-page-articles-discussions), the 30-second fetch timeout applies to each individual page. So, for example, a site whose pages take an average of 15 seconds to load will incur, for a ten-page article, a total Diffbot API request time of ~150 seconds for a ten-page article.

The 30-second fetch timeout can be adjusted using the `timeout` argument in your Diffbot API request. E.g., a request argument of `timeout=10000` will require a page to be fully retrieved in less than ten seconds; a request argument of `timeout=60000` will extend this to one minute.
