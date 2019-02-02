# Performance

It's obvious that PHP framework performance mainly depends on the lines of code that have to be executed for each request. 
While Symlex was designed to be simple and lean, a good performance is a very important by-product of this approach.

!!! quote
    The best code is no code. Where there is no code, there are no bugs. No API to learn. 
    No awkward UI. The best refactors are deletions.<br> ― *Eric Elliott*

As published by [phpbenchmarks.com](http://www.phpbenchmarks.com/en/benchmark/apache-bench/php-7.3/symlex-4.1.html), 
Symlex currently adds 42% less overhead to REST requests than the next best PHP framework:

<img src="https://symlex.org/images/performance-large.svg" alt="Response times of popular PHP frameworks" width="100%">

Note that these response times were measured in fully optimized production mode on [fast server hardware](http://www.phpbenchmarks.com/en/benchmark-protocol.html) with only 5 concurrent requests. In practice,
differences might be much larger in terms of absolute time. Memory consumption should be considered as well:

<img src="https://symlex.org/images/memory-large.svg" alt="Memory footprint of popular PHP frameworks" width="100%">

## Why should you care? ##

First, your users will love it. As a rule of thumb, **100 ms** is about the limit for having them feel that the system 
is reacting instantaneously, meaning that no special feedback is necessary except to display the result. 
The total response time also includes network (~25 ms), browser and other overhead, which only leaves a small fraction 
of those 100 ms for implementing the actual business logic.

Second, you'll save a lot of money for server infrastructure and developers are more productive as tests are running faster.