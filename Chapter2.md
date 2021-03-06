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
<li>
<p>There will be occasions where you need to convert an array-like value (a numerically indexed collection of values) in to a true array, so that you can call array utilities (like indexOf, concat, forEach).</p>
</li>
<li>
<p>Examples of array-like values are list of DOM elements, <code>arguments</code>	object exposed by functions (as of ES6, deprecated).</p>
</li>
<li>
<p><code>slice</code> utility (without parameters) can be used to convert such values into true array</p>
<pre><code>function foo(){
	var arr = Array.prototype.slice.call(arguments);
	arr.push("bam");
	console.log(arr);
}
foo("bar","baz"); //["bar","baz","bam"]
</code></pre>
</li>
<li>
<p>In ES6, this conversion can be done using a built-in utility Array.from(…)</p>
<pre><code>var arr = Array.from(arguments);
</code></pre>
</li>
</ul>
<h2 id="strings"><strong>Strings</strong></h2>
<ul>
<li>
<p>Javascript strings are really not the same as arrays of characters.</p>
</li>
<li>
<p>Strings do have a shallow resemblance to arrays. For instance, both of them have a <code>length</code> property, an <code>indexOf</code> method and a <code>concat</code> method</p>
<pre><code>var a = "foo";
var b = ["f","o","o"];
</code></pre>
</li>
<li>
<p>Javascript strings are immutable while arrays are mutable.</p>
<pre><code>a[1]="0"; //correct approach is a.charAt(1)for string
b[1]="0";
a;//"foo";
b;//["f","0","o"]
</code></pre>
</li>
<li>
<p>None of the string methods that alter its contents can modify in-place, but rather must create and return new strings. By contract, many of the array methods that change array contents actually do modify in-place.</p>
<pre><code>c= a.toUpperCase();
a===c; //false
a; //"foo"
c; //"FOO"
</code></pre>
</li>
<li>
<p>We can borrow non-mutation array methods against our string, so that we can make use of array methods.</p>
<pre><code>a.join; //undefined
a.map; //undefined

Ex1: var c = Array.prototype.join.call(a,"-");
Ex2:
 var d = Array.prototype.map.call(a, function(v)				 	{
	return v.toUpperCase()+".";
}).join("");

c; //"f-o-o"
d; //"F.O.O."

Ex3: a.reverse //undefined
	b.reverse(); //["o","0","f"]
</code></pre>
</li>
<li>
<p>Unfortunately this “borrowing” doesn’t work with array mutatos because strings are immutable and can’t be modified in-place</p>
<pre><code>Array.prototype.reverse.call(a);
//Uncaught TypeError: Cannot assign to read only property '0' of object '[object String]'
</code></pre>
</li>
<li>
<p>Below is the workaround to reverse a string</p>
<pre><code>var c = a.split("").reverse().join("");
c; //"oof"
//this approach doesn't work with complex(unicode) characters in them
</code></pre>
</li>
<li>
<p>If you are more commonly doing tasks on your “strings” that treat them as basically arrays of characters, perhaps its better to just actually store them as arrays rather than as strings. You can always call <code>join("")</code> whenever you actually need a string representation.</p>
</li>
</ul>
<h2 id="numbers"><strong>Numbers</strong></h2>
<ul>
<li>
<p>Javascript has just one numeric type: <code>number</code>. This type includes both integer values and fractional decimal numbers</p>
</li>
<li>
<p>The implementations of Javascript’s numbers is based on the “IEEE 754” standard, often called “floating-point”. Javascript specifically uses “double precision” format (aka 64-bit binary) of the standard</p>
<p><em><strong><u>Numeric Syntax</u></strong></em></p>
<ul>
<li>
<p>By default, Most numbers are outputted as base-10 					   decimals, with trailing fractional 0s removed.</p>
<pre><code>var a = 42.300;
var b = 42.0;
a; // 42.3
b; // 42
</code></pre>
</li>
<li>
<p>The leading portion of a decimal value, if 0, is optional</p>
<pre><code>var a = 0.42;
var b = .42;
</code></pre>
</li>
<li>
<p>Similarly, the trailing portion (the fractional)<br>
of a decimal value after the ., if 0, is optional</p>
<pre><code>var a = 42.0;
var b = 42.;
</code></pre>
</li>
<li>
<p>Very large or very small numbers will by default be outputted in exponent form, the same as the output of the toExponential() method:</p>
<pre><code>var a = 5E10;
a;//50000000000
a.toExponential(); //"5e+10"

var b=  a*a;
b; //2.5e+21;

var c = 1/a;
c; // 2e-11
</code></pre>
</li>
<li>
<p>Because <code>number</code> values can be boxed with the Number object wrapper,  <code>number</code> values can access methods  (toFixed , toPrecision) that are built into the Number.prototype.</p>
<pre><code>var a = 42.59;
a.toFixed(0); //"43"
a.toFixed(1); //"42.6"
a.toFixed(2); //"42.59" 
a.toFixed(3); //"42.590"
a.toFixed(4); //"42.5900"

