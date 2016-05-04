---
title: Sheriff Sale
category: project
featured: images/sheriffsale.jpg
layout: post
---

<p>With the use of angularjs and consuming public data APIs using javascript, I built a hybird web/mobile applcation for use by buyers participating in sheriff sales.</p>
<!--more-->
<p>The city of Philadelphia provides an api supplying a list of houses available in a sheriff sale each month. We wanted to provide a way to display this data easily on a mobile phone. Here is a quick peek of the angular repeater:</p>
<pre class="prettyprint"><code>
	&lt;div ng-repeat=&quot;sale in pagedItems[currentPage]&quot;&gt;
        &lt;div class=&quot;box&quot;&gt;
            &lt;div class=&quot;boxInner&quot; id=&quot;{{sale.PropertyID}}&quot;&gt;
                &lt;a href=&quot;#/detail?addr={{sale.name}}&quot; ng-click=&quot;scrollToundefinedsale.PropertyID)&quot;&gt;
                    &lt;img ng-src=&quot;https://maps.googleapis.com/maps/api/streetview?size=400x400&amp;location={{sale.name}}&amp;fov=60&amp;pitch=0&amp;key=#########&quot;/&gt;
                &lt;/a&gt;
                &lt;div class=&quot;titleBox&quot;&gt;
                    {{sale.BooknWrit}}&lt;br/&gt;
                    {{sale.name}}
                &lt;/div&gt;
            &lt;/div&gt;
        &lt;/div&gt;
    &lt;/div&gt;
</code></pre>
<p><img src="assets/images/sheriffsale1.jpg" align="right" valign="top" vspace="5" hspace="5"/>Each picture on the main page represents a residence for sale and on click shows more detail as seen in the picture above. Pagination is useful instead of infinite scrolling in this case so in the view we add this snippet of code:</p>
<pre class="prettyprint"><code>
	&lt;div class=&quot;pagination pull-right&quot;&gt;
        &lt;ul&gt;
            &lt;li ng-class=&quot;{disabled: currentPage == 0}&quot;&gt;
                &lt;a href ng-click=&quot;prevPageundefined)&quot;&gt;« Prev&lt;/a&gt;
            &lt;/li&gt;
            &lt;li ng-repeat=&quot;n in rangeundefinedpagedItems.length)&quot;
                ng-class=&quot;{active: n == currentPage}&quot;
                ng-click=&quot;setPageundefined)&quot;&gt;
                &lt;a href ng-bind=&quot;n + 1&quot;&gt;1&lt;/a&gt;
            &lt;/li&gt;
            &lt;li ng-class=&quot;{disabled: currentPage == pagedItems.length - 1}&quot;&gt;
                &lt;a href ng-click=&quot;nextPageundefined)&quot;&gt;Next »&lt;/a&gt;
            &lt;/li&gt;
        &lt;/ul&gt;
    &lt;/div&gt;
</code></pre>
<p>One thing I learned about angular is about keeping state, in this case we utilized a factory as you can see here:</p>
<pre class="prettyprint">
	app.factory('State', ['$http', function ($http) {
	    var state = {
	        currentPage:0
	    };
	    return state;
	}]);
</pre>
<p>And in the controller we can react to the change in this state value like such:</p>
<pre class="prettyprint">
    $scope.prevPage = function () {
        if (State.currentPage > 0) {
            State.currentPage--;
            $scope.currentPage = State.currentPage;
            $location.hash('');
            $anchorScroll();
        }
    };

    $scope.nextPage = function () {
        if (State.currentPage < $scope._sales.length - 1) {
            State.currentPage++;
            $scope.currentPage = State.currentPage;
            $location.hash('');
            $anchorScroll();
        }
    };

    $scope.setPage = function () {
        State.currentPage = this.n;
        $scope.currentPage = State.currentPage;
        $location.hash('');
        $anchorScroll();
    };

    $scope.scrollTo = function (id) {
        $location.hash(id);
        $anchorScroll();
    }
</pre>
<p>As you can see anchorScroll resets the page to top when necessary. We use the anchorScroll for retaining the scroll position of the last page visited as users visit the information of each property.</p>