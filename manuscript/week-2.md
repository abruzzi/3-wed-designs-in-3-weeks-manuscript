# Week 2: Social Media - New Instagram Interface

In the second week, we will work together to realize the concept design for Instagram posted by designer Neelesh Chaudhary on [Dribbble](https://dribbble.com/shots/15118067/attachments/6852395?mode=media). This design is a bit more complicated than the previous page, which involves a more complex `grid` layout, as well as a vertical navigation bar.

![Design by Neelesh Chaudhary](week-2/instagram.png)

But don't worry, I'll walk you through it step by step. Now, please prepare your environment and create a new directory structure similar to `week-1`, for example, call it `week-2`.

## Deconstruct the page

As before, the first step is to deconstruct the page. The navigation bar for this page is located on the left side of the page and is arranged vertically. In addition, the page can be roughly divided into three vertical blocks: the navigation bar, the content area, and the profile area.

```html
<div class="main">
  <header class="nav-container">
    header
  </header>
  <section class="posts-container">
    post list
  </section>
  <aside class="profile-container">
    profile
  </aside>
</div>
```

The navigation bar needs to be set to a fixed width, while the content area and personal information area need to be scaled proportionally in different window sizes. The ratio of content area and personal information is about 3:1.

We can first set `main` to a `flex` layout:

```css
.main {
  display: flex;
  align-items: center;
  height: 100vh;
}

.main > * {
  height: 100%;
}
```

The `vh` in `100vh` here is a CSS unit that represents a percentage of the viewport height. `100vh` means 100% of the viewport height, which is the height of the current browser window. If we zoom the browser, `main` will always take up the full height of the window. Similarly, there is a `vw` for viewport width too.

Then, we need to make sure that the width of the navbar is around 32 pixels, and we don't want it to change in size with the window. This can be achieved by setting `flex-grow` and `flex-shrink` to 0. For the remaining two blocks, we give them a 3:1 ratio in `flex-grow`, `flex-shrink` and `flex-basis`:

```css
.nav-container {
  border-right: 1px dashed red;
  flex: 0 0 2rem;
}

.posts-container {
  flex: 3 3 75%;
}

.profile-container {
  border-left: 1px dashed red;
  flex: 1 1 25%;
}
```

In addition, we added a right border to the `navigation bar` and a left border to the `personal information` for visualise (during development stage):

![Top Level Layout](week-2/sections-layout.png)

You may have noticed that we set `flex-basis` to `2rem` in the navbar, which is another new CSS unit that represents the numerical value of the font relative to the root element (that is, the html tag). For example, when the font of the root element is 16 pixels, `1rem` means 16 pixels. If the font of the root element is changed to 24 pixels, `1rem` means 24 pixels. The benefit of this approach is that when the user zooms in and out of the browser font, the page elements can be scaled proportionally.

### Implementing Navigation Bar

Similar to the process in the first week, let's implement the navigation bar first.

```html
<header class="nav-container">
  <nav>
    <ul class="nav-list">
      <li><a href="#">Home</a></li>
      <li><a href="#">Message</a></li>
      <li><a href="#">Link</a></li>
      <li><a href="#">Favorite</a></li>
      <li><a href="#">Video</a></li>
      <li><a href="#">TV</a></li>
      <li><a href="#">Bookmark</a></li>
    </ul>
  </nav>
</header>
```

Here we temporarily use text to replace the icon, and then do the replacement after the typesetting is completed.

```css
nav, .nav-list {
  height: 100%;
}

.nav-list {
  display: flex;
  flex-direction: column;
}

.nav-list li {
  flex: 1;
}
```

First of all, we need to set the height of `nav` and `ul` to 100%. Different from the width, we need to specify it explicitly, otherwise the height of the entire navigation bar is the sum of the heights of each item. The `flex-direction` here indicates that we want `flex` to be laid out in a column direction, by default `flex` is laid out in a row. This way we can use properties like `flex:1` on the children to spread them evenly in the column direction.

![Navigation HTML](week-2/navigation-html-layout.png)

### Using font icons

As you may have noticed, there are a lot of beautiful icons in the navigation bar. Typically, designers provide all icon assets along with the mockup. As we don't have these little nice icons, we can download some alternatives from [Google Fonts](https://fonts.google.com/icons) for demostration purpose.

![Google Fonts](week-2/google-fonts.png)

There are two ways to use Google's Material Symbols, one is through icon fonts, and the other is directly using `svg` files. We first show the more traditional svg file method here.

Download the required files to the `assets` subdirectory in the current directory, then replace the text in the HTML above with icons:

```html
<nav>
  <ul class="nav-list">
    <li><a href="#">
        <img src="assets/home.svg" alt="home">
      </a></li>
    <li><a href="#">
        <img src="assets/chat.svg" alt="chat">
      </a></li>
  <!-- other items -->
  </ul>
</nav>
```

After replacement we get a list of images:

![Icon List](week-2/navigation-icons-init.png)

We need to set the margins of the first and last flex elements to automatic `margin-top: auto`, so they fill the available space in the parent element, and set the width of `li` to `2rem`( i.e. 32px), and finally make sure the width of the image is `100%` to fill the available space.

```css
.nav-list li:first-child {
  margin-top: auto;
}

.nav-list li:last-child {
  margin-top: auto;
}

.nav-list li {
  list-style: none;
  width: 2rem;
  margin: 0 0.5rem 0.5rem 0.5rem;
}

li img {
  width: 100%;
}
```

This way we get the correct layout:

![Navigation Layout](week-2/navigation-icons-grey.png)

If we want to change the color of the svg image, we have to use the image editor to modify these icons one by one. And we have to repeat the process if there are new modifications in the future. But the good news is that we can use font icons to simplify this process.

Also on the Google Fonts site, we can import the font files corresponding to Material Symbols at the top of the HTML file:

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@48,400,0,0" />
```

Then use a `span` to replace the previous `img` element, note that the text in the span needs to correspond to the name of the icon:

```html
<span class="material-symbols-outlined">
favorite
</span>
```

Replace all icons with `span`:

```html
<header class="nav-container">
  <nav>
    <ul class="nav-list">
      <li><a href="#">
          <span class="material-symbols-outlined">
            home
          </span>
        </a>
      </li>
      <!-- the other icons -->
    </ul>
  </nav>
</header>
```

Then use the color picker to add the color of each icon to the corresponding icon:

```css
li a {
  text-decoration: none;
  color: black;
}

li:nth-child(1) a {
  color: #E6254A;
}

li:nth-child(2) a {
  color: #25CCF1;
}

li:nth-child(3) a {
  color: #EEC310;
}

/* other icons */
```

You now get a navigation bar that is closer to the design:

![Navigation With Colors](week-2/navigation-final.png)

### Implementing Content Area

Aside from not using actual icons, our navbar layout is pretty close to the mockup. Next, we implement the content area. Here we take a `top-to-bottom` approach, let's start with the search box at the top.

```html
<section class="posts-container">
  <header>
    <nav>
      <ul class="top-nav-list">
        <li>
          <a href="#">Instagram</a>
        </li>
        <li>
          <a href="#">
            <input type="text" placeholder="Search">
          </a>
        </li>
      </ul>
    </nav>
  </header>

  <!-- the posts -->
</section>
```

Note that here we use the `input` tag to define the input box. If the input box has no value, the string `Search` will be displayed to remind the user of the function of the input box. Obviously, we need to use flex layout to format `nav`, a technique we are already very familiar with:

```css
.top-nav-list {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 1rem;
  border-bottom: 1px dashed #eee;
}
```

Here we use `justify-content: space-between` to align the logo and the search box, and since the height of the search box and the logo may be different, we also use `align-items: center` to ensure that they are centered horizontally. 

For the details, we need to replace the default serif font with a new sans serif font, and then add rounded corners and a background color to the search box:

```css
body {
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}

.top-nav-list a {
  color: black;
}

.top-nav-list input {
  appearance: none;
  border: none;
  background-color: #F6F9FE;
  padding: 0.4rem;
  border-radius: 0.2rem;
}
```

Now we get a relatively good result:

![Top Navigation Bar](week-2/top-navigation.png)

Ahhh, wait a second, it seems we have a small problem here. Although the color of the `.top-nav-list a` link text is specified as black, it doesn't seem to take effect (it is still red). This is because we defined in the navigation bar above:

```css
li a {
  text-decoration: none;
  color: black;
}
```

The `.top-nav-list a` here has a lower priority and is overridden by the previous CSS rule. In CSS, this overriding is very common, and in fact is one of the reasons why CSS is so powerful. We usually only need to define CSS properties of child nodes, and other properties can be inherited from parent nodes or ancestors.

However, sometimes we want some rules to take precedence over others, in which case we need to be as precise and specific as possible. For example, `li a` takes precedence over `a` because the latter specifies `a` in `li`, and `.nav-list li a` takes precedence over `li a`.

To solve this problem above, we can make the rule `.top-nav-list a` more specific:

```css
.top-nav-list li a {
  color: black;
}
```

## Implement the Posts

In this section we will learn a new layout, grid layout. Unlike the single orientation (either row or column) of `flex` layout, in a grid you can define an arrangement in two dimensions at the same time.

### Grid Layout

Let's learn the common properties of grid layout with an example:

```html
<div class="cards grid">
  <div class="card">Card 1</div>
  <div class="card">Card 2</div>
  <div class="card">Card 3</div>
  <div class="card">Card 4</div>
  <div class="card">Card 5</div>
  <div class="card">Card 6</div>
  <div class="card">Card 7</div>
</div>
```

The HTML snippet above defines seven cards, all of which are children of `grid`. We can see how grid layout works by modifying the CSS properties of `grid`.

```css
.cards {
  width: 800px;
}

.grid {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
  justify-content: center;
  gap: 1rem;
}
```

First, we use `display: grid` to enable grid layout. `grid-template-columns` specifies the width of each column, `grid-template-rows` defines the height of each row, `justify-content` is similar to `flex`, when there is room left in the container, this property defines how the elements are aligned in the container. And finally `gap` represents the spacing between elements (in fact we can specify the row spacing and column spacing separately).

So the above code will generate a grid with 3 rows and 3 columns:

![9 cells in Grid](week-2/9-cells.png)

The `grid` layout alows us to specify the height (or width) of each row/column:

```css
.grid {
  //...
  grid-template-columns: 100px 150px 100px;
  grid-template-rows: 100px 150px 50px;
  //...
}
```

And then we get an uneven grid:

![Uneven Cells in Grid](week-2/uneven-cells.png)

Additionally, we can have certain cells span multiple grids, and the layouter will automatically arrange other elements in the container as appropriate. For example, we want cell one to occupy two columns:

```css
.card:first-child {
  grid-column-start: 1;
  grid-column-end: 3;
}
```

So as to get this result:

![Span two Columns](week-2/span-two-columns.png)

Similar operations can be applied to rows too:

```css
.card:nth-child(4) {
  grid-row-start: 2;
  grid-row-end: 4;
}
```

You may have noticed that the order of the cards is not necessarily in the natural order when automatically arranged, such as the card 4 here is in front of the card 3.

![Span Columns and Rows](week-2/span-columns-and-rows.png)

As you can see from the above example, using grid layout can be very flexible in the row and column direction to complete very complex layout. There are some shorthands to write the above example, let's discuss it a little bit.

For example, if the width of each column of the grid is the same, we certainly don't want to repeat many `100px`. In addition, we prefer that the width of the column can be adaptive, we only need to specify the width of one column. Once there is another `200px` available in the container, a new column will be created:

```css
.grid {
  grid-template-columns: repeat(auto-fill, 200px);
}
```

In addition, we can define grids of different sizes, and then use the adaptive grid layout to complete more interesting layouts. Let's expand the HTML in the above example slightly:

```html
<div class="cards grid">
  <div class="card small">Card 1</div>
  <div class="card large">Card 2</div>
  <div class="card medium">Card 3</div>
  <div class="card small">Card 4</div>
  <div class="card large">Card 5</div>
  <div class="card small">Card 6</div>
  <div class="card medium">Card 7</div>
  <div class="card large">Card 8</div>
  <div class="card small">Card 9</div>
</div>
```

And for `card`, we use `grid-auto-rows: 1rem;` to set the height of the row to 1rem, then in the corresponding `small`, `medium` and `large`, we make these elements span different rows , which forms cards of different sizes:

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, 8rem);
  grid-auto-rows: 1rem;
  justify-content: center;
  gap: 1rem;
}

.small {
  grid-row-end: span 4;
}

.medium {
  grid-row-end: span 5;
}

.large {
  grid-row-end: span 6;
}
```

The `grid-row-end` property defines how many rows the element should span. For example, `small` will span 4 rows and 3 `gap` between rows (each gap is 1rem), so the height is 4rem+ 3rem. 5+4rem for `medium` and 6+5rem for `large`.

![Different Height Auto Layout](week-2/mixed-heights.png)

Excellent! With these basics in place, we can move on to the implementation of the posts area.

```html
<section class="posts-container">
  <!-- header -->
  <section class="cards-container">
    <div class="card small">
      Card 1
    </div>

    <div class="card large">
      Card 2
    </div>

    <div class="card medium">
      Card 3
    </div>
    <!-- other cards -->
  </section>
</section>
```

For the container, we can adopt an adaptive layout, that is, specify the ideal width of each row (card), and when the screen size changes, the card will automatically wrap.

```css
.cards-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 15rem);
  grid-auto-rows: 1rem;
  justify-content: center;
  gap: 1rem;
  margin: 1rem 0;
}

.card {
  padding: 1rem;
  border-radius: 0.5rem;
}

.small {
  grid-row-end: span 10;
}

.medium {
  grid-row-end: span 12;
}

.large {
  grid-row-end: span 14;
}
```

![Cards in Design](week-2/grid-layout-initial.png)

It looks like we're making good progress, in the content area we have a search box at the top and a very nice responsive layout. Next, we can implement the content of each card.

### Card Implementation

For each individual item, we can split it into small parts like how we split the page, each part can use its own layout, `flex` is enough for these scenarios.

First, let's implement the header of the card:

```html
<article class="card small">
  <header class="flex basic-info">
    <div class="avatar">
      <img src="assets/juntao.avatar.png" alt="avatar">
    </div>
    <div class="brief">
      <h4>Juntao Qiu</h4>
      <p>Web developer</p>
    </div>
    <div class="menu">
      <span class="material-symbols-outlined">
        more_vert
      </span>
    </div>
  </header>
</article>
```

Then use the `flex` layout for the avatar and menu sections, which we already know very well:

```css
.flex {
  display: flex;
  gap: 1rem;
  align-items: center;
}

.avatar {
  width: 2rem;
  height: 2rem;
}

.avatar img {
  width: 100%;
  border-radius: 100%;
  border: 1px solid orangered;
}

.flex p {
  font-size: 12px;
}

.basic-info {
  padding: 0 1rem;
}

.menu {
  margin-left: auto;
  width: 1.2rem;
  height: 1.2rem;
  color: #C3D0E0;
  cursor: pointer;
}
```

The header of such a card is completed:

![Card Header](week-2/card-header.png)

Next is the image/video part. It should be noted here that different media may have different sizes. In real-world scenarios, we may need to use JavaScript to deal with images with different sizes (like add `small` class when it's small image), but here we will skip JavaScript. We'll randomly takes one of three predefined sizes (small, medium, or large) and then place the card randomly on the page.

```html
<article class="card small">
  <!-- header -->
  <section class="media">
    <img src="assets/boats.JPG" alt="boats in Brighton pier">
  </section>
</article>
```

The corresponding CSS styles are:

```css
.media {
  width: 100%;
  margin: 1rem 0;
  border-radius: 0.5rem;
}

.media img {
  width: 100%;
}

.small .media {
  max-height: 124px;
  overflow: hidden;
}
```

As you can see, the picture part looks pretty nice now:

![Media in Card](week-2/card-media.png)

Next is the `control` area, there are some icons with states. When the user clicks "like", the state of this icon will change. Although the code to save this state requires JavaScript and backend services to work together, we can simulate it in CSS here.

```html
<div class="flex controls">
  <span class="material-symbols-outlined active">
    favorite
  </span>

  <span class="material-symbols-outlined">
    chat_bubble
  </span>

  <span class="material-symbols-outlined">
    movie
  </span>

  <span class="material-symbols-outlined">
    bookmarks
  </span>
</div>
```

Note that here we have added two CSS classes for the first `span`: `material-symbols-outlined` and `active`, in CSS we can add multiple classes to an element, which helps us to the split the styles for state and styles for representation:

```css
.controls {
  padding: 0 1rem;
}

.controls span {
  font-size: 1.2rem;
  color: #C3D0E0;
}

.controls span:last-child {
  margin-left: auto;
}

span.active {
  color: #E6254A;
}
```

For example, `span.active` here is only responsible for modifying the color of the font, while `material-symbols-outlined` is responsible for the font icon taking effect. This way we separate the concerns of different CSS classes. In the same way, the `flex` class above is responsible for turning an element into a `flex` layout.

![Controls](week-2/card-controls.png)

In the final step, we need to add the reply information and timestamp to the card:

```html
<div class="liked">
  <p>Liked by Lucy <span class="highlight">and 152 others</span></p>
</div>

<div class="comments">
  <p>Boats in Brighton Beach with beautiful sunsetting</p>
  <p class="date">Today date</p>
</div>
```

and corresponding styles:

```css
.liked {
  padding: 0.5rem 1rem;
  font-size: 12px;
  color: gray;
}

.highlight {
  font-weight: bold;
}

.comments {
  padding: 0 1rem;
  font-size: 12px;
}

.date {
  color: lightgray;
  font-size: 10px;
}
```

In this way, we have completed all the implementation of a individual card, let's take a look at the result:

![Card Finished](week-2/card-done.png)

For the `medium` and `large` cards, we need some minor adjustments to keep the image from being cropped:

```css
.medium .media {
  max-height: 172px;
  overflow: hidden;
}

.large .media {
  max-height: 234px;
  overflow: hidden;
}
```

This gives us the completed post list, and more importantly, the layout adapts itself to the screen size. Let's add some more cards to check it out:

![Adaptive Post List](week-2/cards-adaptive.png)

### The icing on the cake - Shadows

Finally, let's add a little visual element to the cards to make them look more three-dimensional. Yes, in CSS we can use shadows to an element.

Adding a shadow is pretty simple, just specify the `box-shadow` property, the syntax is:

```css
box-shadow: <h-offset> <v-offset> <blur> <spread> <color>;
```

They are: horizontal offset, vertical offset, blur radius, spread radius and shadow color. You can understand these parameters like this: imagine that the page element is a real card (such as a playing card), and then imagine that there is a light source shining directly above the card. And there will be a projection directly below it, a shadow. If the light source moves horizontally to the left, the shadow under the card will move to the right, which is the horizontal offset, and the same is true for the vertical offset. 

When the light source is far away from the card, the light will diffract and create a blurry shadow, which is the spread radius.

For example, we can set the shadow of the card as:

```css
.card {
  box-shadow: 4px 4px 16px #efefef;
}
```

Meaning, we assume the light source is coming in from the top left corner, the shadow will be offset 4 pixels to the right and down from the card's position, the blur radius will be 16 pixels, and the color will be `#efefef`.


![Box Shadow](week-2/box-shadow.png)

It should be noted that when using the shadow color, try to choose a color that is closer to the background. A common mistake is that people tend to use darker colors or brighter color, making elements look too "dimensional" and unreal.

## Challenges

### Challenge 1

According to the `flex` layout and `grid` layout we have learned, try to implement the user information section on the right. TIP: Our two-week course includes everything you need to know to implement this area (typography, fonts, layouts, rounded buttons, etc.).

As we learned in these two chapters, you can start by deconstructing page elements and writing HTML, and then gradually add CSS. Don't worry about doing all the parts at once, it's more of an iterative, step-by-step process.

### Challenge 2

In addition, you can try to implement card from a social media (or news site) that you are familiar with, extract the card element from it, analyze and implement the card (a post or news feed). Technical points you may need to pay attention to:

1. Reasonable use of semantic HTML tags
1. Use `flex` layout
1. Use font icons

Finally, you may wish to publish your work and share with your friends or colleagues.

In the next chapter, we'll learn mobile-first approach to implement a responsive design, use the `Emmet` plugin to simplify HTML writing, use CSS variables to eliminate duplicate CSS rules, and use media queries to adapt to different screen sizes.