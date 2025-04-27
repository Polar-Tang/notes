I blame the univerisity to teach math without applying to real problem. According to my philosophy every knowledge that is not lead to the practice is useless. So let's jump into the code

I'm trying to replicate the bezier curve by a code i seen on the internet. That code works with multiple paths inside an svg tag. I copied and adapt it to react and it works smothly, but i also need to apply it in a single path, for a different component, so i remove every complexity unnecessary to make it work with a single path in a single svg tag. All i will say next will be comments in the code.
```jsx
const aboutContainerRef = useRef<HTMLDivElement>(null)

const section = aboutContainerRef.current;

useGSAP(() => {

// create a length for points
let numPoints = 10;

let delayPointsMax = 0.3;

let duration = 0.9;

let isOpened = true;

// array to create differences between the curves
let pointsDelay: number[] = [];
// points array
let points: number[] = []

if (!section) return;

	let tl = gsap.timeline({
		onUpdate: render,
		defaults: {
		ease: "power2.inOut",
		duration: duration,
		repeat: -1
		}
	})
// fill of 100s
	for (let j = 0; j < numPoints; j++) {
	points.push(100)
	}
	
	toggle();
	function toggle() {
		tl.progress(0).clear(); // ??

		// populate delayPoints
		for (let i = 0; i < numPoints; i++) {
		// make the delay asymmetric
			pointsDelay[i] = Math.random() * delayPointsMax
		}

		// animate each item value with gsap 
		for (let j = 0; j < numPoints; j++) {
			let delay = pointsDelay[j];
			tl.to(points, {
				[j]: 0 // ??
			}, delay)
		}
	}
	// function runned on Update
	function render() {
	// d path attribute
	let d = "";
	// define the base of the svg vector
	d += isOpened ? `M 0 0 V ${points[0]} C` : `M 0 ${points[0]} C`
	// here's the stuff 
	for (let j = 0; j < numPoints - 1; j++) {	
		let p = (j + 1) / (numPoints - 1)
		let cp = p - (1 / (numPoints - 1)) / 2

		d += ` ${cp} ${points[j]} ${cp} ${points[j + 1]} ${p} ${points[j + 1]}`	
	}	
	d += isOpened ? ` V 100 H 0` : ` V 0 H 0`	
	setclipPathState(`${d}`)
	}
}, [])
```