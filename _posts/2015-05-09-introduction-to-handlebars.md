---
layout: post
title: Introduction to Handlebars
date: 2015-05-09
published: false
---


Today I'm going to talk to you about **Handlebars.js**! This is one of *many* Javascript template frameworks available to work with.

I'm here to talk about it because I find that it is great for use in production, has an easy-to-learn syntax, and doesn't require a lot of work to get up and running with it. Templates are great when you spot a re-use pattern; our modern, dynamic applications require that we render on the fly. It is a common need for a developer to render lists of data, tables from a database, or objects from an API.

#### The thought process behind this

<img src="/images/Template_Creation_Process.png" alt="Templates Creation Process" width='75%'>

> **Fun fact:** One of the reasons that Handlebars is great is that there is a *runtime* version of the library that allows you to use pre-compiled templates. Size comparison: `handlebars.min.js` = 65KB & `handlebars-runtime.min.js` = 10KB

##### 1. Spotting re-use patterns

Let's say for example that a dynamic application you're working on displays a list of items to a user upon logging in. They look something like this:

```html
<section class='items-list'>
	<ul>
		<li class='in-stock'>Apples</li>
		<li class='out-of-stock'>Oranges</li>
		<li class='in-stock'>Grapes</li>
		<li class='in-stock'>Peaches</li>
	</ul>
</section>
```

##### 2. Identify which elements could be repeated

The data comes in from a data structure that looks like this:

```javascript
[
	{ stockStatusClass: 'in-stock', itemName: 'Apples'},
	{ stockStatusClass: 'out-of-stock', itemName: 'Oranges'}
	{ stockStatusClass: 'in-stock', itemName: 'Grapes'},
	{ stockStatusClass: 'in-stock', itemName: 'Peaches'}
]
```

And is rendered like this

```html
<li class='out-of-stock'>Oranges</li>
```

Which parts of this element shows dynamic data? Let's take a look:

```html
<li class='out-of-stock'>Oranges</li>
```

- The class name changes from `out-of-stock` to `in-stock`
- The item name changes too - this is `Oranges` but there are list items for `Grapes` and other fruit.


##### 3. Let's Create a Template

Copy and chop it that DOM element up! Take our DOM element
```html
<li class='out-of-stock'>Oranges</li>
```

and replace areas that has dynamic data. The following data can be rendered piece by piece:

```javascript
[
	{ stockStatusClass: 'in-stock', itemName: 'Apples'},
	{ stockStatusClass: 'out-of-stock', itemName: 'Oranges'}
	{ stockStatusClass: 'in-stock', itemName: 'Grapes'},
	{ stockStatusClass: 'in-stock', itemName: 'Peaches'}
]
```

##### 4. Using this template

```handlebars
<li class='{{stockStatusClass}}'>{{itemName}}</li>
```

```handlebars
<li class='{{stockStatusClass}}'>{{itemName}}</li>
```

##### 5. Template Examples

A blog entry:

```handlebars
<article class='blog-entry'>
	<h3>{{title}}</h3>
	<span class='author'>{{author}}</span>
	<span class='publish-data'>{{publishDate}}</span>
	<p>{{blogContent}}</p>
</article>
```

A table: 

```handlebars
<table>
	<thead>
		<tr class='invoice-line-item-header'>
			<td class='row-index-key'>{{idLabel}}</td>
			<td>{{productLabel}}</td>
			<td>{{descriptionLabel}}</td>
			<td>{{priceLabel}}</td>
		</tr>
	</thead>
	<tbody>
	{{#each invoiceItem}}
		<tr class='invoice-line-item'>
			<td class='row-index-key'>{{id}}</td>
			<td>{{product}}</td>
			<td>{{description}}</td>
			<td>{{price}}</td>
		</tr>
	{{/each}}
	</tbody>
</table>
```