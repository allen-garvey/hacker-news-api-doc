---
title:  "Retrieving every item"
date:   2016-05-18 14:01:19 -0400
---
We know how to get a single item as long as we have an item id, but how do we get those item ids? That's what the next few lessons are about. First we're going to see how to get every item. This can be helpful for you want to run data analysis or data visualization for every story, for example. As a bonus, this will also teach you how to get the latest item, such as for a system that notifies you every time new content is added.

To get the most recent item, you send a request to `https://hacker-news.firebaseio.com/v0/maxitem.json`. This returns a single value, the item id of the most recent item. We can use our new `getItem()` function to get its data.

{% highlight javascript %}
//from previous lesson
function getItem(itemId, callback){
   ...
}
$.get("https://hacker-news.firebaseio.com/v0/maxitem.json", function(mostRecentItemId){
    getItem(mostRecentItemId, function(item){ console.log(item); });
});
{% endhighlight %}

OK, so that was easy, but how do we get every item? Well conveniently, items are stored with incrementing id numbers, from the most recent down to 1. That means we can start a loop counting down, getting all the stories, or whatever item type we're interested in. The the thing to remember here is that we are getting all the stories asynchronously, so that we have to check that we have all the items first before we can do anything with them. Here's how that would work.

{% highlight javascript %}
var items = [];
function addItem(item, length){
    items.push(item);
    //check length to see if items are done downloading
    if(items.length < length){
        return;
    }
    //items are done downloading
    //filter items we want, in this case stories
    items = items.filter(function(item){ return item.type === 'story'; });
    //do whatever you want with the items here
    console.log(items);
}
$.get("https://hacker-news.firebaseio.com/v0/maxitem.json", function(mostRecentItemId){
    //get every item, starting with the most recent
    for(var id=mostRecentItemId;id>=1;id--){
        getItem(id, function(item){ addItem(item, mostRecentItemId); });
    }
});
{% endhighlight %}
