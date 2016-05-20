---
title:  "Retrieving the top stories"
date:   2016-05-18 17:01:19 -0400
---
Now we're going to learn how to use the API to retrieve the top 500 stories of a certain type. You can see all the categories you can retrieve by [in the official documentation, along with their respective urls.](https://github.com/HackerNews/API/#user-content-new-top-and-best-stories) 

The newest stories are the most recently posted stories, best stories are the best stories of all time by rating, and the top stories are stories that are a combination of being recently posted and having a high rating, and are equivalent to what is shown on the Hacker News homepage. Job stories are job listings, either people looking for jobs or companies offering jobs, 'show' stories are people showing off their projects and 'ask' stories are people asking for advice. 

We're going to show how to get all the top stories, since it is what you'll need to be able to do if you want to make your own Hacker News clone. Fortunately, it's not too different from getting every story, which we have just learned in the previous lesson. Also, if you can get all the top stories, it's exactly the same to get all stories of a different category, you just change the url.

All the 'retrieve stories by type' urls return an array of item ids, so be looping through the ids, similarly to how we did it in the previous lesson, we can get all the stories. Remember also that we are getting the stories asynchronously, so we have to check that we have all the items before doing anything.

{% highlight javascript %}
var items = [];
//from previous lessons
function getItem(itemId, callback){
   ...
}
function addItem(item, length){
   ...
}
$.get("https://hacker-news.firebaseio.com/v0/topstories.json", function(storyIds){
    //loop through the array of ids to get all the stories
    for(var i=0;i<storyIds.length;i++){
        getItem(storyIds[i], function(item){ addItem(item, storyIds.length); });
    }
});
{% endhighlight %}

