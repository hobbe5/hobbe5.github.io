---
title: The namespace is not defined
category: article
featured: images/namespace_is_not_defined.jpg
layout: post
---

<p>Learning a new language always comes with adventure, especially when learning the simple nuances of a new language. This is definitely the case when transitioning from an object oriented language to a functional one. There are going to be plenty of head scratching moments. For me, one of them was learning how the dependencies in F# were affected by the order of the files.</p>
<!--more-->
<br />
<p>When first creating a simple console project, you must have your dependencies flow downwards. Meaning if person.fs file contains the definition for the Person Type then any files that reference this type, say Account.fs or User.fs, must be listed below the Person file. The same goes for multiple projects in the same solution. Your data access layer must be introduced before your logic layer and presentation will follow after that.</p>
<p><img src="asset/images/f-menu-move-up-down.png" />One of the errors you will see when your dependencies have not been ordered correctly is 'The namespace or module 'Name' is not defined'. Queue the scratching of the head. As a developer, I have the habit of copying a file that contains some similar code structure I plan to use and renaming the file. In F#, this leads to misleading errors as I thought my dependencies were ordered correctly from a simple drag and drop. In order to fix the problem, you have to right-click and manually move the file up/down to establish the correct dependency order.</p>