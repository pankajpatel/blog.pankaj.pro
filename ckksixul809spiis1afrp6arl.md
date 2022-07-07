## TIL: cache-control is very inconsistent in different Browsers

Today a colleague started asking questions on cache-control and how to achieve consistent behaviour so that he can control it from Server Side

Then another colleague pointed out a StackOverflow thread where the answer makes sense but raises a question on _**why do these differences exist?**_

%[https://stackoverflow.com/a/14842553/759045]

I think I still don't understand it completely after this revelation.

I suggested that we can use Service Workers to intercept all requests and implement better caching strategy there.

In response, another colleague pointed out that SWs are not worth the work and the support is inconsistent.

which lead me to here: https://caniuse.com/serviceworkers

[![caniuse.com_serviceworkers](https://cdn.hashnode.com/res/hashnode/image/upload/v1612543663973/Q1KdnK-3l.png)](https://caniuse.com/serviceworkers)

> Which again raises this question what is best approach to control caches from server?