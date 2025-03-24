# Locomotive Scroll & GSAP Integration

## Overview
This project integrates **Locomotive Scroll** with **GSAP ScrollTrigger** to create smooth scrolling effects and animations. Locomotive Scroll provides smooth scrolling, while GSAP's ScrollTrigger syncs animations with the scroll position.

## Features
- **Smooth scrolling** using Locomotive Scroll.
- **Scroll-linked animations** using GSAP ScrollTrigger.
- **Dynamic text animation** where words in `#page2>h1` are wrapped in `<span>` tags and animated on scroll.

## Installation
### Prerequisites
Ensure you have **GSAP** and **Locomotive Scroll** included in your project. You can add them via CDN or install via npm/yarn:

```sh
npm install gsap locomotive-scroll
```

### Including in HTML
Add the following script tags to include GSAP and Locomotive Scroll via CDN:

```html
<!-- Locomotive Scroll -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/locomotive-scroll/dist/locomotive-scroll.css">
<script src="https://cdn.jsdelivr.net/npm/locomotive-scroll/dist/locomotive-scroll.min.js"></script>

<!-- GSAP & ScrollTrigger -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>
```

## Implementation
### JavaScript Logic
1. **Initialize Locomotive Scroll** on `#main`.
2. **Sync Locomotive Scroll with GSAP ScrollTrigger**.
3. **Wrap each word of `#page2>h1` in a `<span>` tag** for staggered animation.
4. **Animate the text spans** with GSAP as they enter and exit the viewport.

### Code Explanation
```javascript
function locomotive_scroll(){
  gsap.registerPlugin(ScrollTrigger);

  const locoScroll = new LocomotiveScroll({
    el: document.querySelector("#main"),
    smooth: true
  });
  
  locoScroll.on("scroll", ScrollTrigger.update);

  ScrollTrigger.scrollerProxy("#main", {
    scrollTop(value) {
      return arguments.length ? locoScroll.scrollTo(value, 0, 0) : locoScroll.scroll.instance.scroll.y;
    },
    getBoundingClientRect() {
      return {top: 0, left: 0, width: window.innerWidth, height: window.innerHeight};
    },
    pinType: document.querySelector("#main").style.transform ? "transform" : "fixed"
  });

  ScrollTrigger.addEventListener("refresh", () => locoScroll.update());
  ScrollTrigger.refresh();
}
locomotive_scroll();
```

### Text Animation Code
```javascript
var clutter = "";
document.querySelector("#page2>h1").textContent.split(" ").forEach(function(dets){
  clutter += `<span> ${dets} </span>`;
});
document.querySelector("#page2>h1").innerHTML = clutter;

// Animate the words with GSAP
gsap.to("#page2>h1>span", {
  ScrollTrigger: {
    trigger: "#page2>h1>span",
    start: "top bottom",
    end: "bottom top",
    scroller: "#main",
    scrub: 0.5
  },
  stagger: 0.2,
});
```

## Usage
- Include the scripts and styles in your HTML.
- Make sure your main content is wrapped inside `#main`.
- Add an `h1` inside `#page2` to see the text animation effect.
- Run the project in a local development server to see smooth scrolling in action.

## Troubleshooting
- **Scroll not smooth?** Ensure Locomotive Scroll is correctly initialized on `#main`.
- **Animations not triggering?** Check that GSAP ScrollTrigger has the correct `scroller` reference (`#main`).
- **Text not animating?** Ensure the JavaScript modifying `#page2>h1` executes properly.

## License
This project is open-source and free to use under the MIT license.

