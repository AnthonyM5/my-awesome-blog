---
layout: post
title:  "Welcome to Jekyll!"
date:   2021-02-14 16:19:13 -0500
categories: jekyll update
---

Jekyll is a static site generator written by GitHub's co-founder Tom Preston-Werner.  Jekyll is designed to be simple - no database set ups and generates static webpages with HTML & CSS.  Jekyll is also blog-aware, so it has support for blog features like permalinks, categories, pages, posts, layouts, etc to make importing/migrating existing blogs simple.

# For our purposes we will be using Jekyll to create a blog:

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. 

# Front Matter
Next up is to add the necessary [Front Matter](https://jekyllrb.com/docs/front-matter/): 
The front matter YAML block in your file helps Jekyll process and build your pages using either predefined variables (you can also create your own custom variables). 

{% highlight YAML %}
---
layout: post
title: Welcome To Jekyll!
---
{% endhighlight %}








Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[Link to First Post]({% post_url 2021-02-15-first-post %})



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
