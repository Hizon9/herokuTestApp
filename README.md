herokuTestApp
=============

The link on the blog where I posted the text below on <a href="http://nikola-breznjak.com/blog/nodejs/getting-started-with-node-js-on-heroku-on-a-windows-machine/">Getting Started with Node.js on Heroku on a Windows machine</a>

<blockquote><a href="https://www.heroku.com">Heroku</a> is a cloud platform as a service supporting several programming languages which lets app developers spend their time on their application code, not managing servers, deployment, ongoing operations, or scaling...</blockquote>
If you tried to get your feet wet with Heroku by deploying Node.js application, you must have come across <a href="https://devcenter.heroku.com/articles/getting-started-with-nodejs">this article</a> on Heroku's documentation (and you also may have run into problems when starting <a href="http://ddollar.github.io/foreman/">Foreman</a> if you're on a Windows machine - read onward to see how I've solved it).

Obviously, first you have to make an <a href="https://id.heroku.com/signup/devcenter">account on Heroku</a>, then install <a href="https://toolbelt.heroku.com/">Heroku toolbelt </a>(it gives you git, foreman, and heroku command line interface) for your environment (in my case Windows):<br>
<a style="line-height: 1.5;" href="http://nikola-breznjak.com/blog/wp-content/uploads/2014/03/herokuToolbelt.png"><img class="alignnone size-medium wp-image-258" alt="herokuToolbelt" src="http://nikola-breznjak.com/blog/wp-content/uploads/2014/03/herokuToolbelt-300x176.png" width="300" height="176" /></a>

Fire up your command prompt (I use <a href="http://sourceforge.net/projects/console/">Console2</a>) and run <span class="lang:default decode:true  crayon-inline ">heroku login</span> . You need to have <a href="http://nodejs.org/">Node.js</a> installed (in case you don't you can download it <a href="http://nodejs.org/download/">here</a>).

Write some Node.js app and put it in <span class="lang:default decode:true  crayon-inline">app.js</span> file:
<pre class="lang:default decode:true">//app.js
var express = require("express");
var app = express();

app.get('/', function(req, res) {
  res.send('Every day in every way I\'m serving more requests');
});

var port = Number(process.env.PORT || 5000);
app.listen(port, function() {
  console.log("Listening on " + port);
});</pre>
If you have  <span class="lang:default decode:true  crayon-inline">package.json</span>  file then Heroku will recognize your app as Node.js app. In order to create it, run <span class="lang:default decode:true  crayon-inline ">npm init</span>  in the root directory of your app. The npm init utility will walk you through creating a package.json file by asking few questions. If you already made your repository on GitHub and cloned it locally then npm init command in this folder will recognize that too and add:
<pre class="lang:default decode:true">"repository": {
    "type": "git",
    "url": "https://github.com/yourUsername/appName.git"
}</pre>
If you wish, you can freely clone my test app from Github: <a href="https://github.com/Hitman666/herokuTestApp">https://github.com/Hitman666/herokuTestApp.git</a>

Next, install dependencies from your code (in <span class="lang:default decode:true  crayon-inline">app.js</span>  example the required module is <a href="http://expressjs.com/">express</a>). Use <span class="lang:default decode:true  crayon-inline ">npm install &lt;pkg&gt; --save</span>  to install a package and save it as a dependency in the package.json file. In my example that would be one command:
<pre class="lang:default decode:true">npm install express --save</pre>
&nbsp;

You have to set a <a href="https://devcenter.heroku.com/articles/procfile">Procfile</a>, which is a text file in the root directory of your application, to explicitly declare what command should be executed to start a web dyno. In our case this would be the contents of Procfile:
<pre class="lang:default decode:true">web: node app.js</pre>
Now you should be able to start your application locally using Foreman (installed as part of the Heroku Toolbelt) by running <span class="lang:default decode:true  crayon-inline ">foreman start</span> .
<p style="text-align: center;"><span style="line-height: 1.5;"><strong>But here the party started</strong> :/</span></p>
<span style="line-height: 1.5;">In my case foreman didn't start at all, and after a lot of searching, I managed to solve it in few steps. First, I updated my Ruby installation:
</span>
<pre class="lang:default decode:true">gem update --system --source http://rubygems.org/</pre>
Then, according to <a href="http://stackoverflow.com/questions/15399637/cant-start-foreman-in-heroku-tutorial-using-python">this StackOverflow post</a> I installed former version of foreman:
<pre class="lang:default decode:true">gem uninstall foreman
gem install foreman -v 0.61</pre>
Finally, I <span style="line-height: 1.5;">added Ruby's bin folder (Ruby which, in my case, is in <span class="lang:default decode:true  crayon-inline">C:\Program Files (x86)\Heroku\ruby-1.9.2\bin</span>) to my PATH variable.</span>

Ok, almost there! Lets add our app to git (you should skip the git init command if you already cloned your app from GitHub):
<pre class="lang:default decode:true">git init
git add .
git commit -m "init"</pre>
and finally, lets deploy it to Heroku:
<pre class="lang:default decode:true">heroku create
git push heroku master</pre>
To open the app in your browser run <span class="lang:default decode:true  crayon-inline ">heroku open</span> . To push the changes to GitHub you have to execute <span class="lang:default decode:true  crayon-inline  crayon-selected">git push origin master</span>.

<span style="font-family: 'Source Sans Pro', Helvetica, sans-serif; font-size: 16px; line-height: 1.5;"><strong>*</strong>Now that your app is up and running on heroku (<a href="http://gameria.herokuapp.com/">my link</a>) you may want to prevent it from going to sleep mode. Some suggestions on how to achieve this are in <a href="http://stackoverflow.com/questions/5480337/easy-way-to-prevent-heroku-idling">this post on StackOverflow</a> (I went with <a href="http://uptimerobot.com/">UptimeRobot</a>).
</span>

<strong>**</strong>At some point, as I was fiddling with the apps on Heroku's website, I deleted all apps and then the <span class="lang:default decode:true  crayon-inline">git push heroku master</span>  command from my console was not working anymore, so I had to do:
<pre>git remote rm heroku
heroku create
git push heroku master</pre>
<strong>***</strong>Also, I wanted to rename my app and that can be done easily:
<pre class="lang:default decode:true">heroku apps:rename NEWNAME</pre>