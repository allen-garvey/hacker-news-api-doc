---
title:  "Retrieving a story's comments"
date:   2016-05-19 17:01:19 -0400
---
Now for the hardest task yet&mdash;retrieving all the comments for a story. Each item has an array called `kids` that contains the top level comments for that item. To get all the comments for the story, we will have to retrieve its kids, and then the kids of the kids, and so on, until we have retrieved all the comments. You can visualize it as a tree link this.
{% highlight javascript %}
                Story
            /      |        \
        Comment    Comment   Comment
    /         \                 \
   Comment    Comment           Comment
{% endhighlight %}
The final product that we want is a story object that contains an array called comments. Each item in that array is a comment object that also contains an array called comments that contains the child comment objects for that comment. We're going to create a function called `getComments()` that uses recursion to retrieve all the comments for a story. Again, since we are using asynchronous functions to get the comments, we need to know when we are done. Stories have a property called descendants, which is a count of the total number of comments. We can use that to keep track of how many comments are left to get.

{% highlight javascript %}
//from previous lesson
function getItem(itemId, callback){
   ...
}
var commentsToGet = 0;
function getComments(commentIds, commentContainer, doneCallback){
    for(var i=0;i<commentIds.length;i++){
        getItem(commentIds[i], function(item){
            item.comments = [];
            getComments(item.kids, item.comments, doneCallback);
            commentContainer.push(item);
        });
    }
    commentsToGet--;
    if(commentsToGet <= 0){
        //we're done, do something with story and comments
        doneCallback();
    }
}
getItem(11708840, function(item){
    item.comments = [];
    commentsToGet = item.descendants;
    getComments(item.kids, item.comments, function(){ console.log(item); });
});
{% endhighlight %}