a.toPrecision(1); //"4e+1"
a.toPrecision(2); //"43"
a.toPrecision(3); //"42.6"
a.toPrecision(4); //"42.59"
a.toPrecision(5); //"42.590"
a.toPrecision(6); //"42.5900"
</code></pre>
</li>
<li>
<p>Since . is a valid numeric character, it will be interpreted as part of the number literal, if possible, instead of being interpreted as a property accessor:</p>
<pre><code>//invalid syntax:
42.toFixed(3); //Syntax error //because 42. is a valid nunber, so there's no . property operator present to make the .toFixed access

//these are all valid:
(42).toFixed(3); //"42.000"
0.42.toFixed(3); //"0.420"
42..toFixed(3); //"42.000"
42 .toFixed(3); //"42.000"
</code></pre>
</li>
<li>
<p>numbers can also be specified in exponent form (for large numbers), binary, octal and hexadecimal forms.</p>
<pre><code>E=10
var oneThousand = 1E3; //means 1 * 10^3
var oneMillionOneHundredThousand = 1.1E6; // means 1.1 * 10^6

0xf3 //hexdecimal for 243 //x or X can be used.
0o363 //octal for 243 //o or O can be used.
0b11110011 //binary for 243 //b or B can be used.
</code></pre>
<p>Always use lower case predicates (0x,0b,0o) to avoid confusion</p>
</li>
</ul>
<p><em><strong><u>Small Decimal Values</u></strong></em></p>
<ul>
<li>
<p>The most infamous side effect of using binary floating-point 		numbers is:</p>
<pre><code>	`0.1 + 0.2 === 0.3; //false`
</code></pre>
</li>
<li>
<p>The representations for 0.1 and 0.2 in binary floating point are not exact, so when they are added, the result is not exactly 0.3. It’s <em>really</em> close, 0.30000000000000004, but if your comparison fails, “close” is irrelevant.</p>
</li>
<li>
<p>Common practice is to use a tiny “rounding error” value as the <em>tolerance</em> for comparison, which is often called “machine epsilon”, which is commonly 2^-52 (2.220446049250313e-16).</p>
</li>
<li>
<p>As of ES6, Number.EPSILON is predefined with this tolerance.</p>
<pre><code>//polyfill for pre-ES6:
if(!Number.EPSILON){
	Number.EPSILON = Math.pow(2,-52);
}
</code></pre>
</li>
<li>
<p>We can use Number.EPSILON to compare two numbers for equality (within rounding error tolerance)</p>
<pre><code>function numbersCloseEnoughToEqual(n1, n2){
	return Math.abs(n1-n2)&lt;Number.EPSILON;
}
numbersCloseEnoughToEqual(0.1+0.2, 0.3) //true
numbersCloseEnoughToEqual(0.0000001,0.0000002); //false
</code></pre>
</li>
<li>
<p>Maximum floating-point value can be represented is roughly 1.798e+308, predefined as Number.MAX_VALUE.</p>
</li>
<li>
<p>On the small end, Number.MIN_VALUE is roughly 5e-324, which isn’t negative, but is really close to zero!</p>
</li>
</ul>
<p><em><strong><u>Safe Integer Ranges</u></strong></em></p>
<ul>
<li>
<p>Because of how numbers are represented , there is a range of “safe” values for the whole number “integers”, and it’s significantly less than Number.MAX_VALUE.</p>
</li>
<li>
<p>The maximum integer that can “safely” be represented is 2^53 -1, which is 9007199254740991 (9 quadrillion) and minimum value is -9007199254740991. In ES6, These values are predefined as Number.MAX_SAFE_INTEGER and Number.MIN_SAFE_INTEGER respectively.</p>
</li>
</ul>
<p><em><strong><u>Testing for Integers</u></strong></em></p>
<ul>
<li>
<p>To test, if a value is an integer, ES6-specified  Number.isInteger(…) is used.</p>
<pre><code>    Number.isInteger(42);    //true		
    Number.isInteger(42.000);    //true		
    Number.isInteger(42.3);    //false	
