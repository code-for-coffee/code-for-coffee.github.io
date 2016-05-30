---
layout: post
title: Binding parameters to Web API controller with Backbone.js
---

This is a really quick write-up of an issue that isn't well documented. One of our work related projects uses Backbone.js on the front end to handle models and collections and is powered by ASP.NET Web API 2.0. I ran into an issue regarding using the built in model functions <strong>.save()</strong> and <strong>.fetch()</strong>.

I found that to pass parameters in (either from a model or somewhere else) when making a <strong>GET</strong> request (such as the <strong>model.fetch()</strong> method) to the controller, you needed to declare the model/parameter with the <strong>[FromUri]</strong> attribute. Example:
<pre>public testSampleModel Get([FromUri]testSampleModel  model) { ... }</pre>
How to send in a parameter on a <strong>.fetch()</strong>? Good question - here's the answer (using jQuery):
<pre>myModel.fetch({ data: $.param({ paramName: "foobar"}) });</pre>
To pass parameters/model when making a POST request (such as model.save()), I found that you must declare the model/parameter as either [FromBody] or without an attribute at all. Example:
<pre>public testSampleModel Get([FromBody]testSampleModel  model) { ... }</pre>
That's it - just a quick, informative post.