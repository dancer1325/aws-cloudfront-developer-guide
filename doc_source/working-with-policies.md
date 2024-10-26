# Working with policies<a name="working-with-policies"></a>

* built-in kinds of *policies*
  * CloudFront *cache policy*
    * allows
      * specifying
        * 👀characteristics / included | *cache key* 👀
          * HTTP headers,
          * cookies,
          * query strings
        * TTL settings -- for -- objects | CloudFront cache
      * enabling CloudFront -- to -- request & cache compressed objects
    * ways to create it
      * CloudFront console
      * AWS CLI
      * CloudFront API

  * CloudFront *origin request policy* 
    * allows
      * specifying
        * 👀characteristics / included | *origin requests* (NOT | cache key) 👀
          * HTTP headers,
          * cookies
          * query strings
    * ALL values | cache policy -> automatically included | origin requests

  * CloudFront *response headers policy*
    * allows
      * specifying (adding or removing) HTTP headers / CloudFront includes | HTTP responses / -- sent to -- viewers (== web browsers or other clients\)
        * / NO 
          * changes | your origin
          * writing code

* cache key
  * -- determines if -- a viewer's HTTP request -- results in a -- *cache hit*
    * == object -- is served, from the CloudFront cache, to the -- viewer 
    * 👀if you include fewer values | cache key -> higher likelihood of a cache hit 👀
* origin requests
  * == requests / CloudFront -- sends to the -- origin | there's a cache miss


**Other next Topics**
+ [Controlling the cache key](controlling-the-cache-key.md)
+ [Controlling origin requests](controlling-origin-requests.md)
+ [Adding or removing response headers](modifying-response-headers.md)