</code></pre>
</li>
<li>
<p>Polyfill for Number.isInteger()</p>
<pre><code>if(!Number.isInteger){
	Number.isInteger = function(num){
		return typeof num=="number" &amp;&amp; num%1==0;
	}
}
</code></pre>
</li>
<li>
<p>To test, if a value is an safe integer, ES6-specified  Number.isSafeInteger(…) is used.</p>
<pre><code>Number.isSafeInteger(Number.MAX_SAFE_INTEGER); //true
Number.isSafeInteger(Math.pow(2,53)); //false
Number.isSafeInteger(Math.pow(2,53)-1); //true
</code></pre>
</li>
<li>
<p>Polyfill for Number.isSafeInteger()</p>
<pre><code>if(!Number.isSafeInteger){
	Number.isSafeInteger = function(num){
		return Number.isNumber(num) &amp;&amp; Math.abs(num)&lt;=Number.MAX_SAFE_INTEGER;
	}
}
</code></pre>
</li>
</ul>
<p><em><strong><u>32-Bit (Signed) Integers</u></strong></em></p>
<ul>
<li>
<p>While integers can range up to roughly 9 quadrillion safely (53 bits), there are some numeric operations (e.g. bitwise) that are only defined for 32 bit numbers, so the safe range for numbers used in that way must be much smaller. (from Math.pow(-2,31) to Math.pow(2,31)-1)</p>
</li>
<li>
<p>To  force a number value in a to 32-bit signed integer value, use a|0.</p>
</li>
</ul>
</li>
</ul>
<h2 id="special-values"><strong>Special Values</strong></h2>
<p><em><strong><u>The nonvalue Values</u></strong></em></p>
<ul>
<li>
<p>For both <code>undefined</code> and <code>null</code>, the label is both its type and value.</p>
</li>
<li>
<p>Both <code>undefined</code> and <code>null</code> are often taken to be interchangeable as either empty values or non values</p>
</li>
<li>
<p>Other developers prefer to distinguish between them with nuance. For example:<br>
<code>null</code> is an empty value  and <code>undefined</code> is a  missing value.<br>
or<br>
<code>undefined</code> hasn’t had a value yet  and <code>null</code> had a value  but doesn’t anymore.</p>
</li>
<li>
<p><code>null</code> is a special keyword and not an identifier (cannot be used as a variable name), while <code>undefined</code> is an identifier.</p>
</li>
</ul>
<p><em><strong><u>Undefined</u></strong></em></p>
<ul>
<li>In non-strict mode, its possible to assign a value to the globally provided <code>undefined</code> identifier.<pre><code>function foo(){
	undefined=2; //really bad idea!
}
function foo(){
	"use strict";
	undefined=2; //TypeError!
}
</code></pre>
</li>
<li>In both strict and non-strict mode, you can create a local variable of the name <code>undefined</code>, but again , this is a terrible idea!</li>
</ul>
<p><em><strong><u>void operator</u></strong></em></p>
<ul>
<li>
<p>The expression void  “voids” out any value , so that the result of the expression is always the <code>undefined</code> value.</p>
</li>
<li>
<p>It doesn’t modify the existing value, it just ensures that no value comes back from the operator expression. There is no practical difference between void 0, void 1 and undefined</p>
<pre><code>var a=2;
console.log(void a, a) // undefined 42
</code></pre>
</li>
</ul>
<h2 id="special-numbers"><strong>Special Numbers</strong></h2>
<p><em><strong><u>The not number, number</u></strong></em></p>
<ul>
<li>
<p>Any mathematic operation you perform without both operands being <code>number</code> will result in the operation failing to produce a valid number, in which case you will get <code>NaN</code> value</p>
</li>
<li>
<p>Type of NaN is <code>number</code></p>
</li>
<li>
<p>NaN is a very special value in that it’s never equal to another NaN value</p>
<pre><code>var a = 2/"foo";
a === NaN //wrong way to compare if value is a valid number

isNaN(a) //correct way
</code></pre>
</li>
<li>
<p>The job of isNaN is basically , “test if the thing passed in is either not a number or is a number”. But that’s not quite accurate.</p>
<pre><code>window.isNaN(2/"foo"); //true;
window.isNaN("foo"); //true --- ouch!
</code></pre>
<p>Clearly “foo” is literally not a number, but it’s definitely not the NaN value either! This bug has been in JS since the very beginning (over 19 years of ouch).</p>
</li>
<li>
<p>Below is the replacement polyfill utility in ES6</p>
<pre><code>if(!Number.isNaN){
    Number.isNaN = function(n){
   	 return (typeof n ==="number" &amp;&amp; window.isNaN(n));
    }
}

// even more easier way
if(!Number.isNaN){
    Number.isNaN = function(n){
   	return n!==n;
    }
}
</code></pre>
</li>
<li>
<p>If you’re currently using just isNaN(…) in a program, the sad reality is your program has a bug, even if you haven’t been bitten yet!</p>
</li>
</ul>
<p><em><strong><u>Infinities</u></strong></em></p>
<ul>
<li>
<p>Developers from traditional compiled languages like C are probably used to seeing either a compiler error or runtime exception, like “divide by zero”, for an operation like <code>var a = 1/0;</code></p>
</li>
<li>
<p>However in JS, this operation is well-defined and results in the value <code>Infinity</code>(aka Number.POSITIVE_INFINITY).</p>
<pre><code>var a = 1/0 //Infinity
var b = -1/0 //-Infinity
</code></pre>
</li>
<li>
<p>JS uses finite numeric representation (IEEE 754 floating point), so contrary to pure mathematics, it seems it is possible to overflow even with an operation like addition or subtraction, in which case, you’d get Infinity or -Infinity</p>
<pre><code>var a= Number.MAX_VALUE; //1.7976931348623157e+308
a+a //Infinity
</code></pre>
</li>
<li>
<p>According to the spec, if the operation like addition results in a value that’s too big to represent, the IEEE 754 “rounds to nearest” mode specifies what the result should be.</p>
<pre><code>Number.MAX_VALUE+Math.pow(2,969) //closer to max value, so it rounds down
Number.MAX_VALUE+Math.pow(2,970) //closer to Infinity, so it rounds up
</code></pre>
</li>
</ul>

