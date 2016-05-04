---
title: Sheriff Sale
category: project
featured: images/sheriffsale.jpg
layout: post
---

<p>With the use of angularjs and consuming public data APIs using javascript, I built a hybird web/mobile applcation for use by buyers participating in sheriff sales.</p>
<!--more-->
<p>The city of Philadelphia provides an api supplying a list of houses available in a sheriff sale each month. We wanted to provide a way to display this data easily on a mobile phone. Here is a quick peek of the angular repeater:</p>
<pre class="prettyprint">
	<div ng-repeat="sale in pagedItems[currentPage]">
        <div class="box">
            <div class="boxInner" id="{{sale.PropertyID}}">
                <a href="#/detail?addr={{sale.name}}" ng-click="scrollTo(sale.PropertyID)">
                    <img ng-src="https://maps.googleapis.com/maps/api/streetview?size=400x400&location={{sale.name}}&fov=60&pitch=0&key=#########"/>
                </a>
                <div class="titleBox">
                    {{sale.BooknWrit}}<br/>
                    {{sale.name}}
                </div>
            </div>
        </div>
    </div>
</pre>
<p><img src="images/sheriffsale1.jpg" align="right" valign="top" vspace="5" hspace="5"/>Each picture on the main page represents a residence for sale and on click shows more detail as seen in the picture above. Pagination is useful instead of infinite scrolling in this case so in the view we add this snippet of code:</p>
<pre class="prettyprint">
	<div class="pagination pull-right">
        <ul>
            <li ng-class="{disabled: currentPage == 0}">
                <a href ng-click="prevPage()">« Prev</a>
            </li>
            <li ng-repeat="n in range(pagedItems.length)"
                ng-class="{active: n == currentPage}"
                ng-click="setPage()">
                <a href ng-bind="n + 1">1</a>
            </li>
            <li ng-class="{disabled: currentPage == pagedItems.length - 1}">
                <a href ng-click="nextPage()">Next »</a>
            </li>
        </ul>
    </div>
</pre>
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