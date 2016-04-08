---
title: Dynamic Stubs for Testing
featured: images/wbuffet.jpg
layout: post
---

<p>As a developer, I'm a believer in test-driven devleopment, TDD. When setting up a unit test project one of the redundant tasks I find myself performing is setting up initial test data, so I always copy the same two classes for my tests.</p>
<p>The first is a static class called Rand and it's single responsibility is to provide the caller with random data:</p>
<pre>
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
</pre>
