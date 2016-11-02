---
layout: post
title: Setting up AngularJS, Jasmine, and Karma in Visual Studio
---

<h2>Introduction</h2>
I've recently been tasked to research <strong>AngularJS</strong> to use on top of Visual Studio. After reading the official tutorial, unofficial tutorials, and Manning's early access <strong>AngularJS in Action </strong>and <strong>Getting MEAN </strong>(which stands for mongoDB/Express/AngularJS/Node.js), I found that there are too few guides for Windows users. If you're like me, you probably are running into issues installing and running <strong>karma</strong>, would like to run Javascript tests in the convenience of the Test Explorer (or Resharper), and want to let NuGet handle all of your package updating (bower is great, but not for .NET environments). Well, this tutorial is here to help! We'll get you up and running with AngularJS, Jasmine, and Karma.

All of this has been setup/tested on:
<ul>
	<li>Windows 8.1 x64 Pro</li>
	<li>Visual Studio 2013 Professional</li>
	<li>Google Chrome</li>
</ul>
<h2>Getting Started</h2>
Go ahead and install the following software:
<ul>
	<li><a href="http://nodejs.org/">Node.js</a> (Windows installer) - <em>install everything!</em></li>
	<li><a href="http://visualstudiogallery.msdn.microsoft.com/f8741f04-bae4-4900-81c7-7c9bfb9ed1fe">Chutzpah Test Adapter for Visual Studio</a></li>
	<li><a href="http://visualstudiogallery.msdn.microsoft.com/71a4e9bd-f660-448f-bd92-f5a65d39b7f0">Chutzpah Test Runner Context Menu for Visual Studio</a></li>
</ul>
Once all of that is setup it is time to set up our project environment. Go ahead and create a new Visual Studio project - you can choose any project you wish; this tutorial will use ASP.NET MVC 5 (by selecting ASP.NET Web Application). <strong>Select MVC</strong>  and <strong>Add unit tests</strong>. We're going to use a traditional view to power our SPA (though, you could easily create a Web API project instead - just make sure that you set your routes up to point to an index.html page instead of an ASPX/Razor view).

<a href="http://codeforcoffee.org/wp-content/uploads/2014/04/angular.png"><img class="alignnone wp-image-162 size-medium" src="http://codeforcoffee.org/wp-content/uploads/2014/04/angular-300x211.png" alt="Visual Studio project screen" width="300" height="211" /></a>

Now, we need to grab a few Javascript libraries from (hint: if you prefer, you can grab these from NuGet): <a href="https://angularjs.org/">AngularJS</a> (I recommend grabbing the entire ZIP that includes all libraries) and <a href="https://github.com/pivotal/jasmine">Jasmine</a> (you can use another testing framework if you prefer but the official AngularJS examples/tutorials use Jasmine).

Okay - you have all of the tools either installed (or about to be).
<h2>Setting up the project folder</h2>
You should have two projects: Your ASP.NET MVC/Web API project and your Tests project. Inside of the ASP.NET project, we're going to add the following folders: <strong>ng-views</strong> and <strong>ng-scripts</strong>.

