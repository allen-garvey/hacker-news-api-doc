---
title:  "Retrieving an item"
date:   2016-05-18 12:01:19 -0400
---
Now we finally get to dive into some code! We're going to start with the basics, retrieving a single item. Items are at the heart of the Hacker News API, since there are only two basic data types, items and users. We're going to focus on items in this guide, because they generally have more interesting information associated with them, and connect more with other parts of the API. Items are everything that are not user information&mdash;which in this case are stories and comments. Stories are user submitted links, job posts, polls or questions, while comments are comments about a story.

## Technical Issues
Since the Hacker News API is public and unsecured, that means you do not have to pay or register for an API key. It also does not have any access control, which means you are free to query it from client-side code as well as from the server.

## Requesting an Item
To get the information about an item, just send a GET request to the API at this url `https://hacker-news.firebaseio.com/v0/item/{item_id}.json,` replacing `{item_id}` with the id of an item. You can do this in the browser, the same as you would a normal url, such as by following this link [https://hacker-news.firebaseio.com/v0/item/11708840.json.](https://hacker-news.firebaseio.com/v0/item/11708840.json) For a full explanation of all the fields and what they mean, [see the official documentation here](https://github.com/HackerNews/API#items).

To do this with jQuery is almost as easy.

{% highlight javascript %}
$.get("https://hacker-news.firebaseio.com/v0/item/11708840.json", function(item){
    console.log(item);
});
{% endhighlight %}

Since we're going to be doing this a lot, let's wrap this in a function so we can easily reuse it. For parameters, we'll use the item id and a callback, saying what to do with the item once we get it.

{% highlight javascript %}
function getItem(itemId, callback){
    $.get("https://hacker-news.firebaseio.com/v0/item/" + itemId + ".json", function(item){
        callback(item);
    });	
}

{% endhighlight %}
