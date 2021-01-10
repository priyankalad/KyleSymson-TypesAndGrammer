---


---

<h2 id="built-in-types"><strong>Built-In types</strong></h2>
<ul>
<li>
<p>Javascript defines seven built-in types</p>
<ol>
<li>null</li>
<li>undefined</li>
<li>boolean</li>
<li>number</li>
<li>string</li>
<li>object</li>
<li>symbol - added in ES6!</li>
</ol>
<p>– All of these types except object are called “primitives”</p>
</li>
<li>
<p>null is <em>special</em> - special in the sense that it’s buggy when combined with the typeof operator</p>
<p><code>typeof null===object //true</code></p>
<p>– It would have been nice ( and correct!) if it returned “null”, but this original bug in JS has persisted for nearly two decades, and will likely never be fixed because there’s so much existing web content that relies on its buggy behavior that “fixing” the bug would create more “bugs” and break a lot of software.</p>
</li>
<li>
<p>test for a null using its type<br>
<code>var a =null</code><br>
<code>(a! &amp;&amp; a ==="object")//true</code></p>
</li>
<li>
<p>Type of function<br>
<code>typeof function a(){/*..*/} === "function" //true</code></p>
<p>–	Function is actually a subtype of <code>object</code></p>
<p>–	It is referred to as a “callable object” - an object that has an 			internal [[call]] property that allows it to be invoked</p>
<p>–	Functions can have properties</p>
</li>
<li>
<p>Arrays are also “subtype” of object with additional characteristics of being numerically indexed.<br>
<code>typeof [1,2,3] ==="object" //true</code></p>
</li>
</ul>
<h2 id="values-as-types"><strong>Values as Types</strong></h2>
<ul>
<li>
<p>In JS, variables don’t have types-<em>values have types</em>. Variables can hold any value , at any time.</p>
</li>
<li>
<p>The <code>typeof</code> operator always returns string. e.g. “string”, “number”, “object” etc…</p>
</li>
<li>
<p>An “undefined” variable  is one that has been declared in the accessible scope, but at the moment has no other value in it. By contract, an “undeclared” variable is one that has not been formally declared in the accessible scope.</p>
<pre><code>var a;
a; //undefined
b; //ReferenceError: b is not defined
</code></pre>
</li>
</ul>
<p>–  It’d be nice if the browser said something like “b is not found” or “b is not declared” to reduce the confusion.</p>
<ul>
<li>The <code>typeof</code> returns “undefined” even for “undeclared” variables and it does not throw any error too.<pre><code>var a;
typeof a; //undefined
typeof b; //undefined
</code></pre>
</li>
</ul>
<p>– It’d be nice if <code>typeof</code> used with an undeclared variable returned “undeclared” instead of “undefined”</p>
<h2 id="typeof-undeclared"><strong>typeof Undeclared</strong></h2>
<ul>
<li>
<p>The safety guard(preventing an error) on typeof when used against an undeclared variable can be useful in certain cases.</p>
</li>
<li>
<p>One case is when dealing with JS in the browser where multiple script files can load variables into the shared global namespace.</p>
</li>
<li>
<p>A top-level global <code>var DEBUG = true</code> declaration would only be included in a “debug.js” which you only load into the browser during development/testing mode and not in production</p>
<pre><code>//oops this would throw an error!
if(DEBUG){
	console.log("Debugging is starting");
}

//this is  a safe existence check
if(typeof DEBUG !== "undefined"){
	console.log("Debugging is starting");
}
</code></pre>
</li>
<li>
<p>Another case is if you are doing a feature check for a built-in API.</p>
<pre><code>if(typeof atob === "undefined"){
	atob = function(){/*..*/}
}
</code></pre>
</li>
<li>
<p>Another way of doing these checks against global variables, but without the safety guard feature of typeof  is by using <code>window</code> object</p>
<pre><code>if(window.DEBUG){
//...
}
if(!window.atob){
//...
}
</code></pre>
<p>– This is because, there is no <code>ReferenceError</code> thrown if you try to access an object property (even on the global/window object) that doesn’t exist</p>
<p>– On the other hand, this approach should be avoided especially if your code needs to run in multiple environments, where global variable may not be called “window”.</p>
</li>
<li>
<p>Imagine a utility function that you want others to copy-paste into their programs or modules, in which you want to check to see if the including program has defined a certain variable or not.</p>
<pre><code>(function(){
	function FeatureXYZ(){/*...*/}
	
	function doSomethingCool(){
		var helper = (typeof FeatureXYZ !=="undefined")
		?FeatureXYZ
		: function(){/*..default feature..*/}
	}
	doSomethingCool();
})();
</code></pre>
</li>
<li>
<p>Some developers prefer a design pattern called “dependency injection”,  where instead of doSomethingCool() inspecting implicitly for FeatureXYZ to be defines outside/around it, it would need to have the dependency explicitly passed in, like:</p>
<pre><code>function doSomethingCool(FeatureXYZ){
	var helper = FeatureXYZ || function(){/*.. default feature..*/};
	var val = helper();
	//
}
</code></pre>
</li>
</ul>

