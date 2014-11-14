---
layout: bootstrap
title: Knowledge Learning Instruction
date: May 4, 2014
---
<div class="blog-post">
            <h2 class="blog-post-title">{{ page.title }}</h2>
            <p class="blog-post-meta">{{ page.date}}</p>

https://class.coursera.org/bigdata-edu-001/lecture/5 PSLC Datashop

Both incorrect actions (errors of commission) and hint requests (errors of omission—the student did not know how to perform the step on his or her own) are considered errors.(https://pslcdatashop.web.cmu.edu/help?datasetId=408&page=terms#error_rate)

Video about KLI(Knowledge Components, Learning Activity, Instruction), Algebra as a foreign language: http://pact.cs.cmu.edu/koedinger/Koedinger10_WMV%20V9.wmv --> Challenge vs Assist dichotomy 

### Spark
    RDD ( resilient distributed datasets)  An RDD is represented as a Scala object and can be created from a file; as a parallelized slice (spread across nodes); as a transformation of another RDD; and finally through changing the persistence of an existing RDD, such as requesting that it be cached in memory.
    Runs on top of Mesos.
    Application = Driver = Actions or Transformation 
    