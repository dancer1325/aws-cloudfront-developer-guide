# What is Amazon CloudFront?<a name="Introduction"></a>

* := web service / 👀speeds up distribution -- through edge locations -- of your web content 👀
  * type of content
    * static 
      * _Example:_ image files
    * dynamic
      * _Example:_ \.html, \.css, \.js
  * if the content is NOT | edge location -> CloudFront -- retrieves it from an -- origin / you've defined (_Example:_ Amazon S3 bucket, a MediaPackage channel, or HTTP server)
    * _Example:_ a web server / -- identified as the -- source for the definitive version of your content
  * vs serve content | traditional web server directly (== NOT use CloudFront)
    * much slower
      * Reason: 🧠 request is routed -- from -- one network -- to -> another—through the complex collection of interconnected networks / == internet 🧠
* edge location
  * use cases
    * content / -- is served via -- CloudFront
  * allows
    * routing requests to the lowest latency \( == time delay\)
      * -> content -- is delivered with the -- best possible performance
      * == use AWS network -> reduce the # of networks / your users' requests -- must -- pass through 
      * -- from -- FIRST request
    * copying your files | multiple edge locations
      * -> better
        * reliability &
        * availability 

**Topics**
+ [How you set up CloudFront to deliver content](#HowCloudFrontWorksOverview)
+ [CloudFront use cases](IntroductionUseCases.md)
+ [How CloudFront delivers content](HowCloudFrontWorks.md)
+ [Locations and IP address ranges of CloudFront edge servers](LocationsOfEdgeServers.md)
+ [Accessing CloudFront](introduction-accessing-cloudfront.md)
+ [CloudFront pricing](CloudFrontPricing.md)

## How you set up CloudFront to deliver content<a name="HowCloudFrontWorksOverview"></a>

* create a CloudFront distribution
  * == tell CloudFront 
    * where content is delivered from
    * how to
      * track & manage content delivery

![\[How CloudFront works\]](http://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/images/how-you-configure-cf.png)<a name="HowCloudFrontWorksConfiguration"></a>

**How to configure CloudFront -- to deliver -- your content**
1. specify *origin servers* 
   1. _Example:_ Amazon S3 bucket, your own HTTP server
      1. if you're serving content | HTTP -> origin server can be an
         1. Amazon S3 bucket or
         2. HTTP server 
            1. _Example:_ web server
            2. types
               1. run | Amazon EC2 instances
               2. *custom origins*
                  1. == server / you manage
   2. uses
      1. CloudFront gets your files from them
   3. allows
      1. storing the original, definitive version of your objects
1. TODO: You upload your files to your origin servers\. Your files, also known as *objects*, typically include web pages, images, and media files, but can be anything that can be served over HTTP\.

   If you're using an Amazon S3 bucket as an origin server, you can make the objects in your bucket publicly readable, so that anyone who knows the CloudFront URLs for your objects can access them\. You also have the option of keeping objects private and controlling who accesses them\. See [Serving private content with signed URLs and signed cookies](PrivateContent.md)\. 

1. You create a CloudFront *distribution*, which tells CloudFront which origin servers to get your files from when users request the files through your web site or application\. At the same time, you specify details such as whether you want CloudFront to log all requests and whether you want the distribution to be enabled as soon as it's created\.

1. CloudFront assigns a domain name to your new distribution that you can see in the CloudFront console, or that is returned in the response to a programmatic request, for example, an API request\. If you like, you can add an alternate domain name to use instead\.

1. CloudFront sends your distribution's configuration \(but not your content\) to all of its *edge locations* or *points of presence* \(POPs\)— collections of servers in geographically\-dispersed data centers where CloudFront caches copies of your files\.

As you develop your website or application, you use the domain name that CloudFront provides for your URLs\. For example, if CloudFront returns `d111111abcdef8.cloudfront.net` as the domain name for your distribution, the URL for logo\.jpg in your Amazon S3 bucket \(or in the root directory on an HTTP server\) is `https://d111111abcdef8.cloudfront.net/logo.jpg`\.

Or you can set up CloudFront to use your own domain name with your distribution\. In that case, the URL might be `https://www.example.com/logo.jpg`\.

Optionally, you can configure your origin server to add headers to the files, to indicate how long you want the files to stay in the cache in CloudFront edge locations\. By default, each file stays in an edge location for 24 hours before it expires\. The minimum expiration time is 0 seconds; there isn't a maximum expiration time\. For more information, see [Managing how long content stays in the cache \(expiration\)](Expiration.md)\.