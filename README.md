# Carousel React project

This is a solution to the #1 Project out of the [7 React Projects for Beginners in 2023](https://www.freecodecamp.org/news/react-projects-for-beginners-easy-ideas-with-code/). The carousel should allow the user to click the backwards or forwards button to go to the previous or next image.

## !! Don't forget to
- [ ] complete the remaining sections
- [ ] put an access link to the `sol` branch, so viewer can refer to it when reading the documentation
- [ ] indenting the documentation correctly
- [ ] correct spelling mistakes and phrases construction
- [ ] remove this section

## Table of contents

- [Overview](#overview)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [Installation](#installation)
- [Project building process](#build-buildng-process)
  - [Technologies](#technologies)
  - [Process](#process)
- [Solutions comparison](#solutions-comparison)
  - [Similarities](#similarities)
  - [Differences](#differences)
  - [Pros and Cons](#pros-and-cons)
- [What I learned](#what-i-learned)
  - [During development](#during-development)
  - [During solutions comparison](#during-solutions-comparison)
  - [After refactoring](#after-refactoring)
- [Continued development](#continued-development)
- [Useful resources](#useful-resources)
- [Author](#author)
- [Acknowledgments](#acknowledgments)

## Overview

### Screenshot

![](./screenshot.png)

### Links

**s-t-e-v** (challenger)
- Solution URL: [Github repo](https://github.com/s-t-e-v/carousel-react)
- Live Site URL: [Add live site URL here](https://your-live-site-url.com)

**Reed Barger** @freecodecamp.org (project poster)
- Solution URL: [Code sandbox](https://codesandbox.io/s/runtime-field-xp5sol?from-embed)

## Installation

If you want to run my solution on your pc, follow the instructions:

1. Open your terminal and then type
    ```bash
    git clone https://github.com/s-t-e-v/carousel-react
    ```
2. `cd` into the new folder and type
    ```bash
    npm install
    ```
3. To run the React project
    ```bash
    npm start
    ```

    
    **Credits**: [Sattwik Sahu](https://dev.to/swagwik) for explaing how to download & run react project locally [here](https://dev.to/equuscaballus/how-can-i-download-a-react-project-from-github-and-run-in-my-pc-eh3).



## Project building process

### Technologies

- Semantic HTML5 markup
- CSS custom properties
- Flexbox
- [React](https://react.dev/) - JS library

### Process
- The images will be stored in a simple array.
- `use state` should be used in order to store the current image. Updating the state will trigger going to the previous or next image, according to the button the user pressed.
- If the user has gone through all of the images, it should be allowed to cycle through the images again when clicking to the next/previous image buttons.
- **Concepts that should be implemented**:
  - useState (storing and updating state)
  - Conditionals (ternaries)
  - Lists, keys, and .map()


## Solutions comparison

### Similarities

1. Applying `export default` to a function called `App` :
```javascript
export default function App() {
    return (
        {/* code here */}
    );
}
```
2. Using an array to store image path information :
```javascript
const images = [
    "path 1",
    "path 2",
    "etc...",
];
```
3. Assigning to the left & right buttons of the carousel a class specific to both of them. E.g.:
```javascript
{<element className = "left-arrow"></element>}
```
4. Applying to `onClick` a function handling click event for the left & right buttons
5. Importing `useState`:
```javascript
import { useState } from "react";
```
6. Setting up the state of the carousel, being the index of the current image here:
```javascript
export default function App() {
    const [current, setCurrent] = useState(0);
    // ...
}
```

**General observations**:
Both of the solutions ...

### Differences

1. CSS stylesheets in my solution is imported in "index.js":

```javascript
index.js

// Some imports here...
import './style.css';
// rest of the file...
```
In the project poster solution, the stylesheet is imported in "App.js":
```javascript
App.js

import { useState } from "react";
import "./styles.css";
// ...
```
2. The array of image paths is declared outside of the `App` function in the project poster solution, whereas declared inside the `App` function in my solution.
3. In term of App structure,
   - the project poster has only one functional component: `App`.
   - In my solution, the `App` component is composed of a several sub functional components like `<Carousel/>`, `<Arrowleft>`, `<Arrowright>`. My solution is more 'decomposed'.
4. My solution is more verbose than the project poster solution:
    - My solution: *68 lines of code*
    - Project poster solution: *44 lines of code*
5. Click events handling:
    - My solution uses on function to handle click events. To manage clicks from the right & left buttons, I use `LEFT` and `RIGHT` constants and pass them as arguments through the `side` variable:
    ```javascript
    function handleClick(img, side) {
        // code here...
    }
      ```
    - The project poster  uses two functions to handle click events, `prevSlide()` and `nextSlide()`, for handling clicks of the left and right buttons respectively.
6. Click events handling - image index:
    - My solution passes `currentImg` value for the `img` argument. An anonymous function is used for this purpose:
    ```javascript

    function Carousel() {
        // currentImg useState & const images definition
        function handleClick(img, side) {
            // code here...
        }
        return (
            <div className="carousel">
                <Arrowleft onLeftClick={() => handleClick(currentImg, LEFT)}/>
                {/* rest of div content here...*/}
            </div>

        );
    }
    ```
    Besides, as we can see in the code above, the `handleClick` function is passed to the arrow components via `onLeftClick` and `onRightClick` props.
    - The project poster directly uses currentImg value with `handleClick` function, treating it as global variable. No need of `LEFT` or `RIGHT` constants since right and left buttons have their own click handlers. Hence, the `handleClick` functions here **does not require any arguments**:
    ```javascript
    function nextSlide() {
    // code using 'current' (aka current image index) directly
    }
    ```
7. Click events handling - onClick trigger:
    - My solution implements onClick event listenning within the arrow functional components. The event listening function is a props passed to the component. E.g.:
    ```javascript
    function Arrowleft({onLeftClick}) {
        return (
            <button className="arrow arrowleft" onClick={onLeftClick}></button>
            );
    }
    ```
    - The project poster implements onClick event listenning directly in the arrow components, hence **no need of passing a event handling function through props**. E.g.:
    ```javascript
    {<div className="left-arrow" onClick={prevSlide}>
          ⬅
        </div>}
    ```

8. Click events handling - algorithm:
    - My solution: I decided to use `switch` to handle the left and right cases. For each cases, I use if statements to increase or decrease the index of the images `img`. When the index is out of bounds, we set it to the upper or lower bound. This create a 'loop'. Then, I call the `setCurrentImg` state function to update the current image index. E.g.:
    ```javascript
    function handleClick(img, side) {
        switch(side) {
        case LEFT:
            if(img > 0)
                img--;
            else
                img = images.length - 1;
            break;
            // RIGHT & default cases...
        }

        setCurrentImg(img);
    }
    ```
    - Project poster solution: The left and right cases are handled in two separate functions.
    Ternary expressions are used to manage the index increase/decrease looping.
    `setCurrent` state function is directly called, with the ternary expression as argument. E.g.:
    ```javascript
    function prevSlide() {
        setCurrent(current === 0 ? images.length - 1 : current - 1);
    }
    ```

9. HTML:
    - My solution: use of `<button>` tag for the left and right arrows.
    - Project poster solution: use of `<div>` tag for the left and right arrows.
10. Image rerendering:
    - My solution: The slide image is contained in a div. The image source is set by the value of `images[currentImg]`:
        ```javascript
        {<img src={require("./img/" + images[currentImg])} alt=""/>}
        ```
        since `currentImg` changes when an arrow is clicked, the corresponding image file name contained in `images` array is returned. Hence, the right image is loaded.
    - Project poster solution: . The image is contain in a div like previously. The image supposed to be shown is via an iteration through the `images` array using the `.map()` method:
    ```javascript
    jsx

    {images.map(
        (image, index) =>
        current === index && (
            <div key={image} className="slide">
                <img src={image} alt="images" />
            </div>
        )
    )}
    ```
    Here, the short-circuit evaluation `current === index && (Image JSX)` is used to determine whether the image of the iteration should be shown or not. At the end, the array returned by the `.map()` method contains only one element: the image JSX of the current image index. The 'key' property of the div is needed to help react to discriminate the different div image containers so it can rerender the app correctly.

### Pros and cons

**My solution**
*Pros*:
- x
- y
- z

*Cons*:
- a
- b
- c

**Project poster solution**
*Pros*:
- x
- y
- z

*Cons*:
- a
- b
- c


## What I learned

### During development

- I learned how to set up  a react project:
```bash
npm init react-app my-app
```
- I learned the minimal structure of a React application:
```
project-name
    node_modules
    public
        favicon.ico
        index.html
        manifest.json
    src
        App.js
        index.js
        style.css
    package-lock.json
    package.json
```
- According to the compiler, a `switch` statement should always has a default case statement.
- In JSX notation, (and maybe in Js overall), `require` statement is needed to source an image:
```javascript
jsx

{<img src={require("image_path")} alt="" />}
```
  also, even if `alt` is empty, we must put it the image tag.
- I learned how to pass function from a parent component to a child component via props
- I learned how to use anonymous function to pass arguments to function used in child component
- I learnd using props
- I learned how to make the app dynamic thanks to state variables.
- Fragment element of the UI into functional components
- How to establish relationship between parent and child components, so change induce by child component affect the parent state

### During solutions comparison

- It makes more sense to define `images` array outside of the main function `App`.
- It is better to not fragmentate the code too much in functional components if the components of the application are not complex. Putting everything inside the main function in this case is the way to go. The code is more readable and easier to comprehend.
- I learned how to use short-circuit evaluation
- When used in functions, it is better to invoke state variables directly instead of passing them through arguments. E.g. the `img` argument in the `handleClick(img, side)` function is unnecessary
- Not forcing the DRY (Don't Repeat Yourself) approach. I wanted to make one function for right and left buttons click event handling. The differences between the two cases appeared to me slight. I fooled myself believing I respected the DRY principle by doing this. In reality, I ended up producing a code not that optimized and unnecessarily more complex. Coding two functions for each arrows is a simpler, more readable, efficient and straightforward approach.

### After refactoring

- nothing special

### Continued development

I would like to understand what is the best practice between using `require` for loading images or `.map` for preselecting images before loading them.

I would like to dive more into these:
- https://levelup.gitconnected.com/how-to-create-a-minimal-react-and-parcel-app-in-5-steps-2806fa09a371
- https://dev.to/egg_jose/how-to-create-a-react-app-without-create-react-app-command-1fgc
- deploy react app github: https://github.com/gitname/react-gh-pages


Use this section to outline areas that you want to continue focusing on in future projects. These could be concepts you're still not completely comfortable with or techniques you found useful that you want to refine and perfect.

## Useful resources

- [facebook/create-react-app Github](https://github.com/facebook/create-react-app) : This helped me to know how to set up a react application with npm commmand
- [Tic-tac-toe tutorial](https://react.dev/learn/tutorial-tic-tac-toe) : This helped to build a react application
- [How Can I Download a React Project from Github and Run in My PC](https://dev.to/equuscaballus/how-can-i-download-a-react-project-from-github-and-run-in-my-pc-eh3) : helped me to do the *run locally* section of the README and learn how to

## Author

- Website - [sbandaogo](https://sbandaogo.com)
- GitHub - [s-t-e-v](https://github.com/s-t-e-v)


## Acknowledgements

- Allah who made this and everything possible
- [ChatGPT](https://chat.openai.com) (and extensivelly OpenAI and the people who produced the data GPT trained on) which helped me:
    - to understand the map array part of the project poster solution.
    - to embark into the process of analyzing the solution
- [React.dev](https://react.dev) for their [tic-tac-toe tutorial](https://react.dev/learn/tutorial-tic-tac-toe) and their documentation
- [Reed Barger](https://www.freecodecamp.org/news/author/reed/), for providing these cool projects