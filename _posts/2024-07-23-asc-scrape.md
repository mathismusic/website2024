---
layout: post
title: A scraper for internal ASC
mathjax: true
---

This is a short write-up about a scraper for IIT Bombay's internal ASC website; it might be useful during the registration period, all available in one place. Funnily enough, this scraper was run at registration time and ASC did not crash even once.

Scraping is done using `selenium` and the web-driver for Google Chrome. The idea is to first login, and examine manually the HTML tags for the labels of the relevant table entries that need to be clicked. For example, _running courses_ or _grading statistics_. The `selenium.WebDriver.find_element` method, and the option `By.XPATH` is golden for this purpose. The rest is simply trial-and-error routine python code.

The scraper is available [here](https://mathismusic.github.io/asc_scrape/clean_scraper.ipynb) as an `iPython` notebook. The resulting run for the Autumn semester, 2023-24 is available [here](https://mathismusic.github.io/asc_scrape/allcourses-2023-2024-1 - Autumn.csv) (it contains a list and description of all courses that ran, with grading statistics). For the results split by department, use [this](https://mathismusic.github.io/asc_scrape/allcourses-deptwise-2023-2024-1 - Autumn.tgz) tarball.

The scraper is a good starting point to get your hands dirty with some web scraping, and also can be extended with very similar additions to scrape other parts of ASC, if needed. The results are easily subjected to some analysis, to find out courses with high average grades, or courses with high failure rates, etc. The possibilities are endless.

Use it wisely, and happy scraping!