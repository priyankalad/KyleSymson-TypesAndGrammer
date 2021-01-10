---


---

<h2 id="arrays"><strong>Arrays</strong></h2>
<ul>
<li>
<p>It’s a containers for any type of value, from string to number to object or even an array(which is how you get multidimensional array)<br>
<code>var a =[1,"2",[3],{b:2}];</code></p>
</li>
<li>
<p>You don’t need to presize your arrays.</p>
<pre><code>var a =[];
a[0]=1;
a[1]="2";
</code></pre>
</li>
<li>
<p>It’s tricky that arrays also are objects that can have string keys/properties added to them (but which don’t count toward the length of the array)</p>
<pre><code>var a =[];
a[0]=1;
a["foobar"] = 2;
a.length // 1
</code></pre>
</li>
<li>
<p>if a string value intended as a key can be coerced to a standard base-10 number, then it is assumed that you wanted to use it as a number index rather than as a string key!</p>
<pre><code>var a=[];
a["13"] = 42;
a.length //14
</code></pre>
</li>
<li>
<p>Use objects for holding values in keys/properties and save arrays for strictly numerically indexed values.</p>
</li>
</ul>
<h2 id="array-likes"><strong>Array-Likes</strong></h2>
<ul>
<li></li>
</ul>

