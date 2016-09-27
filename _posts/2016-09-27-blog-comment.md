---
layout: post
title: Guidance
description: "Picture test."
modified: 2016-09-27
tags: [jekyll] [blog] [comment system]
---

What a day!

I planned to read several papers this afternoon, but once heard from [Patrick](http://patrick-peng.me/) that the github student pack can provide a free domain name for one year, I could wait to try it. However, as soon as I set up my redirection, the comment system I previously build was down. I tried another one, but with the same failure. What is really interesting was that using Safiri, everything was fine, but testing on my own computer, the comment system was reluctant to come out. Well, in the end, I switch back to the previous system and give up the free domain name.

Recently, I have been working on my blog for its improvements. Here, I would introduce two comment systems that can be added into your blog, if you are interested in having one. A blog would not be a real blog if it has no comment system, right?

OK, here we go！

## Facebook Comment Plugin
I personally would not recommend this comment system due to that it can only use facebook account to comment. However, having a facebook comment system in your blog is much more awesome, maybe, at least to some of us. Seriously, through my own experience, this comment system is not as good as the second one.

Fine, cut the useless talk. Let us begin.

First, visit the [Facebook developer website](https://developers.facebook.com/) and log in. Look to your top right and find the button shown in the figure below.

<center>
<figure>
	<img src="/images/comment system/my apps.png" alt="">
</figure>
</center>

Click it and click 'Add a New App'. Then choose 'Website' and name your app. You will find you have already got a segment of codes. Put them in the file which will be included in any page file that you want to show the system, traditionally in the <body> tag, or in the 'head.html' as I do. What you get is something like this:

``` html
<script>
  window.fbAsyncInit = function() {
    FB.init({
      appId      : 'your app id',
      xfbml      : true,
      version    : 'v2.7'
    });
  };

  (function(d, s, id){
     var js, fjs = d.getElementsByTagName(s)[0];
     if (d.getElementById(id)) {return;}
     js = d.createElement(s); js.id = id;
     js.src = "//connect.facebook.net/en_US/sdk.js";
     fjs.parentNode.insertBefore(js, fjs);
   }(document, 'script', 'facebook-jssdk'));
</script>
```

Then type in your website link and click 'next'. Scroll down to 'Next Steps' and click 'Social Plugins' which opens a new page. Scroll down a little to find this:

<center>
<figure>
	<img src="/images/comment system/comments.png" alt="">
</figure>
</center>

Click 'Read the Docs' and you should see the Comments Plugin's document. You do not have to configure any of the blanks shown, just click 'Get Code' and two segments of codes will appear. Copy the first segment in your <body> tag like this:

``` html
<body id="post" {% if page.image.feature %}class="feature"{% endif %}>

<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = "//connect.facebook.net/en_US/sdk.js#xfbml=1&version=v2.7&appId=your app id";
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>

<!-- contents below -->
```

And copy the second segment to the place you want the system to show. Now you can read the introduction to the attributes of the system and configure them, but you can also add regardless of them like me, where all the values for the configuration will be default:

``` html
<center><div class="fb-comments"></div></center>
```

And you are done! I have given you all the facts that you need to know about how to insert facebook comment system into your blog. Push and try it!

## Disqus
This one will be easier than the previous one and it has more ways for the commentor to log in and comment on your blog, through Facebook, Twitter, and Google. The appearance will be more satisfying, to be honest.

Visit the [official website of Disqus](https://disqus.com/) and get an account. Click 'GET STARTED':

<center>
<figure>
	<img src="/images/comment system/get started.png" alt="">
</figure>
</center>

Choose 'I want to install Disqus on my site'. Then type in the information and click 'Create Site'. Afterwards, you should get something like this:

``` html
    <div id="disqus_thread"></div>
    <script>

    /**
     *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
     *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables */
    /*
    var disqus_config = function () {
        this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() { // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');
        s.src = '//shortname.disqus.com/embed.js';
        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
    </script>
    <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
```

Copy them to where the comment system would show and, you are done here! Push and drink a cup of coffee.

It will be like this:

<center>
<figure>
	<img src="/images/comment system/disqus.png" alt="">
</figure>
</center>

Well, the second part is a little short, mainly because Disqus is much more easier compared to Facebook Comment Plugin. Pfff. I spent the whole afternoon fighting against why the change of the domain name would cause the comment system to fail and I had not found the answer. I guess the problem result from the redirection. When setting the url in the comment system, it is a little bit tricky between the real url and the redirected url.

At least, the comment system works. Cheers.
