# Customizing at the edge with functions<a name="edge-functions"></a>

* *edge function*
  * := code / you write & attach | your CloudFront distribution
    * allows
      * customizing how your CloudFront distributions
        * process
          * HTTP requests
          * HTTP responses
        * perform basic authentication & authorization
        * generate HTTP responses | edge 
    * runs -- close to your -- users
      * Reason: 🧠 minimize latency 🧠
    * managed by AWS
      * == NO needed by you, to manage
        * servers
        * OTHER infrastructure 
  * ways to write and manage edge functions
    * **CloudFront Functions**
      * allows
        * writing lightweight functions | JavaScript -- for --
          * high-scale
          * latency-sensitive CDN customizations 
      * 's runtime environment
        * offers submillisecond startup times
        * scales immediately -- to handle -- millions of requests / second
        * is HIGHLY secure 
      * == native feature of CloudFront
        * == | CloudFront ONLY, you can
          * build
          * test
          * deploy your code
      * see [Customizing at the edge with CloudFront Functions](cloudfront-functions.md)
    * **Lambda@Edge**
      * == extension of [AWS Lambda](https://aws.amazon.com/lambda/) /
        * powerful & flexible computing | complex functions
        * full application logic / -- closer to your -- users
        * highly secure
      * Lambda@Edge functions run | runtime environment
        * Node.js or
        * Python 
      * how to use it?
        * publish them | 1! AWS Region
        * when you associate the function -- with a -- CloudFront distribution -> Lambda@Edge -- automatically replicates -- your code | around the world
      * see [Customizing at the edge with Lambda@Edge](lambda-at-the-edge.md)

## Choosing between CloudFront Functions and Lambda@Edge<a name="edge-functions-choosing"></a>

* CloudFront Functions vs CloudFront Lambda@Edge
  * BOTH, based on CloudFront events -> can run code


|  | CloudFront Functions | Lambda@Edge | 
| --- | --- | --- | 
| Programming languages | JavaScript \(ECMAScript 5\.1 compliant\) | Node\.js and Python | 
| Event sources |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/edge-functions.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/edge-functions.html)  | 
| Scale | 10,000,000 requests per second or more | Up to 10,000 requests per second per Region | 
| Function duration | Submillisecond |  Up to 5 seconds \(viewer request and viewer response\) Up to 30 seconds \(origin request and origin response\)  | 
| Maximum memory | 2 MB | 128 – 3,008 MB | 
| Maximum size of the function code and included libraries | 10 KB |  1 MB \(viewer request and viewer response\) 50 MB \(origin request and origin response\)  | 
| Network access | No | Yes | 
| File system access | No | Yes | 
| Access to the request body | No | Yes | 
| Access to geolocation and device data | Yes |  No \(viewer request\) Yes \(origin request, origin response, and viewer response\)  | 
| Can build and test entirely within CloudFront | Yes | No | 
| Function logging and metrics | Yes | Yes | 
| Pricing | Free tier available; charged per request | No free tier; charged per request and function duration | 

* **CloudFront Functions**'s use cases
  * lightweight, short\-running functions
    + **Cache key normalization**
      + _Example:_ transform HTTP request attributes (headers, query strings, cookies, URL path\) -- to create an -- optimal [cache key](understanding-the-cache-key.md)
    + **Header manipulation**
      + _Example:_ insert, modify, or delete HTTP headers | request or response 
        + _Example specific:_ add a `True-Client-IP` header / EVERY request
    + **URL redirects or rewrites**
      + _Example1:_ redirect viewers -- to -- other pages / -- based on -- information | request
      + _Example2:_ rewrite ALL requests -- from -- one path -- to -- another
    + **Request authorization**
      + _Example:_ validate hashed authorization tokens (as JWT) -- via -- inspecting
        + authorization headers
        + OTHER request metadata


* **Lambda@Edge**'s use cases
  + functions /
    + take several milliseconds to complete
    + require
      + adjustable CPU or memory
      + network access -- to use -- external services for processing
      + file system access or access -- to the -- body of HTTP requests
    + -- depend on -- TP libraries ( _Example:_ AWS SDK -- for integration with -- OTHER AWS services\)
