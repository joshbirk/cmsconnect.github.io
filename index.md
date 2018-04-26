# Integrate external content into Communities using CMS Connect JSON


Launched in Summer ’17, CMS Connect displays content from an external Content Management System (CMS) in your Salesforce Lightning Communities. You can reuse content and maintain the look and feel of your website brand. CMS Connect can pull content from Adobe Experience Manager, WordPress, Drupal, Sitecore, SDL, and others that support content structured as JSON or HTML fragments.
This blog focuses on CMS Connect (JSON). For CMS Connect (HTML), see [_Connect Your Community to Your Content Management System_](https://developer.salesforce.com/docs/atlas.en-us.communities_dev.meta/communities_dev/communities_dev_cms.htm).

While this blog uses WordPress as an example, it applies to any CMS that supports JSON. Start with these [prerequisites](https://developer.salesforce.com/docs/atlas.en-us.communities_dev.meta/communities_dev/communities_dev_cms_prerequisites.htm), including Cross-Origin Resource Sharing (CORS). Add your Community host to the list of trusted hosts in the CORS header in your CMS system.

## Get the JSON URL

Identify the JSON URL for external content. Supported MIME[](http://en.wikipedia.org/wiki/MIME)media type is [_application/json_](https://tools.ietf.org/html/rfc4627).
For example, here's how to get an endpoint from WordPress:

|
**Type**	|
**Description**	|
**Example**	|
|---	|---	|---	|
|**WP Rest API**	|Send a request to */wp-json/wp/v2/posts *by appending it to your site*. *For details, see [_WP REST API_](http://v2.wp-api.org/).	|If your site is *[https://myblog.com](https://myblog.com/), *JSON URL to get content is* https://myblog.com/wp-json/wp/v2/posts*	|
|[WordPress.com](http://wordpress.com/)	|Use this format: *[https://public-api.wordpress.com/rest/v1.1/sites/$site/posts](https://public-api.wordpress.com/rest/v1.1/sites/$site/posts/)*. For details, see [_WordPress REST API_](https://developer.wordpress.com/docs/api/1.1/get/sites/%24site/).	|If your site is * [https://myblog.wordpress.com](https://myblog.wordpress.com/),* JSON URL to get content is * https://public-api.wordpress.com/rest/v1.1/sites/myblog.wordpress.com/posts*	|

And here are some URL examples for different assets in WordPress**:**

|
**Type**	|
**Description**	|
**Example**	|
|---	|---	|---	|
|**Post**	|To get content for a single blog, append `postId` at the end of the URL.	|https://public-api.wordpress.com/rest/v1.1/sites/myblog.wordpress.com/posts/$postId	|
|**Media Files**	|Media files in a library of a particular WordPress site can be accessed using REST calls.	|https://public-api.wordpress.com/rest/v1.1/sites/$site/media/	|
|**Embedded Content**	|Specify the **_embed** parameter to indicate to the server that response should include all embedded resources in same request. See WordPress [__embed_](https://developer.wordpress.org/rest-api/using-the-rest-api/global-parameters/#_envelope) parameter.	|https://myblog.com/wp-json/wp/v2/posts?_embed	|



## CMS Connect JSON Configuration

Configuration involves two simple steps:

1. In Community Workspaces, create a connection to your CMS, defining content items and lists, and supplying an endpoint and paths.
2. Using the Community Builder, drag a CMS Connect (JSON) component to the page. Configure its properties using JSON expressions for your payload.

## What's a Content Item, Content List, & Content Type?

CMS Connect JSON supports two kinds of content: *content items* and *content lists*. A content item, for example, is a single blog that's displayed as a full post. A content list is a group of items, such as a blog series. Each list item shows a blurb and a link to the full blog. *Content Type* is used for grouping related content lists and items. For example,

|
Content Type	|
Content List(s)	|
Content Item	|
|---	|---	|---	|
|Blogs	|Featured Blogs
Trending Blogs
Top Blogs	|Blog Item	|
|News	|Trending News
Political News
Sports News	|News Item	|

You can add up to 5 different JSON Content Types in a connection, each with up to 1 JSON Content Item and 10 JSON Content Lists.

## Determine Server URL and JSON Paths

When you configure a CMS connection, you specify the server URL and the JSON path to the content item or list. Suppose I'm retrieving blogs from a website hosted on WordPress, *https://capricornblog.wordpress.com*. Here’s my JSON URLs.

|
Name	|
URL	|
|---	|---	|
|Content List	|*https://public-api.wordpress.com/rest/v1.1/sites/capricornblog.wordpress.com/posts*	|
|Content Item	|*https://public-api.wordpress.com/rest/v1.1/sites/capricornblog.wordpress.com/posts/{component}*	|

When I set up the CMS connection, I enter these values for Server URL, Content List Path, and Content Item Path:

|Name	|
Value	|
|---	|---	|
|Server URL 	|[*https://public-api.wordpress.com*](https://public-api.wordpress.com/rest/v1.1/sites/capricorncafeblog.wordpress.com/posts)	|
|JSON Path for Content List 	|*rest/v1.1/sites/capricornblog.wordpress.com/posts*
**Note: This path is relative to the Server URL**	|
|JSON Path for Content Item 	|[*rest/v1.1/sites/capricornblog.wordpress.com/posts*](https://public-api.wordpress.com/rest/v1.1/sites/capricorncafeblog.wordpress.com/posts)*/{component}*
**Note: This path is relative to the Server URL**	|


## Create a CMS Connection

* Go to Community Workspaces.
* Click **CMS Connect**.
* Click **New **to create a new CMS Connection. 
* For **Name**, enter a friendly name for the connection, for example, ***Capricorn Blogs***.
* For **CMS Source**, such as WordPress. Choose Other only if your CMS is not listed. 
* For **Server URL**, enter the path, such as [***_https://public-api.wordpress.com_***](https://public-api.wordpress.com/). Only HTTPS is allowed.
* **Do not enter a Root Path for JSON**; it is needed only for HTML.
* Under JSON, click **Add JSON**. For information on properties, see this [_example_](https://developer.salesforce.com/docs/atlas.en-us.communities_dev.meta/communities_dev/communities_dev_cms_example_json.htm).
* **Save** the settings.




## Learn how to extract a JSON Field Path

Using the Community Builder, you insert a **CMS Connect (JSON)** component on your page, defining properties such as ID, Title, Author, etc. But what do you enter for the properties? Previewing the JSON response shows you the JSON expressions to use for paths to the content. To see a preview, enter the combined Server URL and JSON Path in a browser.

Let’s look at some example JSON responses, and the expressions you enter:

Here’s another example. When you specify the **_embed** parameter in a request, the payload includes **_embedded** content:

The syntax follows the [_JSON Pointer specification_](https://tools.ietf.org/html/rfc6901). For more examples, see [_CMS Connect (JSON) Expressions_](https://developer.salesforce.com/docs/atlas.en-us.communities_dev.meta/communities_dev/communities_dev_cms_json_expressions.htm).

## Steps in the Community Builder 

After you configure the CMS connection, switch to the Community Builder.

1. Drag a CMS Connect (JSON) component to the page. 
2. Select your CMS Source.
3. For JSON Content, select the content item or content list.
4. In the component's property editor, configure the properties. 

The options  you see depend on whether you select a content item or content list.  In the** **property editor, prepend each path with **@** to indicate it is a JSON expression. (In Community Workspaces, the fields are always a JSON path, so @ is not needed.)   

Here’s an example content list. For more information on each of the properties, see [_CMS Connect (JSON)_](https://help.salesforce.com/articleView?id=rss_cms_connect_json.htm).


Congratulations! Your Lightning Community now displays your WordPress blogs:

## Detail Page Navigation

When you configure a component for a content list, a CMS detail page is automatically created. When you click **Read More**, an item’s `postId` is added to the URL path to retrieve item content. In addition, the detail page enables SEO for that content.

The default page name is {CMSSourceName-ContentTypeName}. In this example, the CMS Source is ***Capricorn Wordpress*** and the Content Type  is ***Blog***.

The page's runtime URL is similar, for example, ***capricorn-wordpress-blog***. You can change both the page name and URL from Page Properties.





## Summary

Here are the key takeaways:

* If your CMS supports JSON APIs, you can easily reuse content from in your communities using CMS Connect (JSON).
* Start by creating a CMS connection and identifying the appropriate JSON endpoint for retrieving content.
* Add the CMS Connect (JSON) component to your community pages, and configure its  properties.
* When you preview or publish your community page, you see the CMS content.

## Additional Tips

What if you went through the steps and content doesn't display?

* Check CORS. Add the Community host to the list of trusted hosts in the CORS header in your CMS.
* The **Server URL** and **JSON Path** should be a valid URL that returns a JSON response. For JSON, don't enter a **Root Path**. Check media type of JSON Response should be [_application/json_](https://tools.ietf.org/html/rfc4627).
* In the Community Builder, verify the fields in the property editor. For JSON expressions, prepend @. JSON properties are case-sensitive, and should match your CMS payload.
* If a detail page doesn't appear or navigation doesn't work, check your connection settings.
    * Verify that there's a content item and content list under the same content type name.
    * Verify the ID and title of the content item. For the connection, don't prepend @ because JSON expressions are expected.

## Resources

* [_Video_](https://youtu.be/6QrRIyTmUlo): How to Reuse CMS Content in your community with CMS Connect JSON.
* [_Webinar_](https://developer.salesforce.com/events/webinars/integrate-CMS-into-Lightning-Communities): CMS Connect
* Configure Search for CMS Connect JSON Content: [_Set Up Federated Search in Communities_](https://help.salesforce.com/articleView?id=networks_search_federated.htm&type=5)
* Trailhead Module: [_Community Cloud Basics_](https://trailhead.salesforce.com/en/modules/community_cloud_basics)

## About the Author

Shipra Shreyasi ([_@sshreyasi_](https://twitter.com/sshreyasi)) is an engineer in the Community Cloud team who has been working on building a first-class product and CMS Connect implementation.

