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
  * named also as *points of presence* \(POPs\)
    * == collections of servers / geographically\-dispersed | data centers
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
      2. adding headers / indicate how long to cache files | CloudFront edge locations
         1. by default, 24 hours
         2. [0 seconds, noMaximum)
         3. [Managing how long content stays in the cache \(expiration\)](Expiration.md)
2. upload your files | your origin servers
   1. those files
      1. -- named as -- *objects*
      2. == ANYTHING / -- can be served over -- HTTP
         1. web pages,
         2. images,
         3. media files,
   2. if you're using an Amazon S3 bucket -- as an -- origin server -> you can make the objects | your bucket /
      1. publicly readable
         1. -> ANYONE / knows the CloudFront URLs for your objects -> can access them
      2. private
         1. -> control who accesses them
         2. check [Serving private content with signed URLs and signed cookies](PrivateContent.md)
3. create the CloudFront *distribution* / you can also specify if you want to
   1. log all requests
   2. enable the distribution / as soon as it's created
4. CloudFront -- assigns a -- domain name to your new distribution
   1. check in the CloudFront console,
   2. it can be customized
   3. uses
      1. urls for your content
         1. _Example1:_ if CloudFront domain name for your distribution is `d111111abcdef8.cloudfront.net` & the content is logo\.jpg -> accessed via `https://d111111abcdef8.cloudfront.net/logo.jpg`\
         2. _Example2:_ if CloudFront domain name for your distribution is `d111111abcdef8.cloudfront.net` & the content is logo\.jpg & your own domain name is www.example.com -> accessed via `https://www.example.com/logo.jpg`\
5. CloudFront -- sends your distribution's configuration (NOT your content) to -- ALL of its *edge locations* 
