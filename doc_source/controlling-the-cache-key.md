# Controlling the cache key<a name="controlling-the-cache-key"></a>

* *cache key*
  * use cases
    * objects / are cached | CloudFront edge locations
  * 👀:= unique identifier / EVERY object | cache 👀
  * -- determines if -- a viewer's HTTP request -- results in a -- *cache hit*
  * ways to control it
    * use a CloudFront *cache policy*  
      * == attach a cache policy | >=1 cache behaviors | CloudFront distribution
* *cache hit*
  * requirements to happen
    * viewer request -- generates a -- cache key / == prior request
    * object / -- associated to -- that cache key is
      * | edge location's cache
      * valid
  * if a cache hit happens -> object -- is served, to the viewer, from a -- CloudFront edge location
    * 👀-> benefits 👀
      * Reduced load | your origin server
      * Reduced latency -- for the -- viewer
* *cache hit ratio*
  * == ratio of viewer requests / -- result in a -- cache hit
  * as higher cache hit ratio -> better performance from your website or application
  * 👀ways to increase 👀
    * include ONLY the minimum necessary values | cache key
      * check [Understanding the cache key](understanding-the-cache-key.md)

**Topics**
+ [Creating cache policies](#cache-key-create-cache-policy)
+ [Understanding cache policies](#cache-key-understand-cache-policy)
+ [Using the managed cache policies](using-managed-cache-policies.md)
+ [Understanding the cache key](understanding-the-cache-key.md)

## Creating cache policies<a name="cache-key-create-cache-policy"></a>

------
#### [ Console ]

**To create a cache policy \(console\)**

1. Sign in to the AWS Management Console and open the **Policies** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home?#/policies](https://console.aws.amazon.com/cloudfront/v3/home?#/policies)\.

1. Choose **Create cache policy**\.

1. Choose the desired setting for this cache policy\. For more information, see [Understanding cache policies](#cache-key-understand-cache-policy)\.

1. When finished, choose **Create**\.

After you create a cache policy, you can attach it to a cache behavior\.

**To attach a cache policy to an existing distribution \(console\)**

1. Open the **Distributions** page in the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home#/distributions](https://console.aws.amazon.com/cloudfront/v3/home#/distributions)\.

1. Choose the distribution to update, then choose the **Behaviors** tab\.

1. Choose the cache behavior to update, then choose **Edit**\.

   Or, to create a new cache behavior, choose **Create behavior**\.

1. In the **Cache key and origin requests** section, make sure that **Cache policy and origin request policy** is chosen\.

1. For **Cache policy**, choose the cache policy to attach to this cache behavior\.

1. At the bottom of the page, choose **Save changes**\.

**To attach a cache policy to a new distribution \(console\)**

1. Open the CloudFront console at [https://console.aws.amazon.com/cloudfront/v3/home](https://console.aws.amazon.com/cloudfront/v3/home)\.

1. Choose **Create distribution**\.

1. In the **Cache key and origin requests** section, make sure that **Cache policy and origin request policy** is chosen\.

1. For **Cache policy**, choose the cache policy to attach to this distribution's default cache behavior\.

1. Choose the desired settings for the origin, default cache behavior, and other distribution settings\. For more information, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md)\.

1. When finished, choose **Create distribution**\.

------
#### [ CLI ]

 create\-cache\-policy command\. You can use an input file to provide the command's input parameters, rather than specifying each individual parameter as command line input\.

1. **create a cache policy -- via -- input file** 
   1. create a `cache-policy.yaml` / contains ALL input parameters -- for the -- `create-cache-policy` command

      ```
      aws cloudfront create-cache-policy --generate-cli-skeleton yaml-input > cache-policy.yaml
      ```

   2. edit `cache-policy.yaml` 
      1. NOT remove the required fields
      2. check [Understanding cache policies](#cache-key-understand-cache-policy)

   3. create the cache policy -- via -- input parameters from the `cache-policy.yaml` file

      ```
      aws cloudfront create-cache-policy --cli-input-yaml file://cache-policy.yaml
      ```

      1. return the cache policy ID

2. **attach a cache policy | EXISTING distribution**
   1. save the CloudFront distribution configuration / you want to update

      ```
      aws cloudfront get-distribution-config --id distributionID --output yaml > dist-config.yaml
      ```

   2. edit `dist-config.yaml` / 👀goal: use a cache policy | SOME cache behavior 👀
      + | cache behavior
        + add a field named `CachePolicyId` / value = created previous step
        + remove the `MinTTL`, `MaxTTL`, `DefaultTTL`, and `ForwardedValues` fields 
          + Reason: 🧠 These settings are specified | cache policy -> NOT possible to include these fields & cache policy 🧠
      + rename the `ETag` field -- to -- `IfMatch`

   3. use the cache policy | distribution

      ```
      aws cloudfront update-distribution --id distributionID --cli-input-yaml file://dist-config.yaml
      ```

3. **attach a cache policy | NEW distribution**
   1. create a `distribution.yaml` / contains ALL input parameters -- for the -- `create-distribution` command

      ```
      aws cloudfront create-distribution --generate-cli-skeleton yaml-input > distribution.yaml
      ```

   2. edit `distribution.yaml`
      1. | default cache behavior, set `CachePolicyId` / value = created first step
      2. specify other distribution settings / you want to adjust
      3. check [more information about the distribution settings](distribution-web-values-specify.md)

   3. Use the following command to create the distribution using input parameters from the `distribution.yaml` file\.

      ```
      aws cloudfront create-distribution --cli-input-yaml file://distribution.yaml
      ```

------
#### [ API ]

To create a cache policy with the CloudFront API, use [CreateCachePolicy](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateCachePolicy.html)\. For more information about the fields that you specify in this API call, see [Understanding cache policies](#cache-key-understand-cache-policy) and the API reference documentation for your AWS SDK or other API client\.

After you create a cache policy, you can attach it to a cache behavior, using one of the following API calls:
+ To attach it to a cache behavior in an existing distribution, use [UpdateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_UpdateDistribution.html)\.
+ To attach it to a cache behavior in a new distribution, use [CreateDistribution](https://docs.aws.amazon.com/cloudfront/latest/APIReference/API_CreateDistribution.html)\.

For both of these API calls, provide the cache policy's ID in the `CachePolicyId` field, inside a cache behavior\. For more information about the other fields that you specify in these API calls, see [Values that you specify when you create or update a distribution](distribution-web-values-specify.md) and the API reference documentation for your AWS SDK or other API client\.

------

## Understanding cache policies<a name="cache-key-understand-cache-policy"></a>

* *managed policies*
  * 👀 == built-in CloudFront cache policies 👀
    * -- for -- common use cases 
  * [Using the managed cache policies](using-managed-cache-policies.md)
* 👀cache policy's setting categories 👀
  * *policy information*
  * *TTL settings*
  * *cache key settings*
---
### Policy information<a name="cache-key-understand-cache-policy-info"></a>

* **Name**  
  * allows
    * identifying the cache policy
  * uses
    * attach the cache policy | cache behavior

* **Description**  
* == comment / describe the cache policy
  * optional
---
### TTL settings<a name="cache-key-understand-cache-policy-ttl"></a>

* \+ HTTP headers' `Cache-Control` & `Expires` | origin response -> determine how long objects | CloudFront cache remain valid
  * these HTTP headers are optional to exist

* **Minimum TTL**  
  * := minimum amount of time (| seconds) / objects must stay | CloudFront cache -- before -- CloudFront checks if the object has been updated
  * see [Managing how long content stays in the cache \(expiration\)](Expiration.md)

* **Maximum TTL**  
  * := maximum amount of time (|seconds) / objects must stay | CloudFront cache -- before -- CloudFront checks if the object has been updated
  * requirements
    * origin must send `Cache-Control` or `Expires` headers + object
  * see [Managing how long content stays in the cache \(expiration\)](Expiration.md)\

* **Default TTL**  
  * == default amount of time (|seconds) /  objects must stay | CloudFront cache -- before -- CloudFront checks if the object has been updated
  * requirements
    * origin does NOT send `Cache-Control` or `Expires` headers + object
  * see [Managing how long content stays in the cache \(expiration\)](Expiration.md)
---
### Cache key settings<a name="cache-key-understand-cache-policy-settings"></a>

* specify
  * values | viewer requests / CloudFront includes | cache key
    * these values -- are automatically -- included | requests / CloudFront -- sends to the -- origin
      * origin -- named as -- *origin requests* 
* [Controlling origin requests / cache key NOT affected](controlling-origin-requests.md)
* categories
  * [Headers](#cache-policy-headers)
  * [Cookies](#cache-policy-cookies)
  * [Query strings](#cache-policy-query-strings)
  * [Compression support](#cache-policy-compressed-objects)

* **Headers**  
  * == HTTP headers | viewer requests / 
    * CloudFront includes | 
      * cache key &
      * origin requests
  * available settings  
    + **None**
      + == HTTP headers | viewer requests are *NOT* included |
        + cache key
        + origin requests
    + **Include the following headers**
      + == specify which of the HTTP headers | viewer requests / are included
        + [possible to include headers / -- generated by --  CloudFront | cache key ](adding-cloudfront-headers.md) 
      + HTTP headers -- are specified by their -- name
        + _Example:_ let's have the HTTP header  

          ```
          Accept-Language: en-US,en;q=0.5
          ```
          -> header -- is specified as -- `Accept-Language`
        + BUT, CloudFront -- includes the -- full header == name + its value

* **Cookies**  
  * TODO:
The cookies in viewer requests that CloudFront includes in the cache key and in origin requests\. For cookies, you can choose one of the following settings:  
+ **None** – The cookies in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\.
+ **All** – All cookies in viewer requests are included in the cache key and are automatically included in origin requests\.
+ **Include specified cookies** – You specify which of the cookies in viewer requests are included in the cache key and automatically included in origin requests\.
+ **Include all cookies except** – You specify which of the cookies in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\. All other cookies, except for the ones you specify, *are* included in the cache key and automatically included in origin requests\.
When you use the **Include specified cookies** or **Include all cookies except** setting, you specify cookies by their name, not their value\. For example, consider the following `Cookie` header:  

```
Cookie: session_ID=abcd1234
```
In this case, you specify the cookie as `session_ID`, not as `session_ID=abcd1234`\. However, CloudFront includes the full cookie, including its value, in the cache key and in origin requests\.





* **Query strings**  
The URL query strings in viewer requests that CloudFront includes in the cache key and in origin requests\. For query strings, you can choose one of the following settings:  
+ **None** – The query strings in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\.
+ **All** – All query strings in viewer requests are included in the cache key and are also automatically included in origin requests\.
+ **Include specified query strings** – You specify which of the query strings in viewer requests are included in the cache key and automatically included in origin requests\.
+ **Include all query strings except** – You specify which of the query strings in viewer requests are *not* included in the cache key and are *not* automatically included in origin requests\. All other query strings, except for the ones you specify, *are* included in the cache key and automatically included in origin requests\.
When you use the **Include specified query strings** or **Include all query strings except** setting, you specify query strings by their name, not their value\. For example, consider the following URL path:  

```
/content/stories/example-story.html?split-pages=false
```
In this case, you specify the query string as `split-pages`, not as `split-pages=false`\. However, CloudFront includes the full query string, including its value, in the cache key and in origin requests\.





* **Compression support**  
These settings enable CloudFront to request and cache objects that are compressed in the Gzip or Brotli compression formats, when the viewer supports it\. These settings also allow [CloudFront compression](ServingCompressedFiles.md) to work\. Viewers indicate their support for these compression formats with the `Accept-Encoding` HTTP header\.  
The Chrome and Firefox web browsers support Brotli compression only when the request is sent using HTTPS\. These browsers do not support Brotli with HTTP requests\.
Enable these settings when any of the following are true:  
+ Your origin returns Gzip compressed objects when viewers support them \(requests contain the `Accept-Encoding` HTTP header with `gzip` as a value\)\. In this case, use the **Gzip enabled** setting \(set `EnableAcceptEncodingGzip` to `true` in the CloudFront API, AWS SDKs, AWS CLI, or AWS CloudFormation\)\.
+ Your origin returns Brotli compressed objects when viewers support them \(requests contain the `Accept-Encoding` HTTP header with `br` as a value\)\. In this case, use the **Brotli enabled** setting \(set `EnableAcceptEncodingBrotli` to `true` in the CloudFront API, AWS SDKs, AWS CLI, or AWS CloudFormation\)\.
+ The cache behavior that this cache policy is attached to is configured with [CloudFront compression](ServingCompressedFiles.md)\. In this case, you can enable caching for either Gzip or Brotli, or both\. When CloudFront compression is enabled, enabling caching for both formats can help to reduce your costs for data transfer out to the internet\.
If you enable caching for one or both of these compression formats, do not include the `Accept-Encoding` header in an [origin request policy](controlling-origin-requests.md) that's associated with the same cache behavior\. CloudFront always includes this header in origin requests when caching is enabled for either of these formats, so including `Accept-Encoding` in an origin request policy has no effect\.
If your origin server does not return Gzip or Brotli compressed objects, or the cache behavior is not configured with CloudFront compression, don't enable caching for compressed objects\. If you do, it might cause a decrease in your [cache hit ratio](cache-hit-ratio.md)\.  
The following explains how these settings affect a CloudFront distribution\. All of the following scenarios assume that the viewer request includes the `Accept-Encoding` header\. When the viewer request does not include the `Accept-Encoding` header, CloudFront doesn't include this header in the cache key and doesn't include it in the corresponding origin request\.    
**When caching compressed objects is enabled for both compression formats**  
If the viewer supports both Gzip and Brotli—that is, if the `gzip` and `br` values are both in the `Accept-Encoding` header in the viewer request—CloudFront does the following:  
+ Normalizes the header to `Accept-Encoding: br,gzip` and includes the normalized header in the cache key\. The cache key doesn't include other values that were in the `Accept-Encoding` header sent by the viewer\.
+ If the edge location has a Brotli or Gzip compressed object in the cache that matches the request and is not expired, the edge location returns the object to the viewer\.
+ If the edge location doesn't have a Brotli or Gzip compressed object in the cache that matches the request and is not expired, CloudFront includes the normalized header \(`Accept-Encoding: br,gzip`\) in the corresponding origin request\. The origin request doesn't include other values that were in the `Accept-Encoding` header sent by the viewer\.
If the viewer supports one compression format but not the other—for example, if `gzip` is a value in the `Accept-Encoding` header in the viewer request but `br` is not—CloudFront does the following:  
+ Normalizes the header to `Accept-Encoding: gzip` and includes the normalized header in the cache key\. The cache key doesn't include other values that were in the `Accept-Encoding` header sent by the viewer\.
+ If the edge location has a Gzip compressed object in the cache that matches the request and is not expired, the edge location returns the object to the viewer\.
+ If the edge location doesn't have a Gzip compressed object in the cache that matches the request and is not expired, CloudFront includes the normalized header \(`Accept-Encoding: gzip`\) in the corresponding origin request\. The origin request doesn't include other values that were in the `Accept-Encoding` header sent by the viewer\.
To understand what CloudFront does if the viewer supports Brotli but not Gzip, replace the two compression formats with each other in the preceding example\.  
If the viewer does not support Brotli or Gzip—that is, the `Accept-Encoding` header in the viewer request does not contain `br` or `gzip` as values—CloudFront:  
+ Doesn't include the `Accept-Encoding` header in the cache key\.
+ Includes `Accept-Encoding: identity` in the corresponding origin request\. The origin request doesn't include other values that were in the `Accept-Encoding` header sent by the viewer\.  
**When caching compressed objects is enabled for one compression format, but not the other**  
If the viewer supports the format for which caching is enabled—for example, if caching compressed objects is enabled for Gzip and the viewer supports Gzip \(`gzip` is one of the values in the `Accept-Encoding` header in the viewer request\)—CloudFront does the following:  
+ Normalizes the header to `Accept-Encoding: gzip` and includes the normalized header in the cache key\.
+ If the edge location has a Gzip compressed object in the cache that matches the request and is not expired, the edge location returns the object to the viewer\.
+ If the edge location doesn't have a Gzip compressed object in the cache that matches the request and is not expired, CloudFront includes the normalized header \(`Accept-Encoding: gzip`\) in the corresponding origin request\. The origin request doesn't include other values that were in the `Accept-Encoding` header sent by the viewer\.
This behavior is the same when the viewer supports both Gzip and Brotli \(the `Accept-Encoding` header in the viewer request includes both `gzip` *and* `br` as values\), because in this scenario, caching compressed objects for Brotli is not enabled\.  
To understand what CloudFront does if caching compressed objects is enabled for Brotli but not Gzip, replace the two compression formats with each other in the preceding example\.  
If the viewer does not support the compression format for which caching is enabled \(the `Accept-Encoding` header in the viewer request doesn't contain the value for that format\), CloudFront:  
+ Doesn't include the `Accept-Encoding` header in the cache key\.
+ Includes `Accept-Encoding: identity` in the corresponding origin request\. The origin request doesn't include other values that were in the `Accept-Encoding` header sent by the viewer\.  
**When caching compressed objects is disabled for both compression formats**  
When caching compressed objects is disabled for both compression formats, CloudFront treats the `Accept-Encoding` header the same as any other HTTP header in the viewer request\. By default, it's not included in the cache key and it's not included in origin requests\. You can include it in the headers list in a cache policy or an origin request policy the same as any other HTTP header\.