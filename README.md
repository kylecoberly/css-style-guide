# CSS Style Guide
A style guide emphasizing readability (for both CSS and HTML) and simplicity.

## Why?
Existing style guides either over-prioritize CSS performance, make the wrong parts simple, or create beautiful CSS at the cost of ugly HTML.

## The Guidelines
### General
* *Tabs Over Spaces.* There are people that think this:
```css
.my-selector {
  first-property: 1;
  second-property: 2;
  nested-selector {
    nest-property: 3;
  }
}
```
is easier to read than this:
```css
.my-selector {
    first-property: 1;
    second-property: 2;
    nested-selector {
        nest-property: 3;
    }
}
```
Tabs can be formatted any way you want, so everyone gets something they are comfortable with. They are also easier to type and edit.

* *Dasherize Spaces Between Words.* This keeps it consistent with CSS itself (eg. `background-color`);
* *Use Semicolons.* Even if you don't need it on the last property, you never know when you'll need to move or reorder it.
* *Each property on its own line.* Just like a bulleted list.
* *Use "One True Bracket" Format.* Looks like this:
```css
.my-selector:nth-child(3) {
    first-property: 1, 2, 3;
}
```
* *All Lowercase.* Like the rest of CSS.
* *No Units for Zero Values.* 0 is 0 is 0.
* *Use Double Quotes.* Keeps you from having to escape apostrophes.
* *When preprocessing is appropriate, use SCSS.* It's syntactically identical to CSS and adds many great features.

### Selector Choice
Avoid ID selectors, use class selectors as needed, prefer element selectors.

Specificity is not confusing or hard to calculate. ID selectors get 100 points, class selectors get 10 points, element selectors get 1 point. The rule with the most points wins, tie-breaker is last to be declared. Over-specifying and relying on element selectors has some performance implications, but CSS performance is rarely a bottleneck and the gains to code clarity are worth it. Additionally, this approach prevents polluting your clean, semantic HTML with buckets of classes just to locate elements.

Consider this:
```css
.about-me {
    margin: 2rem;
}
.main-header {
    font-weight: 300;
    font-size: 2rem;
}
.body-section {
    padding: 1rem;
}
.text-block {
    font-size: 1rem;
}
.text-block.first {
    font-size: 1.2rem;
}
.footer {
    font-style: italic;
}
```
```html
<div class="about-me">
    <h1 class="main-header">My Header</h1>
    <div class="body-section">
        <p class="text-block first">Some text</p>
        <p class="text-block">Other text</p>
    </div>
    <div class="footer">All rights reserved</div>
</div>
```

vs. this:
```css
.about-me {
    margin: 2rem;
    h1 {
        font-weight: 300;
        font-size: 2rem;
    }
    section {
        padding: 1rem;
        p {
            font-size: 1rem;
            &:first-child {
                font-size: 1.2rem;
            }
        }
    }
    footer {
        font-style: italic;
    }
}
```
```html
<div class="about-me">
    <h1>My Header</h1>
    <section>
        <p>Some text</p>
        <p>Other text</p>
    </section>
    <footer>All rights reserved</footer>
</div>
```
The first is what most style guides recommend, as it is negligibly more performant. The second is much more readable, respects the semantics and hierarchy of the document, and doesn't require coming up with 6 different class names. 

### DRY Smart 
Many specific styles are derivative of more general styles in the document. Make generous use of variables, inheritance, and mixins to keep from repeating yourself, especially regarding things like typography, colors, and vendor prefixes. That said, just because two elements look the same now doesn't mean they should share styling forever. When reusing, ask yourself if the elements are truly the same.

### No Display Classes
Documents describe what something is, stylesheets describe how it should look. Respect the separation of concerns! Display classes are class names like:
* red, turqouise, primary-color
* left, flush, justified
* 3-column, display-on-small, mobile-only

### No "Programmer Abbreviations"
These came from a time when bytes were expensive. Now, attention and readability are far more expensive than bytes. Most abbreviations can be understood, but they place a pointless cognitive load on the user and are difficult to remember since they are generally arbitrary. Additionally, some of them can't be pronounced, or are pronounced differently than they are written. Here is a partial list of common offenders:
* btn
* hdr
* ftr
* cntr
* txt
* prev
* hov
* sm
* md
* lg
* col

Exceptions can be made for "min" and "max".

### Be Ashamed of Comments
Comments mean that your code isn't self-explanatory. This may be because the names aren't descriptive, there isn't enough hierarchy in your code, there isn't enough hierarchy in your modules, there isn't enough hierarchy in your files, because you're using a hack, or because you're lazy. Hey, these all happen and sometimes there isn't time or energy to fix them. By all means, comment in these scenarios. That said, a comment represents a failure to communicate and should be avoided.

The fundamental problem is that comments create two sources of truth- what your code does, and what you say it does. People updating the code may or may not also update the comments (and vice versa), and it encourages other developers to gloss over your actual code. A sufficiently descriptive name in a well-organized hierarchy communicates much better than a comment does.
