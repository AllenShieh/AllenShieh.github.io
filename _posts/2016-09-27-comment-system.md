What a day! 

I planned to read several papers this afternoon, but once heard from Patrick that the github student pack can provide a free domain name for one year, I could wait to try it. However, as soon as I set up my redirection, the comment system I previously build was down. I tried another one, but with the same failure. What is really interesting was that using Safiri, everything was fine, but testing on my own computer, the comment system was reluctant to come out. Well, in the end, I switch back to the previous system and give up the free domain name.

Recently, I have been working on my blog for its improvements. Here, I would introduce two comment systems that can be added into your blog, if you are interested in having one. A blog would not be a real blog if it has no comment system, right?

OK, here we go！

## Facebook Comment Plugin
I personally would not recommend this comment system due to that it can only use facebook account to comment. However, having a facebook comment system in your blog is much more awesome, maybe, at least to some of us. Seriously, through my own experience, this comment system is not as good as the second one.

Fine, cut the useless talk. Let us begin.

### First, get a facebook developer identity.
Visit the facebook developer [website](https://developers.facebook.com/) and log in. Look to your top right and find the button shown in the figure below. Click it and click 'Add a New App'. Then choose 'Website' and name your app. You will find you have already got a segment of codes. Put them in the file which will be included in any page file that you want to show the system, traditionally in the <body> tag, or in the 'head.html' as I do:

``` html
<script>
  window.fbAsyncInit = function() {
    FB.init({
      appId      : '100610900406122',
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
