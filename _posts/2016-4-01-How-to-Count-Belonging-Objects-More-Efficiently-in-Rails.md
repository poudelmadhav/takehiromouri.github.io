---
layout: post
title: How to Count Belonging Objects More Efficiently in Rails
---

A lot of times when writing rails apps, you face a situation where you have to count the number of child objects a parent object has. 

For instance, let's say we have a <code>User</code> model and a <code>Comment</code> model where <code>User</code> <code>has_many :comments</code> and <code>Comment</code> <code>belongs_to :user</code>.

{% highlight ruby %}
class User < ActiveRecord::Base
  has_many :comments
end
{% endhighlight %}

{% highlight ruby %}
class Comment < ActiveRecord::Base
  belongs_to :user
end
{% endhighlight %}

What do we do when we want to count the number of comments that a <code>User</code> has? A lot of times, it would look something like this:

{% highlight ruby %}
user.comments.count
{% endhighlight %}

This is all fine, if you're only using it once in a while. 
But say we need to query the database often for the number of comments that a <code>User</code> has. 

Let's see what the performance is like when using <code>user.comments.count</code>.
In an example app I created with the same models as above, I tested this out with a <code>user</code> with 225 <code>comments</code>:

{% highlight bash %}
irb(main):015:0> User.first.comments.count
  User Load (0.3ms)  SELECT  "users".* FROM "users"  ORDER BY "users"."id" ASC LIMIT 1
   (0.4ms)  SELECT COUNT(*) FROM "comments" WHERE "comments"."user_id" = ?  [["user_id", 1]]
=> 225
{% endhighlight %}

Notice how the <code>comments</code> are queried with a COUNT(*) query. If we were to query 100 users at once for <code>comments.count</code>, it would be a big load for the database.

Here's how we can avoid the COUNT(*) query and improve performance:

<h2>Introducing <code>counter_cache</code></h2>

Instead of querying <code>comments</code>, we can just store the number of comments a user has in the <code>User</code> table and get the number of comments without any SQL query.

{% highlight ruby %}
class AddCommentCounterToUser < ActiveRecord::Migration
  def change
    add_column :users, :comments_count, :integer, default: 0
  end
end
{% endhighlight %}

In order for <code>counter_cache</code> to work, we need to create a database column that ends with <code>_count</code>. So here, we have created a <code>comments_count</code> column that will store the number of comments inside.

After we run the migration, we want to initially update the <code>comments_count</code> for each <code>User</code>.

We can do this by creating a rake file, or manually running code in the console:

{% highlight ruby %}
User.reset_column_information
User.all.each do |user|
  User.update_counters user.id, comments_count: user.comments.length
end
{% endhighlight %}

<code>reset_column_information</code> resets all the cached information about columns, and will be reloaded before the code below runs.

Now after we run this code, we see that the correct number of comments are stored in the user object:

{% highlight bash %}
irb(main):005:0> user
=> #<User id: 1, name: "Bob", created_at: "2016-04-01 06:51:53", updated_at: "2016-04-01 07:37:39", comments_count: 225>
{% endhighlight %}

One more thing we need to do:

In the <code>Comment</code> model, we need to set <code>counter_cache: true</code>.

{% highlight ruby %}
class Comment < ActiveRecord::Base
  belongs_to :user, counter_cache: true
end
{% endhighlight %}

This will increment and decrement the <code>comments_count</code> column automatically in <code>User</code> whenever a comment is created or deleted.

Let's test this out:

{% highlight bash %}
irb(main):006:0> user.comments.create(text: "hello")
   (1.7ms)  begin transaction
  SQL (1.9ms)  INSERT INTO "comments" ("text", "user_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["text", "hello"], ["user_id", 1], ["created_at", "2016-04-02 18:27:36.897717"], ["updated_at", "2016-04-02 18:27:36.897717"]]
  SQL (0.6ms)  UPDATE "users" SET "comments_count" = COALESCE("comments_count", 0) + 1 WHERE "users"."id" = ?  [["id", 1]]
   (5.9ms)  commit transaction
=> #<Comment id: 225, user_id: 1, text: "hello", created_at: "2016-04-02 18:27:36", updated_at: "2016-04-02 18:27:36">
irb(main):007:0> user.comments_count
=> 226
{% endhighlight %}

Awesome! As you can see, it properly incremented the <code>comments_count</code> from <code>225</code> to <code>226</code>.

Using <code>counter_cache</code> can increase performance when you are trying to display, for instance, many users and their comment counts. Instead of making a bunch of COUNT(*) queries, by using <code>counter_cache</code>, you can access the same information by just accessing a column in the <code>User</code> table.

