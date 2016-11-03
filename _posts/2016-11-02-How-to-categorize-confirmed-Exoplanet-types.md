---
layout: post
title: "Categorizing Exoplanets by Type: Part One - Research"
---

It is no secret that I am a space nerd. Over the past few months, I have decided to work on an Exoplanet visualization application (based on an Android application prototype that I had designed). I decided that I wanted to see how many planets of what type have been found by Kepler. The goal is to create a module that will allow users to input a CSV file from the [NASA Exoplanet Archive](http://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=planets) and then render a list of planets by type. This introduced a few interesting challenges. First, Javascript isn't [very precise with floating point values](http://stackoverflow.com/questions/11695618/dealing-with-float-precision-in-javascript). Second, how will I convert CSV data from NASA into something usable? Finally, how will I figure out what the planet type even is? Once I solve these questions, I can then take that knowledge and build a library/module/tool. However, before we can start, I needed to perform some research. In the firs part of this series of posts, I will outline my research, conclusion and results. 

#### Project Outline

Our user stories are as follows:

- A user instantiates our module with a NASA provided CSV file as an argument
- A user queries for a list of planets that kepler has detected and is provided an Object
- A user queries for an official list of planetary types to view and is provided an Object
- A user queries for a list of solar systems and is provided an Object
- For any result a user requests, a user may export the results to a prettified JSON file

These sounds like  pretty reasonable user stories. The module itself is not too complex but serves a single purpose: to allow users to convert NASA Exoplanet data into something usable.

#### Getting the NASA data

NASA provides most of their data in some format or another. The Exoplanet Archive at CalTech provides a [very nice interface](http://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=planets). They have provided a lot of data in this exoplanet table. We are able to speculate the mass of an exoplanet based on transits that it makes between our telescopes and its host star. This mass is usually referred to as a 'Jupiter Mass' (or, how many Jupiters it would take to achieve the mass of this planet). However, I needed to filter out superfluous data. Before I could determine what data we'll need, I needed to figure out how planets are categorized. In 2011, [Abel Mendez](http://phl.upr.edu/library/notes/amassclassificationforbothsolarandextrasolarplanets) from the University of Puerto Rico at Arecibo published a paper that provides a scale to classify planets by their mass. However, these masses were in relative earth masses; not relative Jupiter masses. 

<img class="responsive-img" src="/images/EMC.jpg">

I decided to structure these as JSON and as a Javascript module for future use.

<script src="https://gist.github.com/code-for-coffee/70d06dc8090d7645e728f5fd786d2774.js"></script>

Thankfully, I can easily convert a Jupiter mass to an Earth mass. I do this by comparing how many Earth mases it would take to fill up Jupiter. According to wikipedia, [https://en.wikipedia.org/wiki/Jupiter_mass](Jupiter is equivilent to 317.83 Earth masses):

_Convert Jupiter mass to Earth Mass_

```
1 (jupiter mass)       input_value
______               = ___________
317.83 (earth mass)    output_value
```

In keeping with our user stories, I now know that we can find the planet type and with some math, calculate the available Jupiter mass values that each exoplanet has. I can also check off querying for a list of planet types; I just need to turn that into a data structure. I also need to know which star (or host) that the planet is orbiting around. [Heading back to the exoplanet Archive](http://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=planets), I begin to look at each column. I pay attention to a few values that I'll think will be important: **Row ID**, **Host Name**, **Planet Letter**, **Number of Planets in System**, **Planet Mass (in Jupiter Mass)**. I then _select columns_ and de-select every column but those I mentioned. From there, I select **Download Table**, _download currently checked columns_, and select _download table as CSV_.

<img class="responsive-img" src="/images/exoplanet_tutorial.png">

> _Screenshot of the NASA Exoplanet Archive with Columns Filtered_

Awesome! Now I have this wonderful `planets.csv` in my downloads folder. Upon inspection, it contains over a thousand rows of precious planets that the human race has discovered. Columns are identified in comments for ease of use/readability (thanks, NASA engineers).

```csv
# This file was produced by the NASA Exoplanet Archive  http://exoplanetarchive.ipac.caltech.edu
# Wed Nov  2 16:07:55 2016
#
# COLUMN pl_hostname:    Host Name
# COLUMN pl_letter:      Planet Letter
# COLUMN pl_pnum:        Number of Planets in System
# COLUMN pl_orbper:      Orbital Period [days]
# COLUMN pl_orbpererr1:  Orbital Period Upper Unc. [days]
# COLUMN pl_orbpererr2:  Orbital Period Lower Unc. [days]
# COLUMN pl_orbperlim:   Orbital Period Limit Flag
# COLUMN pl_bmassj:      Planet Mass or M*sin(i)[Jupiter mass]
# COLUMN pl_bmassjerr1:  Planet Mass or M*sin(i)Upper Unc. [Jupiter mass]
# COLUMN pl_bmassjerr2:  Planet Mass or M*sin(i)Lower Unc. [Jupiter mass]
# COLUMN pl_bmassjlim:   Planet Mass or M*sin(i)Limit Flag
# COLUMN pl_radj:        Planet Radius [Jupiter radii]
# COLUMN pl_radjerr1:    Planet Radius Upper Unc. [Jupiter radii]
# COLUMN pl_radjerr2:    Planet Radius Lower Unc. [Jupiter radii]
# COLUMN pl_radjlim:     Planet Radius Limit Flag
#
rowid,pl_hostname,pl_letter,pl_pnum,pl_orbper,pl_orbpererr1,pl_orbpererr2,pl_orbperlim,pl_bmassj,pl_bmassjerr1,pl_bmassjerr2,pl_bmassjlim,pl_radj,pl_radjerr1,pl_radjerr2,pl_radjlim
1,11 Com,b,1,326.03000000,0.32000000,-0.32000000,0,19.40000,1.50000,-1.50000,0,,,,
2,11 UMi,b,1,516.22000000,3.25000000,-3.25000000,0,10.50000,2.47000,-2.47000,0,,,,
```

> _Sample CSV file from the Exoplanet Archive_

Now, I just need to convert my CSV to a JSON file for ease of use with Javascript. After some research, I found a nice [npm module](https://www.npmjs.com/package/csvtojson) called `csvtojson`. I decided to give it a whirl. After a bit, I threw together the following class to convert my CSV to JSON.

<script src="https://gist.github.com/code-for-coffee/0793992d6a521b813db3ac1aa373df73.js"></script>

To execute and test my code, I just run `node CSVObjectifier.js` in the same directory as my CSV file. When I do, I get some weird results... each object looks mangled.

```javascript
{ '# This file was produced by the NASA Exoplanet Archive  http://exoplanetarchive': { ipac: [Object] },
    field2: 'CoRoT-28',
    field3: 'b',
    field4: 1,
    field5: 5.20851,
    field6: 0.00038,
    field7: -0.00038,
    field8: 0,
    field9: 0.484,
    field10: 0.087,
    field11: -0.087,
    field12: 0,
    field13: 0.955,
    field14: 0.066,
    field15: -0.066,
    field16: 0 },
```

I hypothesize that this is because of the comments at the top of our CSV file (they begin with `# ...`). To test this hypothesis, I remove the comments to the point where the first line of my CSV file is `rowid,pl_hostname,pl_letter,pl_pnum,pl_orbper,pl_orbpererr1,pl_orbpererr2,pl_orbperlim,pl_bmassj,...` and save file. Upon testing, it appears that my hypothesis was validated: each POJO (plain old Javascript object) looks clean.

```js
{ rowid: 100,
    pl_hostname: 'GJ 163',
    pl_letter: 'b',
    pl_pnum: 3,
    pl_orbper: 8.631,
    pl_orbpererr1: 0.001,
    pl_orbpererr2: -0.001,
    pl_orbperlim: 0,
    pl_bmassj: 0.035,
    pl_bmassjerr1: '',
    pl_bmassjerr2: '',
    pl_bmassjlim: 0,
    pl_radj: '',
    pl_radjerr1: '',
    pl_radjerr2: '',
    pl_radjlim: '' },
  ... 3302 more items ]
```

My output file looks like a great starting point for any Node.js (or really, anything) to work with!

```json
[{"rowid":1,"pl_hostname":"11 Com","pl_letter":"b","pl_pnum":1,"pl_orbper":326.03,"pl_orbpererr1":0.32,"pl_orbpererr2":-0.32,"pl_orbperlim":0,"pl_bmassj":19.4,"pl_bmassjerr1":1.5,"pl_bmassjerr2":-1.5,"pl_bmassjlim":0,"pl_radj":"","pl_radjerr1":"","pl_radjerr2":"","pl_radjlim":""},{"rowid":2,"pl_hostname":"11 UMi","pl_letter":"b","pl_pnum":1,"pl_orbper":516.22,"pl_orbpererr1":3.25,"pl_orbpererr2":-3.25,"pl_orbperlim":0,"pl_bmassj":10.5,"pl_bmassjerr1":2.47,"pl_bmassjerr2":-2.47,"pl_bmassjlim":0,"pl_radj":"","pl_radjerr1":"","pl_radjerr2":"","pl_radjlim":""},{"rowid":3,"pl_hostname":"14 And","pl_letter":"b","pl_pnum":1,"pl_orbper":185.84,"pl_orbpererr1":0.23,"pl_orbpererr2":-0.23,"pl_orbperlim":0,"pl_bmassj":4.8,"pl_bmassjerr1":"","pl_bmassjerr2":"","pl_bmassjlim":0,"pl_radj":"","pl_radjerr1":"","pl_radjerr2":"","pl_radjlim":""},{"rowid":4,"pl_hostname":"14 Her","pl_letter":"b","pl_pnum":1,"pl_orbper":1773.4,"pl_orbpererr1":2.5,"pl_orbpererr2":-2.5,"pl_orbperlim":0,"pl_bmassj":4.64,"pl_bmassjerr1":0.19,"pl_bmassjerr2":-0.19,"pl_bmassjlim":0,"pl_radj":"","pl_radjerr1":"","pl_radjerr2":"","pl_radjlim":""},{"rowid":5,"pl_hostname":"16 Cyg B","pl_letter":"b","pl_pnum":1,"pl_orbper":798.5,"pl_orbpererr1":1,"pl_orbpererr2":-1,"pl_orbperlim":0,"pl_bmassj":1.68,"pl_bmassjerr1":0.15,"pl_bmassjerr2":-0.15,"pl_bmassjlim":0,"pl_radj":"","pl_radjerr1":"","pl_radjerr2":"","pl_radjlim":""}]
```

> _The above JSON output is truncated._ View the full JSON file [on Github](https://gist.github.com/code-for-coffee/7f085a0c885d6fd3f271f1f8c392c224).

Now, I have been able to get the data I need, convert it a bit, and I know I can use it with practically anything now that it is JSON. My object structure even looks pretty! 

#### Considering Mathematical Accuracy

Javascript uses 64-bit floating point numbers for the `number` type that we use everything with `parseInt()` and `parseFloat()`. This means that we have an upper limit of a numerical value: [9,223,372,036,854,775,807](https://en.wikipedia.org/wiki/9223372036854775807). Because one of the primary jobs in figuring out a planet's size requires calculating what appear to be large numbers, we should write a script to see if any numerical values fall beyond this range. We can do so by looping through each object in our JSON. According to the CSV comments, *pl_bmassj* is our key/column that contains the planet's mass (see them commented above; I removed them from my CSV for conversion). To test this, I wrote a script that checked each row's `pl_bmassj`.

<script src="https://gist.github.com/code-for-coffee/b8b95ba78579e380db1bb757f3440ced.js"></script>

After running this script, it appears that none of my numbers are too large. This is excellent news (no need to implement a BigDecimal [style library](https://github.com/dtrebbien/BigDecimal.js)). However, in my log output I noticed that quite a few rows didn't have values at all. This is okay; due to [our current methods of detecting exoplanets](https://en.wikipedia.org/wiki/Methods_of_detecting_exoplanets), scientists are not always able to calculate an accurate mass. I should take a note of that and find out how many planets we can categorize vs how many we cannot.

<script src="https://gist.github.com/code-for-coffee/54c8a19218ba4b4fb42314338bf2d296.js"></script>

So about 1/3 of confirmed exoplanets have mass. That data alone is interesting. Given all of the research that I have done so far, I now have a wealth of data and information to sift through. I have found that everything that I need: a way to categorize planets, mass information and the conversions needed, and I have ruled out any need for a large numeric type support in Javascript.

In the next post in this series, I will take this knowledge and put it to use with Node.js. I'll use the research acquired and begin building out a node module that anyone can use with the intent of putting it online for anyone to use. In the final post of the series, I'll use the module to render findings on the NASA exoplanet dataset. See you soon!