---
layout: post
title: Caching in Rails - How ETags and fresh_when work
---

<p>
  To make web pages load faster, caching is super important. Let's first look at what caching means:
</p>

<p>
  <q><i>Caching means to store content generated during the request-response cycle and to reuse it when responding to similar requests</i></q> (from <a href="http://edgeguides.rubyonrails.org/caching_with_rails.html" target="_blank">Rails Guides</a>)
</p>

<p>
  Let's break down what happens when a request is sent to a server to figure out what this all means:
</p>


<h4>First Request</h4>
<ol>
  <li>A request is sent to server</li>
  <li>Renders entire response body</li>
  <li>Generates an ETag by doing an MD% hash on entire response body (looks like below)</li>
{% highlight ruby %}
headers['ETag'] = Digest::MD5.hexdigest(body)
{% endhighlight %}
  <li>
    Rails sends back the response body as well as the ETag as a header field to the client (usually a browser)
  </li>
  <li>The client caches the response and stores the ETag</li>
</ol>

<h4>Second Request</h4>
<ol>
  <li>We request the same page again</li>
  <li>The client sends the stored ETag as a <code>If-None-Match</code>header</li>
  <li>Renders entire reponse body</li>
  <li>Generates an ETag in the same way as previous</li>
  <li>Compares ETag that was sent as header to the one that was just generated</li>
  <li>If it matched, then the body is not sent to the client and instead sent a <code>304</code> not modified status</li>
</ol>


<p>
  Basically if the page isn't modified, the browser reads the page from the cache.
</p>

<h3>So what are ETags?</h3>
<p>
  <strong>ETags</strong> stands for <strong>Entity Tags</strong>. It's essentially a key to see if the page has changed or not.
</p>

<p>
  From a client side perspective, this is awesome because now we don't have to re-download everything, everything is stored in the cache.
</p>

<p>
  But from a server side perspective, it still has to generate the ETag every single time there is a request. Processing the entire response body everytime we want to generate an ETag is not the most efficient.
</p>

<h3>Introducing <code>fresh_when</code></h3>
<p>
  You can set your custom ETags with <code>fresh_when</code>. For example, let's say you have a blog application and you want to tell rails to update the cache when an article has been updated or created. You can simply add <code>fresh_when</code> in your controller action. In this case, we're going to add it in the <code>show</code> action.
</p>

{% highlight ruby %}
def show
  @article = Article.find(params[:id])
  fresh_when(@article)
end
{% endhighlight %}


<p>
  What this does is generate an ETag for us like this:
</p>

{% highlight ruby %}
headers['Etag'] = Digest::MD5.hexdigest(@article.cache_key)
{% endhighlight %}

<p>
  The <code>cache_key</code> is what keeps track of if something has updated. For instance, <code>@article.cache_key</code> would look something like <code>'article/23-2016-224150000'</code>.
</p>

<p>
  The <code>cache_key</code> is generated in this format: <code>&lt;model name&gt;/&lt;id&gt;-&lt;updated_at&gt;</code>.
</p>

<hr>

<p>
  According to <a href="https://robots.thoughtbot.com/introduction-to-conditional-http-caching-with-rails" target="_blank">a simple test that thoughtbot ran</a>, performance improved by 94% (33ms to 2ms) just by including one <code>fresh_when</code> in a controller action. That's an amazing improvement.
</p>

<p>
  As we can see <code>fresh_when</code> is a powerful tool that we can use to improve our caching. Instead of processing the whole response body, we can focus on tracking if a resource is stale or fresh by using <code>fresh_when</code>.
</p>


<p>
  Happy coding!
</p>







