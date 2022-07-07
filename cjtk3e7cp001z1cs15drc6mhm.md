## DRY HTML or DRY CSS? 🤯

Till now I have seen and worked on many projects and one term that I came across frequently is **DRY** i.e. Don't Repeat Yourself in code.

Let's take a look at **what** is DRY and **why** is it important.

## 🔬 What

As the abbreviation suggests, Don't repeat yourself. Functionality should be written once and responsible for just one thing. This way you will have reusable blocks with you to use anywhere. We use `functions` or OOP most of the time to achieve DRY knowingly/unknowingly.

---

## 🤔 Why

Reason to keep the code DRY is the idea of Single Source of Truth and Single point of Action.

Consider an example of Bank which has basic feature of Interest on some money. There are many kinds of deposits and loans which rely on this functionality. If this feature was written separately at every single place, it would be a nightmare to manage such code. And hence there are functions as a basic solution and Object as more advanced ones.

For another example, if you have a basic site and you have a header and footer for that site. You keep copy pasting it on every page, it would accumulate a lot of code which is the same. Hence we put header and footer in separate files and include those files where ever necessary. This will save code and a lot of time. Changes will be easier.

----

## 🤓 DRY in Front End

Front End; the field which is always under the question of ***what's more efficient***. 

But why this question? I think that because most of the things/tasks don't have just one single way to achieve. There are many ways which will work; correct or incorrect. And if questions about code quality are not asked, they will pile up as a mess.

The primary parts of Front End that come under scrutiny are HTML and CSS.

You can write HTML and CSS with the utmost freedom and still both will not complain. And even if something is wrong, both will ignore the errors and move on.

### DRY in HTML

To keep HTML DRY, you can follow the component approach and keep them separate. And each file should be responsible for only one component.

Rarely, it would happen that you are going to code a static website with code repetition. Most of the times it will be a Server Side Language rendering HTML or a Single Page Application.

In case of both Server Side or Client Side HTML generation, keeping one component per file will keep the HTML DRY. But sometimes you can't ignore the repetition and have to go on with that.

### DRY in CSS

In my opinion, to keep CSS clean and DRY, following design semantics will be the best idea. There are different approaches to keep CSS sane i.e. modular and maintainable like BEM (Block Element Modifier), OOCSS (Object Oriented CSS), Atomic CSS etc.

BEM, OOCSS, ACSS and similar methodologies will provide a way to create CSS without being dependent on the structure or content very much.

You can briefly read about the comparison among them at http://clubmate.fi/oocss-acss-bem-smacss-what-are-they-what-should-i-use/

### HTML + CSS

Well for the above ways are good if they are taken into consideration separately. But in my opinion, if you are in HTML and CSS is DRY, you need to know details about the CSS and vice versa.

So if there is always something to know, why not mix them. Little details about HTML and little details about CSS will make it a bit easier.

As if we follow one directory and file per component, we can limit CSS in a similar way; one CSS file per component.

And in that CSS file, you can keep CSS semi-DRY and this will relax the HTML as well keeping it semi-DRY.

Let's take an example; component for the figure which houses image and caption as its children. And as per mock-ups, the figure should look like as follows:

![figure-design-mockup](https://res.cloudinary.com/time2hack/image/upload/q_auto:good/figure-design-mockup.png)

And in the design, it should look like as follows:

![figure-design-budapest](https://res.cloudinary.com/time2hack/image/upload/q_auto:good/figure-design-budapest.jpg)

![figure-design-copenhagen](https://res.cloudinary.com/time2hack/image/upload/q_auto:good/figure-design-copenhagen.jpg)

And for code, we can go ahead in following way:

```html
<figure class="t2h-figure">
  <img src="https://images.unsplash.com/photo-1521014186850-f8104766c605?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=900&fit=max&ixid=eyJhcHBfaWQiOjExNzczfQ&s=6cf7c906f1b0f3c993aa9889508b6c86" />
  <figcaption>
    Budapest
  </figcaption>
</figure>

<figure class="t2h-figure">
  <img src="https://images.unsplash.com/photo-1514553808637-6cda003eb4ab?ixlib=rb-0.3.5&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=900&fit=max&ixid=eyJhcHBfaWQiOjExNzczfQ&s=6cf7c906f1b0f3c993aa9889508b6c86" />
  <figcaption>
    Copenhagen
  </figcaption>
</figure>
```
And CSS (PostCSS) for above will be:
```css
@use postcss-cssnext;

:root {
  --text-color: #fff;
  --max-width-figure: 700px;
}

.t2h-figure {
  margin: 1em;
  position: relative;
  display: inline-block;
  max-width: var(--max-width-figure);
  color: var(--text-color);
  text-align: center;
  
  &:after {
    content: '';
    clear: both;
  }
  & img {
    width: 100%;
    display: block;
  }
  & figcaption {
    width: 60%;
    position: absolute;
    top: 50%;
    left: 50%;
    padding: 1em;
    font-size: 2.5em;
    font-weight: 800;
    letter-spacing: 0.2em;
    font-family: arial;
    text-transform: uppercase;
    border: 4px solid var(--text-color);
    transform: translateY(-50%) translateX(-50%);
  } 
}
```
As you can see above, only information HTML needs from CSS is the className `.t2h-figure` and rest goes with general markup structure for figure. And CSS just needs to be aware of genera markup structure.

This code can be improved in many ways, although the point was to show an example.

https://codepen.io/pankajpatel/embed/zaNaQY/?height=548&theme-id=20316&default-tab=result&embed-version=2

P.S. you can read more about PostCSS and CSSNext here: 
- [PostCSS: Shiny CSS PreProcessor written in JavaScript 🚀](https://time2hack.com/2018/03/postcss-css-preprocessor-in-javascript/)
- [CSSNext: TurboCharge your CSS Development 🏎](https://time2hack.com/2018/04/postcss-cssnext-turbocharge-css-development/)


---

## Conclusion 💪

With DRY, primary idea is to keep the code clean and maintainable. Above is my idea and approach to DRY; what do you think about it? 

Let me know your thoughts and opinions about this article through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/patel_pankaj_)  and  [@time2hack](https://twitter.com/time2hack) .

If you find this article useful, share it with others 🗣
