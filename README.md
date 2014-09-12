#FE Code Guidelines

Write markup before CSS. This means you can visually see which CSS properties are naturally inherited and thus avoid reapplying redundant styles.

By writing markup first you can focus on data, content and semantics and then apply only the relevant classes and CSS afterwards.

##Contents
+  [HTML](#html)
  +  [General](#general)
  +  [Semantics](#semantics)
  +  [Attribute order](#attribute-order)
  +  [IDs vs Class names](#ids-vs-class-names)
  +  [Naming classes](#naming-classes)
    +  [Sensibleness](#sensibleness)
    +  [Object-Oriented CSS (OOCSS)](#object-oriented-css-oocss)
    +  [General](#general)
    +  [BEM (OOCSS)](#bem-oocss)
    +  [JS Hooks](#js-hooks)
+  [CSS](#css)
  +  [Consistency](#consistency)
  +  [Source order](#source-order)
  +  [Anatomy of rulesets](#anatomy-of-rulesets)
  +  [Layout](#layout)
  +  [Selectors](#selectors)
  +  [Over-qualified selectors](#over-qualified-selectors)
  +  [Selector performance](#selector-performance)
  +  [CSS selector intent](#css-selector-intent)
  +  [Magic numbers and absolutes](#magic-numbers-and-absolutes)
  +  [Debugging](#debugging)
  +  [Good to know](#good-to-know)
+ [Javascript](#javascript)
  + [Comments](#comments)  
  + [jQuery](#jquery)
+ [Animation](#animation)
  +  [Do's](#dos)
  +  [Don'ts](#don'ts)
+ [Tools](#tools)
+ [References](#references)

##HTML
HTML should not contain any styling or even implies what the styling might be. Everything on the page is either a required site resource, content, or describing content.

###General
+  Use HTML5 doctype  
+  Define language `<html lang="en-us">`  
+  Define Character encoding `<meta charset="UTF-8">`  
+  Set IE compatibility mode to the latest `<meta http-equiv="X-UA-Compatible" content="IE=Edge">`  
+  tab indented (4 spaces long)   
+  Always use double quotes, never single quotes, on attributes.  
+  Use single space between inline elements `<button>Save</button> <button>Cancel</button>`  

###Semantics
Semantics in front-end developments concerns whether to use a div or a header, or if some text should be a paragraph or a heading, or whether a list is to be ordered or unordered, and so on.

###Attribute order  
HTML attributes should come in this particular order for easier reading of code.  

1.  class  
2.  id, name  
3.  data-*  
4.  src, for, type, href  
5.  title, alt  
6.  aria-*, role  

###IDs vs Class names
Use only classes for styling, because they are reusable. Still using ID attributes in your HTML can be a good thing and in some cases, absolutely necessary. For example, they provide efficient hooks for JavaScript. For CSS, however, ID selectors aren’t necessary as the performance difference between ID and class selectors is nearly non-existent and can make styling more complicated due to increasing specificity.

###Naming classes

Always ensure classes are sensibly named; keep them as short as possible but as long as necessary. Ensure any objects or abstractions are very vaguely named (e.g. `.ui-list`, `.media`) to allow for greater reuse. Extensions of objects should be much more explicitly named (e.g. `.user-avatar-link`). Don’t worry about the amount or length of classes; gzip will compress well written code incredibly well.

####Sensibleness

When choosing names for your classes, look more at their longevity. How long will this name continue to make sense? how long will it stay relevant? How much sense will it make in six months?

####Object-Oriented CSS (OOCSS)

Instead of building dozens of unique components, try and spot repeated design patterns across them all and abstract them out into reusable classes; build these skeletons as base ‘objects’ and then peg classes onto these to extend their styling for more unique circumstances.

Build the structure of the component using very generic classes so that we can reuse that construct and then use more specific classes to skin it up and add design treatments.

####General

For the most part simply use hyphen delimited classes (e.g. `.foo-bar`, not `.foo_bar` or `.fooBar`)

####BEM (OOCSS)

Meaning - block,element,modifier - is a methodology for naming and classifying CSS selectors in a way to make them a lot more strict, transparent and informative.

The naming convention follows this pattern:  
```
.block{}  
.block__element{}  
.block--modifier{}  
```
+  `.block` represents the higher level of an abstraction or component.
+  `.block__element` represents a descendent of .block that helps form .block as a whole.
+  `.block--modifier` represents a different state or version of .block.

An analogy of how BEM classes work might be:  
```
.person {}  
.person--woman {}  
.person__hand {}  
.person__hand--left {}  
.person__hand--right {}  
```
Here we can see that the basic object we’re describing is a person, and that a different type of person might be a woman. We can also see that people have hands; these are sub-parts of people, and there are different variations, like left and right.  

We can now namespace our selectors based on their base objects and we can also communicate what job the selector does; is it a sub-component (`__`) or a variation (`--`)?

So, `.page-wrapper` is a standalone selector; it doesn’t form part of an abstraction or a component and as such it named correctly. `.widget-heading`, however, is related to a component; it is a child of the .widget construct so we would rename this class `.widget__heading`.

BEM looks a little uglier, and is a lot more verbose, but it grants us a lot of power in that we can glean the functions and relationships of elements from their classes alone. Also, BEM syntax will typically compress (gzip) very well as compression favours/works well with repetition.

####JS Hooks

**Never use a CSS styling class as a JavaScript hook.** Attaching JS behaviour to a styling class means that we can never have one without the other.

If you need to bind to some markup use a JS specific CSS class. This is simply a class namespaced with `.js-`, e.g. `.js-toggle`, `.js-drag-and-dmrop`. This means that we can attach both JS and CSS to classes in our markup but there will never be any troublesome overlap.

```
<div id="#slider-uid" class="slider js-slider">
    <div class="slider__slides">
      ...  
    <div>  
</div>
```
The above markup holds two classes; one to which you can attach some styling for slider and another which allows you to add the functionality. In this case we can remove js-slider class when slider functionality is not required (like if there is only one slide to display).

##CSS
###Consistency
Keep margins and paddings consistent! If we don't include spacing in a consistent and correct way to the elements, than we end up with spacing nightmare and unmaintainable css. For example html elements should only have spacing on top/bottom of the element (margin: 20px 0 0 0) and padding of wrapper element on opposite side (margin: 20px 0 0 0; padding: 1px 20px 20px 20px; background:blue).

###Source order
Try and write stylesheets in specificity order. This ensures that you take full advantage of inheritance and CSS’ first C; the cascade.  

A well ordered stylesheet will be ordered something like this:  
1.  **Elements** unclassed h1, unclassed ul etc. -> Use [Normalize.css](http://necolas.github.io/normalize.css/) as starting point.   
2.  **Grid system**   
3.  **Objects and abstractions** — generic, underlying design patterns like .btn, .data, .tooltip, .slider, .tabs etc.  
4.  **Theme**: Site wrapper, header and footer  
5.  **Components**: .registration, .banner, .contact  
6.  **Style trumps**: Color schemas  

This means that—as you go down the document—each section builds upon and inherits sensibly from the previous one(s). There should be less undoing of styles, less specificity problems and all-round better architected stylesheets.  

###Anatomy of rulesets
```
[selector] {  
    [property]: [value];  
    [<- Declaration ->]  
}  

```
I have a number of standards when structuring rulesets:
+  Use hyphen delimited class names (except for BEM notation)  
+  tab indented (4 spaces long)  
+  Multi-line  
+  Declarations in relevance (NOT alphabetical) order  
+  Always include the final semi-colon in a ruleset  

One exception to our multi-line rule might be in cases of the following:
```
.t10 { width:10% }  
.t20 { width:20% }  
.t25 { width:25% }  
.t30 { width:30% }  
.t33 { width:33.333% }  
.t40 { width:40% }  
.t50 { width:50% }  
.t60 { width:60% }  
.t66 { width:66.666% }  
.t70 { width:70% }  
.t75 { width:75% }  
.t80 { width:80% }  
.t90 { width:90% }  

```

###Layout
All components you build should be left totally free of widths; they should always remain fluid and their widths should be governed by a parent/grid system.

Heights should never be be applied to elements. Heights should only be applied to things which had dimensions before they entered the site (i.e. images and sprites). Never ever set heights on `p`s, `ul`s, `div`s, anything. You can often achieve the desired effect with `line-height` which is far more flexible.

Grid systems should be thought of as shelves. They contain content but are not content in themselves. You put up your shelves then fill them with your stuff. By setting up our grids separately to our components you can move components around a lot more easily than if they had dimensions applied to them; this makes our front-ends a lot more adaptable and quick to work with.  

You should never apply any styles to a grid item, they are for layout purposes only. Apply styling to content inside a grid item. Never, under any circumstances, apply box-model properties to a grid item.

###Selectors

Keep selectors short, efficient and portable.

Heavily location-based selectors are bad for a number of reasons. For example, take `.sidebar h3 span{}`. This selector is too location-based and thus we cannot move that `span` outside of a `h3` outside of `.sidebar` and maintain styling.

Selectors which are too long also introduce performance issues; the more checks in a selector (e.g. `.sidebar h3 span` has three checks, `.content ul p a` has four), the more work the browser has to do.

Make sure styles aren’t dependent on location where possible, and make sure selectors are nice and short.

Selectors as a whole should be kept short (e.g. one class deep) but the class names themselves should be as long as they need to be. A class of `.user-avatar` is far nicer than `.usr-avt`.

**Remember:** classes are neither semantic or insemantic; they are sensible or insensible! Stop stressing about ‘semantic’ class names and pick something sensible and futureproof.

###Over-qualified selectors
An over-qualified selector is one like `div.promo`. You could probably get the same effect from just using `.promo`. Of course sometimes you will want to qualify a class with an element (e.g. if you have a generic `.error` class that needs to look different when applied to different elements (e.g. `.error{ color:red; }` `div.error{ padding:14px; }`)), but generally avoid it where possible.

Another example of an over-qualified selector might be `ul.nav li a{}`. As above, we can instantly drop the `ul` and because we know `.nav` is a list, we therefore know that any `a` must be in an `li`, so we can get `ul.nav li a{}` down to just `.nav a{}`.

###Selector performance
Whilst it is true that browsers will only ever keep getting faster at rendering CSS, efficiency is something you could do to keep an eye on. Short, unnested selectors, not using the universal (`*{}`) selector as the key selector, and avoiding more complex CSS3 selectors should help circumvent these problems.

###CSS selector intent
Instead of using selectors to drill down the DOM to an element, it is often best to put a class on the element you explicitly want to style. Let’s take a specific example with a selector like .header `ul{}`...

Let’s imagine that `ul` is indeed the main navigation for our website. It lives in the header as you might expect and is currently the only `ul` in there; `.header ul{}` will work, but it’s not ideal or advisable. It’s not very future proof and certainly not explicit enough. As soon as we add another `ul` to that header it will adopt the styling of our main nav and the the chances are it won’t want to. This means we either have to refactor a lot of code or undo a lot of styling on subsequent `ul`s in that `.header` to remove the effects of the far reaching selector.

Your selector’s intent must match that of your reason for styling something; ask yourself ‘**am I selecting this because it’s a `ul` inside of `.header` or because it is my site’s main nav?**’. The answer to this will determine your selector.

Make sure your key selector is never an element/type selector or object/abstraction class. You never really want to see selectors like `.sidebar ul{}` or `.footer .media{}` in our theme stylesheets.

Be explicit; target the element you want to affect, not its parent. Never assume that markup won’t change. **Write selectors that target what you want, not what happens to be there already.**

Read more about it at [Shoot to kill: CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

###`!important`
It is okay to use `!important` on helper classes only. To add `!important` preemptively is fine, e.g. `.error{ color:red!important }`, as you know you will always want this rule to take precedence.

Using `!important` reactively, e.g. to get yourself out of nasty specificity situations, is not advised. Rework your CSS and try to combat these issues by refactoring your selectors. Keeping your selectors short and avoiding IDs will help out here massively.

###Magic numbers and absolutes
A magic number is a number which is used because ‘it just works’. These are bad because they rarely work for any real reason and are not usually very futureproof or flexible/forgiving. They tend to fix symptoms and not problems.

For example, using `.dropdown-nav li:hover ul{ top:37px; }` to move a dropdown to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only works here because in this particular scenario the `.dropdown-nav` happens to be 37px tall.

Instead you should use `.dropdown-nav li:hover ul{ top:100%; }` which means no matter how tall the .dropdown-nav gets, the dropdown will always sit 100% from the top.

Every time you hard code a number think twice; if you can avoid it by using keywords or ‘aliases’ (i.e. `top:100%` to mean ‘all the way from the top’) or—even better—no measurements at all then you probably should.

Every hard-coded measurement you set is a commitment you might not necessarily want to keep.

###Debugging

If you run into a CSS problem **take code away before you start adding more** in a bid to fix it. The problem exists in CSS that is already written, more CSS isn’t the right answer!

Delete chunks of markup and CSS until your problem goes away, then you can determine which part of the code the problem lies in.

It can be tempting to put an `overflow:hidden;` on something to hide the effects of a layout quirk, but overflow was probably never the problem; fix the problem, not its symptoms.

###Browser specific rules
Include browser specific rule right after the original rule. This way we can instantly see the exception while modifying the original rule. Don't pile browser specific rules to the bottom of stylesheet or into a separate file.
```
.rule {}  
.ie9 rule {}
```
Note: Device specific rules (@media queries) should be all in one file/bottom of the file.  

###Good to know
+  [box-sizing: border-box;](http://www.paulirish.com/2012/box-sizing-border-box-ftw/)
+  [smoothing: antialiased](http://maxvoltar.com/archive/-webkit-font-smoothing)
+  [text-rendering: optimizeLegibility](https://developer.mozilla.org/en-US/docs/Web/CSS/text-rendering)
+  [Normalize.css](https://github.com/necolas/normalize.css)

##Javascript

Guide [http://javascript.crockford.com/code.html](http://javascript.crockford.com/code.html)  
Passes [http://www.jslint.com/](http://www.jslint.com/)  

###Comments
A good comment includes information such as:
+  A description of what the code is doing  
+  Why the code is doing it in this way (as opposed to an alternative)  
+  A reference to any related bug or issue in an issue tracker.  

[Learning JavaScript Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)  
[Programming Javascript Applications](http://chimera.labs.oreilly.com/books/1234000000262)
[Developing Backbone.js Applications](https://github.com/addyosmani/backbone-fundamentals/)

###jQuery  
[http://lab.abhinayrathore.com/jquery-standards/](http://lab.abhinayrathore.com/jquery-standards/)  

##Animation

More Information: [Parallax Done Right](https://medium.com/@dhg/82ced812e61c)  

###Do's

+  Only use properties that are cheap for browsers to animate (translate3d, scale, rotation and opacity).  
+  Use window.requestAnimationFrame.  
+  Round values appropriately.  
+  Only animate elements in viewport.  
+  Animate only absolutely and fixed position elements.  
+  Use natural <body> scroll.  

###Don'ts
+  Avoid background-size:cover.  
+  Don’t bind directly to scroll event.  
+  Don’t animate massive images or dramatically resize them.  
+  Avoid animating 100 things at once.  

##Tools
[http://www.jslint.com/](http://www.jslint.com/)  
[http://developers.google.com/speed/pagespeed/insights/](http://developers.google.com/speed/pagespeed/insights/)  
[http://www.stylestats.org/](http://www.stylestats.org/)  
[http://jsperf.com/](http://jsperf.com/)  

##References
###Books
+  The Smashing Book #4 - New perspectives on web design  
+  SMACSS book  

###Other
+  [cssswizardry css guidelines](https://github.com/csswizardry/CSS-Guidelines)  
+  [http://nicolasgallagher.com](http://nicolasgallagher.com)  
+  [http://mdo.github.io/code-guide/](http://mdo.github.io/code-guide/)  
+  [http://lab.abhinayrathore.com/jquery-standards/](http://lab.abhinayrathore.com/jquery-standards/)
+  [Learning JavaScript Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)  
+  [Parallax Done Right](https://medium.com/@dhg/82ced812e61c)  

##Thanks
+  [Harry Roberts](http://csswizardry.com/)  
+  [Nicolas Gallagher](http://nicolasgallagher.com)  
+  [Nicole Sullivan](http://www.stubbornella.org/)  
+  [Nicholas C. Zakas](http://www.nczonline.net/)  
