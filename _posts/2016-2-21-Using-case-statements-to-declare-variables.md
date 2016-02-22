---
layout: post
title: Using case statements to declare variables
---

<p>
  If you wanted to assign different variables depending on several different situations, you might be tempted to do so like this:
</p>

{% highlight ruby %}
if model == 'Apple MacBook Air 13.3'
  price = 999.00
elsif model == 'Apple MacBook Pro with Retina Display 13.3'
  price = 1169.00
elsif model == 'Apple MacBook Pro with Retina Display 15'
  price = 1849.00
else
  price = "Could not find model"
end
{% endhighlight %}

<p>
  In these situations, ruby offers a more elegant solution using <strong>case statements</strong> to do the same thing. It turns out that you can use case statements to declare variables like this:
</p>

{% highlight ruby %}
price = case model
        when 'Apple MacBook Air 13.3' then 999.00
        when 'Apple MacBook Pro with Retina Display 13.3' then 1169.00
        when 'Apple MacBook Pro with Retina Display 15' then 1849.00
        else "Could not find model"
        end
{% endhighlight %}

<p>
  This is a quick little trick that you could use in this common situation.
</p>

<p>
  Happy coding! :)
</p>