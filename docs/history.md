# Relationship with Symfony and Silex

Symlex was started in 2014 as a simple Silex boilerplate, since Silex itself doesn't come with a "Standard Edition" 
that points you in the right direction. Using Silex instead of Symfony was recommend by SensioLabs (the creators 
of both frameworks) as a light-weight alternative to Symfony + FOSRestBundle for quickly building high-performance 
REST services and single-page Web applications.

It was soon noticed that Pimple - the service container that comes with Silex - feels cumbersome for developers 
coming from Symfony and makes it hard to reuse existing code. In addition, many Silex code examples and even real-world 
applications accessed the service container from all parts of the code (not only the framework itself), 
which circumvents inversion of control and leads to awkward testability. Symlex therefore promotes the strict use of dependency 
injection and combines the convenience of a full-fledged service container with the speed of a micro-framework.

Today, Symlex has its own routing component (based on Symfony 4) and does not use Silex anymore. 
The framework has proven to be useful for a large number of different applications. Some of them were based on the regular
Symfony kernel before and did the change because they were drowning in complexity and suffered from response times well 
above 30 seconds in development mode. Symlex brought them back on track without big changes to their existing code base.

<img src="https://symlex.org/images/symlex-vs-symfony.svg" alt="Micro-Kernel Architecture" width="100%">
