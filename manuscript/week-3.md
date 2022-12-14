# Week 3: Mobile First - Creative Agency

Congrats on completing the first two weeks of designs, we're finally in week three! During this week, we will be implementing a work from designer Wildan [Creative Agency](https://dribbble.com/shots/16478383-Creative-Agency-Responsive-Design). The reason why this mockup was chosen is not only that it uses a typical and widely used layout (so you can learn how to do a typical layout on your own), but also because it is a responsive design.

Responsive design is common in today’s websites, and people tend to browse the web on mobile devices rather than desktops. In mobile devices, we need to display more important information on a smaller screen, so we need to carefully analyze and design in terms of typography, visible elements and layout.

![Design By Wildan](week-3/mockup.png)

We will use a mobile-first approach here, that is, first consider how to layout on a smaller screen, and  increase the screen size gradually into a wider screen. There will be some layout changes during the process. [Kevin Powell](https://www.kevinpowell.co/) once introduced a practical mobile-first strategy in his video. If you haven't seen it, it is highly recommended that you learn and use that strategy.

Without further ado let's get started.

## Deconstruct the mockup

As Kevin explained in [his video](https://www.youtube.com/watch?v=bn-DQCifeQQ), we should start with the HTML for the desktop version, because usually the desktop version contains more elements and content. After completing the HTML document, you can start writing the mobile version of the CSS, and finally override some (usually only a small amount) of CSS rules for the larger screens.

Just as what we have done in the previous two weeks, we can start with the navigation bar (assuming you have created the `week-3` directory and a directory structure similar to the previous two projects):

```html
<header>
  <nav>
    <ul class="nav-list">
      <li>
        <span class="material-symbols-outlined">
        browse_gallery
        </span>
      </li>
      <li>Stella Studio</li>
      <li class="menu-item"><a href="#">Home</a></li>
      <li class="menu-item"><a href="#">Services</a></li>
      <li class="menu-item"><a href="#">Product</a></li>
      <li class="menu-item"><a href="#">Price</a></li>
      <li>
        <div class="menu">
          <span class="material-symbols-outlined">
            menu
          </span>
        </div>
      </li>
    </ul>
  </nav>
</header>
```

### Emmet Plugin

Allow me to pause here for a moment and introduce the use of the `Emmet` extension. `Emmet` is a built-in extension for Visual Studio Code that can be used to generate HTML code efficiently. You may have found that there are a lot of keystrokes in HTML writing, which is a common problem of all markup languages. For example, after you enter `<ul>`, you need to use `</ul>` to close the tag accordingly. Also, if the structure you're typing has a lot of repetition, you'll need a lot of extra keystrokes.

`Emmet` was born to solve this problem. In `Emmet`, you can use expressions to generate HTML, its expressions are somewhat similar to CSS selectors, for example, we want to create an `article` tag, and the CSS class of this `article` is `.post`. Then you only need to enter `article.post`, then press the `tab` key, and `Emmet` will automatically generate the corresponding HTML:

```html
<article class="post"></article>
```

If we want this `article` to contain an `h1` and two adjacent paragraphs `p` (with CSS class `content`), the corresponding expression is:

```
article.post>h1+p.content*2
```

And `>` here means direct child as we mentioned in last chapter. After pressing the `tab` key we can get the generated HTML:

```html
<article class="post">
  <h1></h1>
  <p class="content"></p>
  <p class="content"></p>
</article>
```

In addition, the extension also places the cursor in the tag of `h1` for you to type the content. After `h1` is completed, press the `tab` key again, and the cursor will jump to the first `p`. `tab` goes to the second paragraph.

Pretty awesome, right? It is much faster than typing HTML "manually". I strongly recommend that you take a little time to familiarize yourself with the syntax and common usage of `Emmet`, after which you can easily write expressions and generate HTML fragments with almost no thought.

For example, in the page `header` above, the corresponding `Emmet` expression can be described as:

```
header>nav>ul.nav-list>(li>a[href="#"])*7
```

Note that the parentheses `(li>a[href="#"])` have higher priority here, it will generate 7 `li`s, each `li` has a link, where the `href` attribute of the link is `#`. Without this parenthesis, it means that seven hyperlinks are generated in one `li` (obviously this is not what we wanted).

### Simulate Mobile Device in Chrome

Well, that concludes the introduction to `Emmet`, here is a [cheat sheet](https://docs.emmet.io/cheat-sheet/) you may want to print out and post on your desk. Now we can continue on the page. 

To simulate a mobile device, we can enable the browser DevTool emulator:

![Chrome DevTool](week-3/chrome-dev-tool.png)

At this point the browser window looks like this:

![Chrome with iPhone Simulation](week-3/chrome-simulator.png)

Let's start by doing some resets in CSS, like removing the default `margin` and `padding` for all elements, and more importantly, choosing a font that is similar to the design. Again, usually we can do some research on [Google Fonts](https://fonts.google.com), here I use `Roboto` sans-serif font as it looks close to the design.

There are two ways to use an external font, we can use `@import` to import the font in the CSS file:

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700&display=swap');

* {
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Roboto', sans-serif;
}

li {
  list-style: none;
}
```

The page looks neat with the new font and resets. Next is the `flex` layout we are best at, as well as some regular typography rules (adding inner and outer spacing to make the page and design as close as possible):

```css
.nav-list {
  display: flex;
  gap: 1rem;
  padding: 1rem 0;
  align-items: center;
}

.menu-item {
  display: none;
}

.nav-list li:last-child {
  margin-left: auto;
}

.menu {
  width: 1.5rem;
  height: 1.5rem;
  border: 1px solid black;
}
```

So we get a navbar that looks very close to the design, notice that on mobile pages we hide all the links on the menu (via `display: none`):

![Navigation on Mobile](week-3/navigation-mobile.png)

Next is the banner area. Except for the buttons on the left and the right, this part is very simple. Just write the corresponding HTML in order. This time, you can try our new learnt `Emmet` expression:

```
.hero>h1+.hero-actions>.buttons+.categories>span*3
```

This code can generate the corresponding HTML structure, and we just fill out the blanks:

```html
<div class="hero">
  <h1>Enabling brands to flow with change in order to grow 🚀</h1>

  <div class="hero-actions">
    <div class="buttons">
      <button class="button-primary">Get started</button>
      <button class="button-secondary">Get quote</button>
    </div>
    <div class="categories">
      <span>/Web Design</span>
      <span>/Branding</span>
      <span>/Marketing</span>
    </div>
  </div>
</div>
```

I believe you can easily use `flex` to layout these two buttons, right?

```css
.hero {
  margin: 1rem 0;
}

.hero h1 {
  font-weight: 400;
}

.buttons {
  margin: 2rem 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

button {
  border-radius: 2rem;
  appearance: none;
  border: none;
  padding: 1rem 2rem;
  text-transform: capitalize;
  font-weight: 700;;
}

.button-primary {
  background-color: #192026;
  color: white;
}

.button-secondary {
  background-color: #E0E6E6;
  color:black;
}
```

### CSS Variables

I need to pause again for a little while and see how to get rid of hardcoding like `#192026` and `#E0E6E6`. You must have noticed in the past two weeks that there are many color values ​​that will continue to appear in our CSS. 

In design, this repeatation is critical for brand coherence, and a company often has some dedicated color combinations, such as Coca-Cola's red, IKEA's blue-yellow, and so on.

However, in the code, we don't like the repeatation that much. A good practice is to define these colors in a common place, and reference them in other places where they are used. We can do this through CSS variables in our CSS -- it wasn't so popular just a few yeas back.

CSS variables are usually defined at the root of a document, it starts with double hypens (like `--primary-color`), and then you can reference them via `var` keyword:

```css
:root {
  --color-primary: #192026;
  --color-secondary: #E0E6E6;
}
```

When using, simply put the variable name in a `var()`:

```css
.button-primary {
  background-color: var(--color-primary);
  color: white;
}

.button-secondary {
  background-color: var(--color-secondary);
  color:black;
}
```

The advantage of this is that we only need to modify one place when necessary, all the places will be updated. A common usage scenario is dark mode. For example, at night or when the light is dim, we want the entire page to be dark and we definitely don't wan to change all the font color or background color of sections. We only need to modify CSS variables, and other page elements will be updated automatically.

```css
.categories {
  margin: 3rem 0;
  font-size: 12px;
}

.categories span {
  padding-right: 1rem;
  text-transform: uppercase;
  color: var(--color-secondary);
}

.categories span:last-child {
  padding-right: 0;
}
```

Then we update the category list to complete the hero banner on mobile:

![Banner on Mobile](week-3/hero-banner-mobile.png)

Next is a more complex area, which is very different between the mobile and desktop versions. The mobile design is rendered in sequence, while the desktop version presents a left-right split-screen layout due to it has more available space. But for HTML, the two are no different -- remember that HTML is only about the content while CSS does the presenting part?

```html
<div class="details">
  <article>
    <h2>We propel results-driven growth</h2>
    <p><!-- the long text --></p>
    <button>View details</button>
  </article>

  <article class="author">
    <div class="info">
      <div class="avatar">
        <img src="assets/founder.png" alt="founder">
      </div>
      <div>
        <h4>Juntao Qiu</h4>
        <p>Founder</p>
      </div>
    </div>
    <div class="image-container">
      <img src="assets/sprial.jpg" alt="sparel growth">
    </div>
  </article>
</div>
```

Note that here we can use a shortcut key of `Emmet` to generate placeholder text. For example, enter `lorem30` and press `tab`, `Emmet` will automatically generate a random paragraph of 30 words, which is useful when we are marking up the page content.

And if you're curious, here is what `Lorem ipsum` is according to [wikipedia](https://en.wikipedia.org/wiki/Lorem_ipsum):

> In publishing and graphic design, **Lorem ipsum** is a placeholder text commonly used to demonstrate the visual form of a document or a typeface without relying on meaningful content.

Here's the effect of my typing `lorem8`, which even generates punctuation to help with our initial typography check:

```
Lorem ipsum dolor sit amet, consectetur adipisicing elit.
```

Next, let's complete the typesetting of the article:

```css
.details {
  padding: 2rem 10% 6rem 10%;
  background-color: var(--color-primary);
  color: white;
}

.details button {
  width: 100%;
  margin: 2rem 0;
}

.details h2 {
  margin-bottom: 1rem;
  font-weight: 400;
  line-height: 1.5;
}

.details p {
  font-weight: 200;
  line-height: 1.5;
}
```

Looking closely, we can see that the image of the article is covered in the article area, which means that we need `absolute` positioning here:

```css
.author {
  position: relative;
}

.author .info {
  display: flex;
}

.author .info div:last-child {
  margin-left: 1rem;
}

.author .info .avatar {
  width: 2rem;
  height: 2rem;
}

.avatar img {
  width: 100%;
  border-radius: 2rem;
}

.author .image-container {
  width: 100%;
  position: absolute;
}

.author .image-container img {
  width: 100%;
}
```

This gives the layout of the article section on the mobile:

![Mobile Layout Done](week-3/article-mobile.png)

## Desktop version

At this time, if we open this page with a desktop browser, we will find that the page is more or less usable (except for the disappearing menu), we can click buttons, read article, etc. In general, it is an at least functional. 

And that's why we emphasize mobile first. If it's the other way around, a lot of times on mobile devices there will be a lot of scroll bars and sometimes the font size is either too large or too small to read.

Obviously, we need to modify some layout rules for desktop browsers. So how does CSS know if the page is on a mobile device or a desktop monitor? The answer is a media query expression. To put it simply, CSS is designed to be applied to a variety of media, and the screen device is just one of them, in addition to the printing device, the screenreader, etc. We can also apply certain rules independently for landscape/portrait or specified width screens.

### Media query

In CSS, we can use a syntax like this to detect device characteristics and use a specific set of rules on demand:

```css
@media only screen and (max-width: 800px) {
  body {
    font-size: 24px;
  }
}
```

The above statement means that when the device that opens the page is a screen device (different from printers and screenreaders), and the screen size is less than or equal to `800` pixels, set the font size to `24` pixels.

In addition, we can combine multiple conditions together:

```css
@media (min-width: 600px) and (max-width: 1000px) {
  .container {
    display: flex;
    align-items: center;
  }
}
```

With this feature, based on the mobile page we just completed, we can display menu items at the right screen size:

```css
@media (min-width: 800px) {
  h1 {
    font-size: 4rem;
  }

  h2 {
    font-size: 3rem;
  }

  .menu-item {
    display: block;
  }

  .menu-item a {
    text-decoration: none;
    color: var(--color-primary);
  }
}
```

With the above media query, we define that when the browser window is larger than 800 pixels (this is enough reason to believe that the page is opened on the desktop), we display `menu-item`, and the font size of the title is appropriately enlarged a bit.

![Menu on Desktop](week-3/menu-desktop.png)

For the buttons layout, we also need to override some styles:

```css
@media (min-width: 800px) {
  .hero-actions {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .categories {
    margin-left: auto;
  }

  .buttons {
    justify-content: flex-start;
    gap: 0.5rem;
  }
}
```

![Hero Section on Desktop](week-3/hero-banner-desktop.png)

Finally, we need to adjust the layout of the article section so that it has a left-right layout:

```css
@media (min-width: 800px) {
  .details {
    display: flex;
    flex-direction: row-reverse;
    padding: 2rem 10%;
  }

  .details article {
    flex: 1;
  }

  .details article:first-child {
    padding: 0 2rem;
  }

  .image-container {
    width: 80%;
    top: 10%;
  }
}
```

This will give you the correct layout for the desktop version:

![Desktop Layout Final](week-3/page-desktop.png)

Now, you can publish your work through `Netilify` or `Surge`, and then visit it on other devices (and you can also invite friends too) to see how the page performs on mobile phones.

Let's take a break here and take a more relaxed look at some of the great works we've achieved closely.

## Design principles

So far, we have implemented three pages from scratch, and we have learned from a very close distance how designers use simple elements such as fonts, sizes, shapes, colors, white space, etc., to make a page vivid and can be delivered efficiently information behind it. So, what makes a design more **aesthetic** than others? What are the principles behind this that we can learn from as web developers?

The first thing to note is that design something "beautiful" is a skill you can learn. Like other skills, it can be learned through some trainings and following proven principles. There are already many excellent designers who have summed up basic patterns and principles.

### Characteristics of good design

When we talk about a good design, it should have at least the following characteristics:

1. Intuitive, it can accurately convey design intent
1. Easy to use
1. Simple, there is no extra thinking required
1. Beautiful and pleasant to use

In fact, the best design is to make the user not feel the existence of the design itself, that is, the interface of human-computer interaction is integrated into the daily habits. 

Many times, users can only realize the existence of a certain design when it is bad: for example, you searched the whole page but could't find the button to resend the email; after entering a deep level, you found that you could't go back; mouse hover links that didn't have a visual hint of clicable and so on.

We're here to learn a few simple and easy-to-use principles: **Alignment**, **Contrast**, **Repetition**, and **Proximity**. In addition, we should also pay attention to the principles of **Balance** (the distribution of elements on the page), **White space** (for the use of negative space, so that there is enough space between elements).

#### Alignment

Alignment is the easiest to do and the easiest to see the effect of. In fact, if you look carefully enough, you will find that this principle exists in any designs.

For example, this is a slide I made a while ago on `UNIX philosophy`:

![UNIX Philosophy](week-3/alignment-origin.png)

In this design, all text follows some invisible lines:

![The Invisible Lines](week-3/alignment-desc.png)

If pictures, texts, etc. are not arranged neatly, users need to spend some energy to process them in their brains, which invisibly increases the difficulty of use. Of course, not all situations require strict alignment, and once you've mastered the basic principles of design, you can break these rules when appropriate.

#### Contrast

To contrast is to distinguish different things. For example, the distinction between the foreground color and the background color, the distinction between the title and the text, and so on. When using contrast, it must be strong enough, such as using complementary colors in text, two completely different fonts to reflect differences, etc.

![Contrast](week-3/contrast-origin-correct.png)

In the image above, we can see that the orange-red color of the text contrasts sharply with the gray background. The text size and color change between the title and subtitle can also make readers feel the difference between the two. Finally, in the subtitle, we use italics to highlight the keyword `command line`.

Contrast can have many forms in design, some common methods are as follows:

- Use different colors
- Use different types of fonts
- Different font size
- Weight of the font (bold, regular, thin)
- Space and negative space

#### Proximity

People have a habit of grouping things that are closer together, or at least thinking they are related. For example, when we search in Google, the search result is like:

![Google Search Results](week-3/google-search-result.png)

We can actually easily determine which text belongs to the first item and which belongs to the second item by the amount of space between the text. And if we analyze the example of the `UNIX philosophy` again, we can also see the use of the principle of proximity:

![Proximity](week-3/proximity.png)

This principle is very important, and we will use it a lot in our own designs. For example, the size of the space between a large piece of information and another, the size of the space between the title and the text, the distance between the picture and its description text, and so on. 

Using the principle of proximity can save readers a lot of time, which in turn makes our designs more meaningful (and intuitive). Imagine this scenario: You are browsing a long list on your phone, each item in the list contains an image and corresponding description text, and you have now browsed to the 7th, but since all the spacing is the same, you can't tell whether the description text you see now is a description of the previous image or the next one.

#### Repetition

Repetition, that is, using a pattern over and over again. In the design, there are many places where repetition is used, such as the highlight color of a site, the style of buttons/links. There is a lot of repetition on the same page, and within a whole web site.

The above example also includes the principle of repetition, such as the three `UNIX` philosophies listed separately, each entry contains a large number, then a main title and a subtitle, and the font of the main title and subtitle is different. 

This repetition creates a rhythm that is not surprising at any point when someone read the design. Even if when on their first visit, after learning or understanding the first pattern, what follows is nothing more than the repetition of the first pattern.

For example, here is another slide from the `UNIX philosphies`, I just used the same font and color to create the repetition:

![Repeatation](week-3/contrast-origin-correct.png)

You can immediately know that they come from the same design. If we use other fonts, or different colors, that breaks the repetition. It takes a little time for users to get used to this adjustment and realize the existence of our design. Note, **Our goal is to make the design invisible to the user**.

### How to organize content

After understanding these foundamental principles, let's take a look at how to use them in design.

#### Hierarchy in typing

The same content, the information conveyed in different formats can make a huge difference, we have a simple example here.

Suppose we make a class schedule for "3 Weeks 3 Pages". When using the browser's default style, the page looks like this:

![Without Hierarchy](week-3/without-hierachy.png)

As you can see, the course name, time, and course description information are mixed together, and the reader needs to spend some effort (though it may only take a few seconds) to find out the most important parts.

But if we use different font weight for information with different importance, the results will be more usable:

![With Hierarchy](week-3/with-hierachy.png)

Obviously, the revised figure is more understandable, and the reader does not need to spend too much thinking to obtain important information (title and time).

#### Colors

Colors often affect the user's emotions, such as red for passion, enthusiasm (sun, flowers, etc.), blue for quietness and peace (blue sky, the color of the sea). Therefore, the selection of colors also needs to be carefully analyzed and studied. For example, if you're designing a site for a restaurant, red might be a good choice, but if you're designing for an airline or a security company, you may need to consider blue or green.

A common misconception for beginners is to use two much colors to compose their color scheme. In fact, `Marketo` conducted a survey of companies in various industries on brand color selection, and the results were interesting: 95% of companies used only 2 colors or less. The 4 most used colors are: blue, red, black/gray, yellow/gold.

When you're designing your own, if you're not sure which color to use, try `blue`. After choosing the main color, you need to determine the text color and background color. The choice of text color and background color can be completely independent of the main color. The only rule is to guarantee legibility (and readability).

In fact, the theory and application of color is totally worth a book, and we won't discuss it too much here. You can find out more in [this article](https://icodeit.org/2021/08/colour-theory-for-dev-part-1/) and [this article](https://icodeit.org/2021 /08/colour-theory-for-dev-part-2/) for some deeper insight about colours.

## Summarize

In this chapter, we learned to use `Emmet` to simplify the writing of HTML, and with a little practice you can improve the efficiency several times. Then we explored how to use external fonts on Google Fonts to make the whole design more professional. It is worth noting that many businesses have their own font requirements, and you may need to confirm with the designer which font should be used on a website. Then we learned to use CSS variables to eliminate duplication, thus making our code cleaner and easier to maintain. Next we learned how to use media queries to make pages responsive: different appearances on different devices.

Finally, we discussed some common visual design principles you might want to try applying to your own pages.

Next, I hope you can use what you've learned to do some fun exercises.

## Challenges

### Challenge 1

You've written a lot of HTML and CSS in the first two chapters, and there are a lot of duplications in them. I hope you can use CSS variables to eliminate duplication of color, layout, etc. in your code. Also, if you need to write new HTML snippets, try `Emmet` to generate HTML instead of do it manually. 

Also, our designs in the first two chapters are based on the desktop. You can try using `media query` to hide certain elements on mobile devices, or change the layout of certain elements to make them more convenient on mobile devices.

### Challenge 2 and the Final one

With design principles in mind, try to design something you are interested in. Your hobby can be a good start, hiking, movie, books or cooking, and you name it. Once you have picked the theme, try doing some research and develop a design - timeline, card, hero area, news list or whatever you like. If you're stuck, try dribbble or behance for some inspiration.  

And the most beautiful part comes, you will need to use HTML and CSS to implement the design, publish it on `Surge` or `Netlify`, and share it with me maybe?

Unless you're very lucky, you're bound to encounter a lot of difficulties, but don't worry, using the knowledge in this book, at least you won't be overwhelmed, you already know where to go, and the rest is some technical details. Even though these details might take some time, it's part of the learning, and I'm sure you'll be able to do it eventually.

## What's next?

You have learned the basic development process and development methods, if you encounter new unfamiliar CSS properties or new HTML tags, you can visit [MDN](https://developer.mozilla.org/en-US /) to find the latest and most detailed guidance and explanations. 

If you are not sure about the level of support of some new properties, you can confirm their coverage on [caniuse](https://caniuse.com/).
