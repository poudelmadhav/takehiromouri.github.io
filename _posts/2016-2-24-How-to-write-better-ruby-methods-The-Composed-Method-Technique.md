---
layout: post
title: How to Write Better Ruby Methods - The Composed Method Technique
---

<p>
  In my search to write better code, I've stumbled across a technique to write better methods. It's called <strong>The Composed Method Technique</strong> (from <a href="https://read.amazon.com/kp/embed?asin=B004MMEJ36&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_JqsYwb0AAT5XE" target="_blank">Eloquent Ruby</a>).
</p>

<p>
  The composed method technique basically has 3 rules.
</p>

<strong>
  <ol>
    <li>Each method should do a single thing</li>
    <li>Each method needs to operate at a single conceptual level</li>
    <li>Each method needs to have a name that reflects its purpose</li>
  </ol>
</strong>

<p>
  Let's walk through what each of these rules mean.
</p>

<h3>Each method should do a single thing</h3>
<p>
  Each method should focus on solving a single aspect of the problem. So if your method is solving two problems, then you should be creating a new method. For example, consider the following method that determines if one should transition from one relationship to another:
</p>

{% highlight ruby %}
def transition
  return if numeric_love_level(current_partner) < 7
  if life_partner_material?(current_partner)
    if side_chick.exists? && current_partner.knows_about_side_chick? || self.cant_take_anymore_crap
      puts "Sorry, we can't be together anymore"
      current_partner = nil
      if side_chick.exists?
        side_chick.send_message("Wanna netflix and chill?")
      else
        tinder.find_partner
      end
    end
  end
end

# other methods of figuring out relationship problems
{% endhighlight %}

<p>
  This is such messy code (and a horrible algorithm to begin with...) The <code>transition</code> method does so many things: it tries to see if there's still love between the two, it tries to see if the partner is life partner material, it tries to see if a side chick exists and if the partner knows about her, and if so, break up with the partner and seek out other potentials. All in all, this method is a <i>mess</i>. Again, <strong>each method should do a single thing</strong>.
</p>

<p>
  Let's seperate each problem into a method and see what it looks like (still a horrible algorithm):
</p>

{% highlight ruby %}
def transition
  return if love_still_exists?
  return false unless break_up?
  break_up!
  find_new_partner
end

def love_still_exists?
  numeric_love_level(current_partner) < 7 ? false : true
  return true if life_partner_material?(current_partner)
  # other complicated relationship stuff below
end

def break_up?
  side_chick.exists? && current_partner.knows_about_side_chick? ? true : false
  return true if self.cant_take_anymore_crap
  false
  # what a crappy algorithm to figure out if you should break up or not :/
end

def break_up!
  puts "Sorry, we can't be together anymore"
  current_partner = null
end

def find_new_partner
  if side_chick.exists?
    side_chick.send_message("Wanna netflix and chill?")
  else
    tinder.find_partner
  end
end

# other methods of figuring out relationship problems
{% endhighlight %}

<p>
  Although I advise absolutely no one to try out this algorithm in real life, the code itself is much easier to read and understand. Splitting the methods into small little chunks result in better code.
</p>

<h3>Each method nees to operate at a single conceptual level</h3>
<p>
  This basically means not to mix high-level logic with the nitty-gritty details. For example, the <code>transition</code> method holds high-level logic and branches out to other methods:
</p>

{% highlight ruby %}
def transition
  return if love_still_exists?
  return false unless break_up?
  break_up!
  find_new_partner
end
{% endhighlight %}

<p>
  What we don't want is something like this:
</p>

{% highlight ruby %}
def transition
  # high-level logic
  return if love_still_exists?
  return false unless break_up?
  # nitty gritty details here
  puts "Sorry, we can't be together anymore"
  current_partner = null
  # nitty gritty details again
  if side_chick.exists?
    side_chick.send_message("Wanna netflix and chill?")
  else
    tinder.find_partner
  end
end
{% endhighlight %}

<p>
  It's much better organized if the conceptual level is constant as we saw in the above example.
</p>

<h3>Each method needs to have a name that reflects its purpose</h3>
<p>
  This one is quite straightforward. In essense, what we don't want is something like this:
</p>

{% highlight ruby %}
def eat_cheeseburger
  puts "Sorry, we can't be together anymore"
  current_partner = null
end
{% endhighlight %}

<p>
  or this:
</p>

{% highlight ruby %}
def draw_circle
  numeric_love_level(current_partner) < 7 ? false : true
  return true if life_partner_material?(current_partner)
  # other complicated relationship stuff below
end
{% endhighlight %}

<p>
  A method named <code>eat_cheeseburger</code> is expected to eat a cheese burger, not say a cliche break up quote and set <code>current_partner</code> to <code>null</code>. Similarly, a method named <code>draw_circle</code> is expected to draw a circle, not figure out complicated love matters.
</p>

<p>
  In this situation, <code>break_up!</code> or <code>love_still_exists?</code> makes 500% more sense.
</p>

<h3>Writing Shorter Methods</h3>
<p>
  Applying the composed method technique allows you to write shorter and smaller methods instead of long ones. There are several merits to writing shorter methods in Ruby.
</p>

<p>
  The first is that <strong>writing smaller details will help you get the details correct</strong>. It's the same idea to writing pseudocode - except you are defining a new method for each line of pseudocode.
</p>

<p>
  The second is that <strong>it's easier to test with smaller methods</strong>. If a method contains a solution to more than one problem, you can't isolate a buggy solution; you can only test the entire method. If you have methods that are divided into seperate purposes, you can isolate them and test if they work. It's much easier to debug this way.
</p>

<p>
  The last upside is that in Ruby, the composed method way of defining and building classes is super effective since <strong>the cost of defining a new method in Ruby is extremely low</strong>. You just need an additional <code>def</code> and <code>end</code>. In other languages, it can take a little bit more code and thus a little bit more costly. The cost performance of defining a new method in Ruby is high and there's really no reason not to define a new method.
</p>

<p>
  <strong><i>Unless</i></strong> you are over doing the "short methods" thing. Your "short method" should actually do something and you shouldn't just define a short method because you can:
</p>

{% highlight ruby %}
def break_up!
  say_cliche_break_up_line
  set_current_partner_to_null
end

def say_cliche_break_up_line
  puts "Sorry, we can't be together anymore"
end

def set_current_partner_to_null
  current_partner = null
end
# this is over doing it
{% endhighlight %}

<hr>

<p>
  I hope my poorly thoughout algorithm was able to explain the topic of writing better methods in Ruby. I apologize for my lack of relationship intelligence, if you have any suggestions please reach out to me as I would like to refactor my poorly constructed model of how one should transition between relationships.
</p>

<p>
  Happy coding!
</p>


