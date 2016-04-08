---
title: Dynamic Stubs for Testing
featured: images/wbuffet.jpg
layout: post
excerpt: <p>As a developer, I'm a believer in test-driven devleopment, TDD. When setting up a unit test project one of the redundant tasks I find myself performing is setting up initial test data, so I always import the same two classes for my test projects.</p>
---

<p>As a developer, I'm a believer in test-driven devleopment, TDD. When setting up a unit test project one of the redundant tasks I find myself performing is setting up initial test data, so I always import the same two classes for my test projects.</p>
<!--more-->
<p>The first is a static class called Rand and it's single responsibility is to provide the caller with random data:</p>
{% highlight csharp %}
public static class Rand
{
    public static int GetInt()
    {
        return GetInt(Constants.RANDOM_STUB_MAX);
    }
    public static int GetInt(int max)
    {
        return new Random().Next(max);
    }
    public static int GetInt(int min, int max)
    {
        return new Random().Next(min, max);
    }
    public static string GetString(int length)
    {
        return new Guid().ToString().Substring(0, length);
    }
}
{% endhighlight %}
<p>The second is an abstract class which makes use of generics for my test classes to inherit:</p>
<pre class="prettyprint">
internal abstract class BaseFake&lt;T&gt;
{
    public abstract T GetRandom();

    public IEnumerable&lt;T&gt; GetRandomList(int count)
    {
        var list = new List&lt;T&gt;();
        for (int i = 0; i < count; i++)
            list.Add(GetRandom());
        return list;
    }
}
</pre>
<p>With the help of these two classes I can provide fake data to my unit tests quickly and cleanly. For instance if I had a class that represents an address then that fake would look like such:</p>
<pre class="prettyprint">
internal class AddressFake : BaseFake&lt;Address&gt;
{
    public override Address GetRandom()
    {
        return new Address
        {
            Line1 = Rand.GetString(8),
            Line2 = Rand.GetString(8),
            City = Rand.GetString(6),
            Region = Rand.GetString(10),
            PostalCode = Rand.GetInt(10000, 99999).ToString()
        };
    }
}
</pre>
<p>All my fake data is contained to one area and I can manipulate my fakes whichever way I want depending on the behavior I want to test. Keep testing.</p>