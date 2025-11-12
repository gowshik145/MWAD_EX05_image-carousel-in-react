# MWAD_EX05_image-carousel-in-react

## Reg No: 212223220026
## Name: Gowshik S
## Date:

## AIM
To create a Image Carousel using React 

## ALGORITHM
### STEP 1 Initial Setup:
Input: A list of images to display in the carousel.

Output: A component displaying the images with navigation controls (e.g., next/previous buttons).

### Step 2 State Management:
Use a state variable (currentIndex) to track the index of the current image displayed.

The carousel starts with the first image, so initialize currentIndex to 0.

### Step 3 Navigation Controls:
Next Image: When the "Next" button is clicked, increment currentIndex.

If currentIndex is at the end of the image list (last image), loop back to the first image using modulo:
currentIndex = (currentIndex + 1) % images.length;

Previous Image: When the "Previous" button is clicked, decrement currentIndex.

If currentIndex is at the beginning (first image), loop back to the last image:
currentIndex = (currentIndex - 1 + images.length) % images.length;

### Step 4 Displaying the Image:
The currentIndex determines which image is displayed.

Using the currentIndex, display the corresponding image from the images list.

### Step 5 Auto-Rotation:
Set an interval to automatically change the image after a set amount of time (e.g., 3 seconds).

Use setInterval to call the nextImage() function at regular intervals.

Clean up the interval when the component unmounts using clearInterval to prevent memory leaks.

## PROGRAM
# APP.JSX
```

import React from 'react';
import Carousel from './Carousel';

const images = [
  { src: process.env.PUBLIC_URL + '/images/mountain.jpg', alt: 'Mountain sunrise' },
  { src: process.env.PUBLIC_URL + '/images/city.jpg', alt: 'City skyline' },
  { src: process.env.PUBLIC_URL + '/images/img3.svg', alt: 'Abstract waves' },
];

export default function App(){
  return (
    <div className="app">
      <h1>React Image Carousel (Hooks) BY Gowshik s 212223220026</h1>
      <Carousel images={images} autoplayInterval={3000} />
    </div>
  );
}
```
# COROUSEL.JSX
```

import React, { useState, useEffect, useRef } from 'react';

export default function Carousel({ images = [], autoplayInterval = 4000 }) {
  const [index, setIndex] = useState(0);
  const [isPlaying, setIsPlaying] = useState(Boolean(autoplayInterval));
  const timeoutRef = useRef(null);
  const containerRef = useRef(null);

  const next = () => setIndex((i) => (i + 1) % images.length);
  const prev = () => setIndex((i) => (i - 1 + images.length) % images.length);
  const goTo = (i) => setIndex(i);

  useEffect(() => {
    if (!isPlaying || images.length <= 1) return;
    timeoutRef.current = setTimeout(() => {
      next();
    }, autoplayInterval);
    return () => clearTimeout(timeoutRef.current);
  }, [index, isPlaying, images.length, autoplayInterval]);

  useEffect(() => {
    const onKey = (e) => {
      if (e.key === 'ArrowRight') next();
      if (e.key === 'ArrowLeft') prev();
    };
    window.addEventListener('keydown', onKey);
    return () => window.removeEventListener('keydown', onKey);
  }, [images.length]);

  // Pause on hover
  const handleMouseEnter = () => setIsPlaying(false);
  const handleMouseLeave = () => setIsPlaying(Boolean(autoplayInterval));

  // Basic swipe support
  useEffect(() => {
    const node = containerRef.current;
    if (!node) return;
    let startX = 0, endX = 0;
    const onTouchStart = (e) => { startX = e.touches[0].clientX; };
    const onTouchMove = (e) => { endX = e.touches[0].clientX; };
    const onTouchEnd = () => {
      const dx = endX - startX;
      if (Math.abs(dx) > 40) {
        if (dx < 0) next();
        else prev();
      }
    };
    node.addEventListener('touchstart', onTouchStart, { passive: true });
    node.addEventListener('touchmove', onTouchMove, { passive: true });
    node.addEventListener('touchend', onTouchEnd);
    return () => {
      node.removeEventListener('touchstart', onTouchStart);
      node.removeEventListener('touchmove', onTouchMove);
      node.removeEventListener('touchend', onTouchEnd);
    };
  }, [images.length]);

  if (!images || images.length === 0) return <div className="carousel empty">No images provided</div>;

  return (
    <div
      className="carousel"
      ref={containerRef}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
      aria-roledescription="carousel"
    >
      <div className="carousel-track" style={{ transform: `translateX(-${index * 100}%)` }}>
        {images.map((img, i) => (
          <div key={i} className="carousel-slide" role="group" aria-roledescription="slide" aria-label={img.alt}>
            <img src={img.src} alt={img.alt} draggable="false" />
          </div>
        ))}
      </div>

      <button className="carousel-btn prev" onClick={prev} aria-label="Previous slide">‹</button>
      <button className="carousel-btn next" onClick={next} aria-label="Next slide">›</button>

      <div className="carousel-dots" role="tablist" aria-label="Slide dots">
        {images.map((_, i) => (
          <button
            key={i}
            className={'dot' + (i === index ? ' active' : '')}
            onClick={() => goTo(i)}
            aria-label={`Go to slide ${i + 1}`}
            aria-selected={i === index}
            role="tab"
          />
        ))}
      </div>

      <div className="carousel-controls">
        <button onClick={() => setIsPlaying((p) => !p)} aria-pressed={!isPlaying}>
          {isPlaying ? 'Pause' : 'Play'}
        </button>
      </div>
    </div>
  );
}

```

## OUTPUT
<img width="1912" height="1018" alt="image" src="https://github.com/user-attachments/assets/99b57f6b-c535-4177-aefe-16b192197be8" />
<img width="1907" height="1014" alt="image" src="https://github.com/user-attachments/assets/18c0b36f-2be3-4907-8dfc-748951aba260" />

## RESULT
The program for creating Image Carousel using React is executed successfully.
