---
layout: post
title: Use A Robot to Reply in Your WeChat Official Account
tags: [wechat, robot chatter]
image:
  feature: cleverbot.jpg
---

Do you want to let a chat robot automatically, and emotionally reply to the users who send messages to your WeChat Official Account?

Generally, the chatting robot I put into the WeChat Official Account works fine. But there are times that it would crash. But, hey, it is fun, right? Hearing from the friends' feedbacks, it could support most of the languages, and do not try to input codes to shut it down, it does not work that way!
<center>
<figure>
	<img src="/images/chat robot/1.jpg" alt="">
</figure>
</center>
<center>
<figure>
	<img src="/images/chat robot/2.jpg" alt="">
</figure>
</center>

Several weeks ago, I found an interesting chat robot while watching live streams of [a video blogger](http://live.bilibili.com/1011) on Bilibili and was wondering if I could use the robot to do something for me. I googled the robot and found its [offical website](https://www.cleverbot.com/). It is said to be the best chatting robot in the world right now, and through the tryouts, I found it very impressive. I would very much like to show the robot to everyone, but in a attractive way. As a result, I decided to try putting the robot into the WeChat Official Account, in short, WCOA afterwards.

There are several parts to be completed in this task. First, and easy to be thought of, the automatic replies of WCOA should be replaced by the messages provided by the chatting robot, in which I should get the API of the robot and find where the replied messages are. Second, always-running service means that a server should be set up to process the messages received anytime. The last I am concerned of would be introduced afterwards.

I would not mention how to create a WCOA, just go to [the website](https://mp.weixin.qq.com/) and sign up, but remember to open the develop mode first. In order to become a developer, you need to enter a URL and a Token. The URL would be server address and the token would be used for verification. The general setting procedure can be found [here](https://mp.weixin.qq.com/wiki), but these documents are a little bit vague. However, you should find and download the sample codes on that website, but I do not remember where to download, so I just give it [here](http://allenshieh.github.io/resources/wx_sample.php) for reference (it is NOT the final version).

Just pause here for the WCOA, and we are gonna first set up the server. Basically, you could use any existing servers on the Internet, but I would only introduce the [Sina App Engine](http://sae.sina.com.cn/) (SAE). I bet the signing up would not be a difficulty for you guys, so I would just skip it. Once you finished signing up, sign in and you would see the dashboard like this.
<center>
<figure>
	<img src="/images/chat robot/dashboard.png" alt="">
</figure>
</center>

Click this to create your own app.
<center>
<figure>
	<img src="/images/chat robot/create.png" alt="">
</figure>
</center>

We would use PHP for accessing the chatting robot API, so be sure not to choose wrong. Just enter the rest of the blanks and note that different running environment options would cost differently. After creating, click the app you just created and open the column on the left as shown in the picture below. Click the code management.
<center>
<figure>
	<img src="/images/chat robot/management.png" alt="">
</figure>
</center>

Then we again stop here, and just in case you wanna know, we would just modify the codes we download before, upload it, and fill in the correct URL and Token, we are done. Open the file 'wx_sample.php' and you can actually find the Token you need.

~~~ php
define("TOKEN", "weixin");
~~~

We turn to the chatting robot website to get the API. Click the API icon on the website.
<center>
<figure>
	<img src="/images/chat robot/api.png" alt="">
</figure>
</center>

Then again you need to sign up. Once you sign in the system, you could find your API key on the dashboard. You could follow the [link](https://www.cleverbot.com/api/howto/) to set up the access to the API. The first 5000 times of calling is free, and that's enough for most of the personal WCOA. Just remember the API key on your dashboard. We are gonna need it.

Back to the codes. Since we would enter the URL and Token ourselves, so we would not need to do the valid step, but we still call the message response function.

~~~ php
$wechatObj = new wechatCallbackapiTest();
//$wechatObj->valid();
$wechatObj->responseMsg();
~~~

While in the message response function, we can easily find what the function would do if the message sent from the user is not empty. The message from the user is analyzed by the following codes.

~~~ php
libxml_disable_entity_loader(true);
              	$postObj = simplexml_load_string($postStr, 'SimpleXMLElement', LIBXML_NOCDATA);
                $fromUsername = $postObj->FromUserName;
                $toUsername = $postObj->ToUserName;
                $keyword = trim($postObj->Content);
                $time = time();
                $textTpl = "<xml>
							<ToUserName><![CDATA[%s]]></ToUserName>
							<FromUserName><![CDATA[%s]]></FromUserName>
							<CreateTime>%s</CreateTime>
							<MsgType><![CDATA[%s]]></MsgType>
							<Content><![CDATA[%s]]></Content>
							<FuncFlag>0</FuncFlag>
							</xml>";  
~~~

The value of '$postObj' is the message content object. We would use the following code to extract the content of it.

~~~ php
$input = rawurlencode ($postObj->Content);
~~~

Then, you should tell the function to link to the robot website and enter your API key.

~~~ php
$url = "https://www.cleverbot.com/getreply";
$key = "your api key";
~~~

Use the following code to get the response from the robot.

~~~ php
$apidata = json_decode (file_get_contents ("$url?input=$input&key=$key"));
~~~

What we get from the website is a json object which has many elements. We only care about some of them, and in my case, I focus on the 'output' and 'cs' content. But generally, the result returned by the website is like below.

~~~ php
{
  "cs":"76nxdxIJO2...AAA",
  "interaction_count":"1",
  "input":"",
  "output":"Good afternoon.",
  "conversation_id":"AYAR1E3ITW",
  ...
}
~~~

We can access the key value 'output' by this.

~~~ php
$contentStr = $apidata->output;
~~~

And the 'contentStr' would be the value returned to the user. Till now, we haven't introduced why we would care about 'cs'. In the chatting scenarios, people usually remember what have been said in their brains. In this way, we could remember the chatting status, but the robot cannot do such implication. Thus, the Cleverbot gives a solution which is using 'cs' to record the current status or the client status. Knowing the status, the robot can know how the chatting goes. The 'cs' value is unique, but how it can reveal the current status is not introduced, in which I am really interested.

Having known such mechanism, we could save the 'cs' for every user of our WCOA. In order to save the file on SAE, we would use the public saving space on SAE, which is provided for special occasions (normally, SAE would not recommend saving permanent files on the server). In order to do that, try the codes below.

~~~ php
$user_cs = "saekv://filename";
$cs = file_get_contents( $user_cs );
...
file_put_contents( $user_cs, $cs );
~~~

Once you manage the file saving, we are done with the codes. But remember, by just adding the codes I list above, you won't be able to run it correctly. You need to figure them out yourself.

Now, you could ZIP you file and upload to the app you create on SAE. Copy your app link and paste them with '/wx_sample.php' or something else to the URL blank on the WCOA page. Fill in the Token. It should be like this.
<center>
<figure>
	<img src="/images/chat robot/url.png" alt="">
</figure>
</center>

As for the EncodingAESKey, just fill it in in the way you like. Hit the confirm button and you become one of the WeChat developer.

Till now, I have probably introduced all the steps you would need to know to add the chatting robot to you WCOA. If you encounter any problem, leave a comment (you may need to a VPN to access the comment system provided by DISCUS). The robot would charge if you run out of all your 5000 calls to the function and the free SAE service can only last one months. So make your own decision on it. I have paid SAE. LOL

See ya. Peace.
