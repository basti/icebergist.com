---
date: 2008-07-07 09:00:00 +0100
title: 'Collection partial variable naming &#8211; new in edge Rails'
author: Slobodan Kovačević
layouts: posts
url: "/posts/collection-partial-variable-naming/"
comments: true
categories:
  - Ruby and Rails
tags:
  - edge
  - rails
published: true
---
One thing that always annoyed me when rendering collection partials is that a local var in partial is named same as a partial template name. So, to go around this I always created another local variable in partial and gave it a more meaningful name.

No longer&#8230; As the [Ryan says][1] from now on in the Edge Rails you can specify the name of the local variable in which each collection element will be exposed within a partial. You will can do this:

<pre>render :partial =&gt; 'employees', :collection =&gt; @workers, :as =&gt; :person</pre>

It&#8217;s a simple thing, but it&#8217;s just one of the things that bugged me. I am glad it&#8217;s gone now.

[1]: http://ryandaigle.com/articles/2008/7/7/what-s-new-in-edge-rails-collection-partial-variable-naming "What's New in Edge Rails: Collection Partial Variable Naming "
