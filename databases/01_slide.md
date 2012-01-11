!SLIDE subsection

#the great hash in the sky

!SLIDE bullets

"I've long treated the database as mainly being a big hash."
- DHH

!SLIDE image center

<img src="hits.jpg" width="800" height="400"/>

All hail the great hash in the sky!

!SLIDE

 *: Forgive me, I'm only going to talk about relational databases.

!SLIDE bullets

# purpose
* Store data reliably & efficiently.
* Query data flexibly & performantly.

!SLIDE subsection

#how does it work?

!SLIDE

<div id="layercake">
  <div>parser</div>
  <div>optimizer</div>
  <div>executor</div>
  <div>storage</div>
</div>

<div id="comments">
<i>(Modestly simplified for your thinking pleasure.)</i>
<br>
See <a href="http://www.postgresql.org/docs/9.1/static/overview.html">PostgreSQL Manual, Ch 44.</a>
</div>

!SLIDE

<div id="layercake">
  <div>PARSER</div>
  <div>optimizer</div>
  <div>executor</div>
  <div>storage</div>
</div>

<div id="comments">
<div><b>input</b>: SQL strings </div>
<div><b>output</b>: parse tree </div>
<br>
<p>
Responsibility includes syntax checking, error messages.
</p>
</div>

!SLIDE

<div id="layercake">
  <div>parser</div>
  <div>OPTIMIZER</div>
  <div>executor</div>
  <div>storage</div>
</div>

<div id="comments">
<div><b>input</b>: parse tree</div>
<div><b>output</b>: query plan</div>
<br>
<p>
Cost-based optimizer uses data statistics and available indices to find fastest query execution path.
</p>
</div>

!SLIDE

<div id="layercake">
  <div>parser</div>
  <div>optimizer</div>
  <div>EXECUTOR</div>
  <div>storage</div>
</div>

<div id="comments">
<div><b>input</b>: query plan</div>
<div><b>output</b>: result set</div>
<br>
<p>
Converts the query plan into execution nodes, retrieves results by recursively evaluating the execution nodes.
</p>
</div>

!SLIDE

<div id="layercake">
  <div>parser</div>
  <div>optimizer</div>
  <div>executor</div>
  <div>STORAGE</div>
</div>

<div id="comments">
<div><b>input</b>: data</div>
<div><b>output</b>: tuples</div>
<br>
<p>
Responsible for keeping track of data, statistics, and caching. Also, responsible for not losing stuff.
</p>
</div>