The ng-views folder will contain all of your Angular-specific views. The ng-scripts folder will contain your controllers, models, directives, and so forth. In the <strong>Tests</strong> project, create an <strong>ng-tests</strong> project. <em>If you installed Angular and Jasmine via NuGet you should be able to skip to the next paragraph. </em>Now, extract your AngularJS archive and copy <strong>all </strong>of the scripts to the ~/<strong>Scripts/Angular/</strong> folder (you'll need to create the subfolder). Unarchive Jasmine and copy the contents of the <strong>lib</strong> folder to <strong>~/Scripts/Jasmine/</strong>. You'll need to update your <strong>/App_Start/BundleConfig.cs</strong> file - do that now by adding the following lines to the RegisterBundles method:
<pre class="prettyprint prettyprinted" style="color: #222222;"><span class="pln" style="color: #000000;">bundles</span><span class="pun" style="color: #000000;">.</span><span class="typ" style="color: #2b91af;">Add</span><span class="pun" style="color: #000000;">(</span><span class="kwd" style="color: #0000ff;">new</span><span class="typ" style="color: #2b91af;">ScriptBundle</span><span class="pun" style="color: #000000;">(</span><span class="str" style="color: #a31515;">"~/Scripts/Angular"</span><span class="pun" style="color: #000000;">).</span><span class="typ" style="color: #2b91af;">IncludeDirectory</span><span class="pun" style="color: #000000;">(</span><span class="str" style="color: #a31515;">"~/Scripts/Angular","*.js"</span><span class="pun" style="color: #000000;">));
</span></pre>
<pre class="prettyprint prettyprinted" style="color: #222222;"><span class="pln">bundles</span><span class="pun">.</span><span class="typ" style="color: #2b91af;">Add</span><span class="pun">(</span><span class="kwd" style="color: #0000ff;">new</span><span class="typ" style="color: #2b91af;">ScriptBundle</span><span class="pun">(</span><span class="str" style="color: #a31515;">"~/Scripts/Jasmine"</span><span class="pun">).</span><span class="typ" style="color: #2b91af;">IncludeDirectory</span><span class="pun">(</span><span class="str" style="color: #a31515;">"~/Scripts/Jasmine","*.js"</span><span class="pun">));</span></pre>
Go ahead and right click on the ng-tests folder - you'll notice that you can run tests using Chutzpah from the context menu - one will load tests into the Test Explorer and the other option will load a static browser instance version of Jasmine.

Finally, if you love IntelliSense (who doesn't), you'll want to create a <strong>_references.js</strong> script in both your ASP.NET project and your Test project. Create a blank Javascript file in both projects, select <strong>all</strong> of your Javascript files, and drag/drop them into both blank files (or you can also build out the file yourself - up to you). Your _references.js file should look like:
<pre>/// &lt;reference path="modernizr-2.6.2.js" /&gt;
/// &lt;reference path="jquery-1.10.2.js" /&gt;
/// &lt;reference path="bootstrap.js" /&gt;
/// &lt;reference path="respond.js" /&gt;
/// &lt;reference path="jquery.validate.js" /&gt;
/// &lt;reference path="jquery.validate.unobtrusive.js" /&gt;
/// &lt;reference path="../Scripts/angular.js" /&gt;
/// etc..</pre>
Now, with any new Javascript file you create, do the same thing with just the <strong>_references.js</strong> file to first line of every project. Go ahead and create some baseline files for you <strong>ng-scripts</strong> and <strong>ng-tests</strong>; something like the following will do (this will assist in the next step) - if the code you see is foreign to you don't fret, any tutorial (links to some at the end of this post) will explain them later:

In <strong>/TestProject/ng-tests/MainCtrlSpec.js</strong>:
<pre id="LC1" class="line">describe('Controller: MainCtrl', function() {
    beforeEach(module('MyApp'));
    var MainCtrl, scope;
    beforeEach(inject(function($controller) {
        scope = {};
        MainCtrl = $controller('MainCtrl', {
            $scope: scope
        });
    }));
    it('should have scope defined', function () {
        expect(scope).toBeDefined();
    });
});</pre>
<p class="line">In <strong>/ASPNETProject/ng-scripts/MainCtrl.js</strong>:</p>

<pre class="line">var myModule = angular.module('MyApp', []);
myModule.controller('MainCtrl', ['$scope',
 function ($scope) {
 // I'm a lonely controller :(
 }
]);</pre>
<p class="line">Okay, so now we have everything in Visual Studio up ready for AngularJS. Now, on to the fun part.</p>

<h2>Installing Karma on Windows</h2>
Now, we need to install Karma (which is used for end-to-end/integration testing). To do this, open up the DOS prompt (as an adminstrator). Browse to the base of your project directory (where both of your projects are located). Now, run the following commands:
<pre>npm install -g karma
npm install -g karma-cli</pre>
This will install Karma to your global PATH (so you can access it anywhere on the OS - such as in other projects and so forth). Now, you need to configure Karma for your project. To do so, just type in:
<pre>karma init</pre>
You'll now be prompted to select your testing framework (Jasmine is the default), if you'd like to install Require.js (no is the default), which browser to run the tests in (Chrome is the default), the location of your scripts (see the example below - you'll want to point them to the <strong>/ng-scripts/</strong> and <strong>/ng-tests/</strong> folder while selecting all Javascript files (<strong>**.js</strong>), the location/file types to exclude (empty by default), and if you'd like Karma to actively watch for changes to your files (yes!) so it can re-run them as needed. Your command line output should look like:
<pre>Which testing framework do you want to use ?
Press tab to list possible options. Enter to move to the next question.
&gt; <strong>jasmine</strong>
Do you want to use Require.js ?
This will add Require.js plugin.
Press tab to list possible options. Enter to move to the next question.
&gt; <strong>no</strong>
Do you want to capture any browsers automatically ?
Press tab to list possible options. Enter empty string to move to the next question.
&gt; <strong>Chrome</strong>
&gt;
What is the location of your source and test files ?
You can use glob patterns, eg. "js/*.js" or "test/**/*Spec.js".
Enter empty string to move to the next question.
&gt; <strong>AngularJumpstart/ng-scripts/**.js</strong>
&gt; <strong>AngularJumpstart.Tests/ng-tests/**.js</strong>
Should any of the files included by the previous patterns be excluded ?
You can use glob patterns, eg. "**/*.swp".
Enter empty string to move to the next question.
&gt;
Do you want Karma to watch all the files and run the tests on change ?
Press tab to list possible options.
&gt; <strong>yes</strong>
Config file generated at "../path/to/your/project/karma.conf.js".</pre>
<em><strong>Note</strong>: if you don't have valid Javascript files in the ng-scripts and ng-tests folders, you'll get an error when trying to create the config file. Make sure you have at least an empty js file in each folder.</em>

Now, before moving on we need to make sure that Angular is references in the <strong>karma.conf.js</strong> file. Open it up and make sure that <strong>angular.js</strong> and <strong>angular-mock.js</strong> are included (you may need to add other files as time goes on, too). Look for the <strong>files</strong> array:
<pre id="LC16" class="line">// list of files / patterns to load in the browser
    files: [
      'AngularJumpstart/Scripts/Angular/angular.js',
      'AngularJumpstart/Scripts/Angular/angular-mocks.js',
      'AngularJumpstart/ng-scripts/**.js',
      'AngularJumpstart.Tests/ng-tests/**.js'
    ],</pre>
Once you've saved that, all you need to do is run karma:
<pre>karma start</pre>
You'll see the application initialize and an instance of your browser run. Every time you change any of the files that Karma is monitoring the tests will re-run (and show if they're successful or if they fail). If you'd like to familiarize yourself with how Karma configurations work, please view the <a href="http://karma-runner.github.io/0.8/intro/configuration.html">official documentation</a>.
<h2>Wrap-Up</h2>
Well, now you have everything set up to start your AngularJS project. If you're new to the AngularJS world, here are a few places to get started:
<ul>
	<li><a href="https://docs.angularjs.org/tutorial">Official AngularJS Tutorial</a></li>
	<li><a href="http://www.syncfusion.com/resources/techportal/ebooks/angularjs">AngularJS Succinctly</a> (a great pocket-reference sort of guide)</li>
	<li><a href="http://www.manning.com/bford/">AngularJS in Action</a> (pre-release book)</li>
</ul>
If you see any errors feel free to comment and I'll take care of them. Thanks for reading!

&nbsp;
<pre></pre>
<div id="LC1" class="line"></div>