---
title: How to unit test Python Spark jobs
layout: post
---

# How to unit test Python Spark jobs

I imagine you wrote a Spark job in Python where you crunched some data using Spark functions. These transformations can not be tested with an ordinary unit testing library alone. Nevertheless it is worth to test them as well. It's easy and I show you how.

## Create a local Spark context

You need to **run the Spark job** in order to test transformations done by applying Spark functions. Like you would normally run a Spark job, you need to provide a Spark context. The recommended way for unit testing Spark jobs is to create a **local Spark context** for each test. There is no need to call spark-submit though. Instead you can invoke Spark from Python directly. The Python interpreter needs to locate the PySpark package in order for that to work. A package called **FindSpark** can help with that. You can install it by simply typing

```
pip install findspark
```
You need to call `findspark.init()` before setting up the context (for example on the module level). Findspark will use `SPARK_HOME` env variable to locate Spark Python packages (make sure it's set!).

Putting everything together in a minimal example:

<script src="https://gist.github.com/domenp/7e8b2572215e28eb75dd.js"></script>

In the example above I have setup local Spark context for each test in unit testing library's setUp function and stop the context in tearDown function. I have put all data transformations in `non_trivial_transform` function that accepts RDD. In this case unit testing is as simple as mocking up RDD and passing it to the function.

## Conclusion

That's about all. In case you prefer a battery included solution checkout [Spark Testing Base](https://github.com/holdenk/spark-testing-base) package. It does approximately the same as we did above but it also provides an option to reuse the Spark context between the jobs and supports testing Spark streaming jobs.

**Happy unit testing!**

