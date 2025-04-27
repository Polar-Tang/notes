Hello, i'm Virtual Nautilus and this is a new summary for the people who's too lazy to fully comprehend documentation. In this article we will talk about how to set clip path to its full parent with.
### What's clip path
Clip path property allows you to display a specific region of an element to display. Clip path may have different values that represent SVG tags, svg `clip-path`, `clip-path: polygon();`, `clip-path: cirle();`, `clip-path: url(#c1);`, etc. For our task we will use an usage case of `clip-path: url(#c1);` as example.
### Example
Let's imagine you are working with a cool animation that utilize svg vectors (vectors are like coordinates but for create 2d shapes, widely used in web developing), so it's dynamically generated:
```jsx
<ul className=" shape-overlays__path wave-section bg-blue w-full z-5 relative"
	style={{
	clipPath: `path("${newVector}")`, // newVector= "M 0 0 V 100 ..."
	}}
>
</ul>
```
In this example we got a JSX where we utilize a vector, which may be a React state or simply a global variable being redefined.
Now the `newVector` has the desired SVG value, so the desired shape. But it's not scaled to the body's width.
#### Using clipPath
To solve this problem and scale this to our HTML body, we need to declare our svg tag (probably a path tag) wrapped by a `clipPath`, nested to a SVG. 
```html
<svg
viewBox="0 0 100 100"
>
	<clipPath id="pathClip" clipPathUnits="objectBoundingBox">	
		<path d={newVector}/>
	</clipPath>
</svg>
```
And define `viewbox` to utilize percentage, our path with our vectors, and `clipPathUnits` to the `objectBoundingBox` value to stretch the svg for ocupping the full container size.
So now let's `url()` instead of path for refering to this clipPath 
```jsx
<ul className=" shape-overlays__path wave-section bg-blue w-full z-5 relative"
	style={{
	clipPath: `url("${newVector}")`, // newVector= "M 0 0 V 100 ..."
	}}
>
</ul>
```