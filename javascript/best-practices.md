# Pragmatic Standards: JS/CSS/HTML Coding Standards and Best Practices

## Introduction:

This is a coding standards and best practices guide.
This document focuses on __pragmatism, not perfection__, and is community-driven, so please feel free to contribute!

## TL;DR:

### JavaScript
- Your code must pass http://www.jshint.com/
- Never use eval()
- Always use the strict equality operator (===) unless testing for truthiness / falsiness
- Never exceed 5 levels of indentation / nesting
- Never exceed 15 lines of code per function
- Never exceed 100 characters per line of code
- Never declare global variables except the 'phluant' object (used as namespace)
- Always declare variables closest to the context in which they're used
- Always reference global variables through the window object (eg. window.alert()) 

### HTML
- Write semantic HTML (use the correct HTML elements for your purposes)
- Never include inline javscript in your HTML
- Give meaningful ids & class names

### CSS
- Group properties by purpose (eg. for positioning, coloring, etc.)

### Style
- Indentation / Whitespacing: 
    - 1 tab per nesting
    - a keyword followed by ( (left parenthesis) should be separated by a space
    - opening braces should exist on the same line as the code block's header, separated by a single space
    - closing brackets should vertically align with the beginning of the code block's header
- Always omit braces when possible
- Quotations: use ' for js / css, and " in HTML
- Word Seperations: use hyphens for html / css, and camelcase for js
- Always reduce code statements to 1 line if it does not exceed 100 characters
- Never omit semicolons

### General Practices
- Try to make the program's flow as obvious as possible
- Strive to define / use variables & functions in the most specific context possible
- Give intuitive / descriptive names to everything
- Use "git pull --rebase" when pulling down code
- Leave comments often, but only when they're

## If You're A JavaScript Noob, Read These:

### [JavaScript: The Good Parts, by Douglas Crockford][good-parts]
The good news is, this book is very short.  The bad news is, it's very dense and will likely require a lot of thinking and re-reading.  But The Good Parts is, in my mind, the definitive guide on how an engineer should approach JavaScript.  If you take nothing else aware from this article, please take away this: read The Good Parts.

### [Best Practices for Speeding Up Your Site, by the Yahoo! Developer Network][yahoo-speed]
In particular, check out the JavaScript section.  This is the best guideline for optimization I'm aware of.

### [Constructors considered mildly confusing, by Joost Diepenmaat][constructors-confusing]
If you've ever been confused by how constructors and the prototype property work in JavaScript, this article does a very detailed job of explaining how they work.

### [jQuery API][jquery-api]
jQuery is essential for the modern JavaScript developer.  Consult their API docs to learn all the library has to offer.

## If You're A JavaScript Ninja, Read These:

### [The Surprisingly Elegant JavaScript Type Model, by Kannan Vijayan][javascript-type]
JavaScript's type model is deceptively complicated, but this article does the best job I've ever seen of explaining it.

### [JavaScript Object vs Function experiment, by Steve Kwan][object-function-experiment]
If you liked Kannan's article, you might like this.  It's a really trippy demonstration of the relationship between the Object and Function objects.

### [Partial Application in JavaScript, by Ben Alman][partial-application]
Currying and partial function application are important, but advanced, concepts within functional languages like JavaScript.  When you're ready to truly master these concepts, this article is one of the best

### [JavaScript constructor vs prototype experiment, by Steve Kwan][constructor-prototype-experiment]
If you _really_ want to understand constructor vs prototype, here's an in-depth example.

## Best Practices We Follow:

### Use the Module Pattern to encapsulate.
The Module Pattern uses functions and closures to provide encapsulation in functional languages like JavaScript.

The Module pattern is the right solution if you want to:

* encapsulate a chunk of code and a "class" doesn't make sense
* provide public/private support (which "classes" don't support).

The Module Pattern tends to work well for a chunk of code that could be described as a Singleton class.

For an excellent explanation of the Module Pattern, read [The Good Parts, by Douglas Crockford][good-parts].

For a thorough demonstration of the Module Pattern, see this [Module Pattern example][module-pattern-experiment].

### Namespace your JavaScript if you need to refer to it elsewhere.
Your JavaScript shouldn't be floating off in the global namespace, where it collides with other stuff you've included.

Although JavaScript doesn't have built-in notions of namespaces, its object model is flexible enough that you can emulate them.  Here's an example:

```js
var MyNamespace = MyNamespace || {};

MyNamespace.MyModule = function()
{
    // Your module is now in a namespace!
}
```

For a more in-depth example of namespacing, see this [Module Pattern example][module-pattern-experiment].

### Always anonymously scope JavaScript if you’re not going to call it elsewhere.
If you're writing a one-off chunk of JavaScript that never needs to be referred to by other code, it's wise to anonymously scope it so it won't get accidentally referenced elsewhere. This is especially true for code that would otherwise execute in global context.

To do this, just wrap your code in an anonymous function closure:

```js
(function(){ // An anonymous function that can never be referenced by name...

var x = 123; // local variable that won't conflict with any global variables
console.log(x);
    
})(); // Call the anonymous function once, then throw it away!
```

### When optimizating, focus on the big things.
Some things cause a big dip in performance, such as:

* Excessive DOM changes that force the page to re-render
* Events that get fired _all the time_ (for example, resizing/scrolling)
* Lots of HTTP requests (and even this is becoming less important).

These are problems you should address.  Fixing them could result in less page stuttering.

If your optimizations are making your code uglier (and thus more difficult to maintain), ask yourself: are these "optimizations" worth it?  For most webpages, 1ms isn't going to make or break the user experience.  There are probably better uses of your time.

### Lazy load assets that aren't immediately required.
You can _greatly_ improve load times and reduce bandwidth considerations by lazy loading page assets.

If your page is media-heavy, we recommend investigating JavaScript techniques to load media assets only when they are required.

### `unbind()` all event handlers before binding.
Generally speaking, this is fine:

```js
var handleClick = function()
{
    // Do some stuff
};

var init = function()
{
    $("a.some-link").click(handleClick);
};
```

There's one problem, though: if `init()` ever gets called more than once, that link is going to wind up with `handleClick()` bound to it twice.  And that means it'll execute twice if the link is clicked.

A good defensive coding practice is to unbind an event handler before binding, like so:

```js
var handleClick = function()
{
    // Do some stuff
};

var init = function()
{
    $("a.some-link").off(handleClick).click(handleClick);
};
```

Note that _relying_ on the above can sometimes be a bad "code smell," because it is often an indication that you don't truly understand how your code is working.  After all, if you really understand what's going on, you should be able to prevent init from being called twice.

## Bad Practices We Avoid:

### Inlining the crap out of functions and object literals.

At no point in your javascript files should you exceed 5 levels of nested code (including base level). Use variables and function definitions to achieve a flatter code structure and a clearer program flow. 

```js
var name = 'Steve Kwan';
var company = 'Electronic Arts';

var myFunction = function()
{
    $('form#my-form').submit
    (
        function(event)
        {
            event.preventDefault();
            $.ajax
            (
                '/some_service',
                {
                    type: "POST",
                    data:
                    {
                        name: name,
                        name: company
                    },
                    success: function(data)
                    {
                        callSomeCompletionFunction
                        (
                            {
                                response1: data.value1,
                                response2: data.value2
                            }
                        )
                    },
                    error: function(data)
                    {
                        callSomeErrorHandler
                        (
                            {
                                response1: data.value1,
                                response2: data.value2
                            }
                        )
                    }
                }
            );
        }
    );
};
```

This kind of code is problematic because:

* It causes major readability and maintainability issues
* It prevents you from reusing some of those nested functions

### Causing excessive document reflows.
DOM modification is slow, because in addition to rebuilding the DOM tree, the browser must recalculate all element dimensions and see how they fit together.  This is _especially_ a problem when your elements are positioned via `static` (the default) or `relative` - and this is almost always the case.

This is will make the user's browser stutter:

```js
var table = $('<table></table>');

$('body').append(table);

for (var i=0; i<10000; i++)
{
    $('table').append
    (
        '<tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>'
    );
}

window.alert("Done!");
```

Keep your DOM modifications to a bare minimum.

If you must make significant DOM modifications, try to batch up your DOM modifications and do them all at once.  

If you cannot do them all at once, do your modifications in a document fragment and only append to your main document once.  At the very least that'll prevent the browser from having to do complex rendering while the elements are being added.

Compare the performance of the snippet below to the one above.  The one below is much faster!

```js
var table = $('<table></table>');

for (var i=0; i<10000; i++)
{
    $(table).append
    (
        '<tr><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td></tr>'
    );
}

// Append the table at the end this time.  The document has not been modified
// until we do this.
$('body').append(table);

window.alert("Done!");
```

### Really long function chains.
Function chaining like this can be really handy:

```js
$('.some-element').click(handleClick).parents('.container')[0].slideDown();
```

This terse, abbreviated form of jQuery can be very clear and good for performance.  But you can also get yourself into trouble.

In the above example, if the `.container` selector returns no results, you'll encounter a JavaScript error.

Function chaining is great, but be wary of null or undefined values coming back mid-chain.

### Causing a flash of unstyled content (FOUC) due to late-loading JavaScript.
We've all seen it: you load a website, it appears unstyled for a brief period, then it flickers into the right design.  You may not be able to reproduce the issue if you refresh the page.  What happened?

This is called a flash of unstyled content, or FOUC.  It's an old problem originally stemming from incorrect placement of stylesheets in your markup.  However, FOUC is making a resurgence due to late-loading JavaScript that redecorates the page.

It's common knowledge that putting your JavaScript at the bottom of your document leads to better performance.  However, if your JavaScript contains heavy DOM manipulation this may have the unpleasant side effect of your page being rendered twice: once without JavaScript, then a second time once your JavaScript is loaded.

So what's the solution?  Well, you could move all your JavaScript back into the `<head>`, which will result in your JavaScript being loaded before the document itself.  Unfortunately, this causes a completely different (and worse) performance problem: now your page will not render _at all_ until all JavaScript is loaded.  So don't do that.

Unfortunately, there is no good way to prevent this issue that I am aware of.  But there are some things you can do to alleviate the problem:

* **Minimize the amount of DOM manipulation done in JavaScript.**  
    As a rule, minimize or batch up DOM manipulations.  Document reflow is one of the biggest client-side performance hits you can incur.  There's a really in-depth explanation of reflow earlier in this document.
* **Specify the `width`/`height` for the page elements that will be changed by JavaScript.**  
    You can minimize the impact of FOUC by specifying dimensions on all the page regions that will be affected by late-loading JavaScript.

    For example, if your JavaScript renders a component that will always be 200em tall, specify that height in CSS.  That way the browser knows how much space to allocate for your component before it has been loaded, and this will minimize document reflow and jitters when the content comes in.
* **Mask the problem with CSS3 or JavaScript intro animations.**  
    You can slap a quick band-aid over the problem by applying a fade-in or slide-in animation to the element being affected by FOUC.  This hides than problem rather than solving it, but in a lot of cases this may be good enough.

    Note that if the offending late-loading JavaScript comes in _really_ late, masking the problem with an animation may not actually help because the animation will complete before the JavaScript gets executed.
* **Block rendering until all content on the page has been loaded.**  
    In most cases, this is a bad solution.  However, it's still worth discussing.

    You can possible block progressive rendering by hiding the FOUC-affected regions, and only showing them once you know for certain all content is loaded (usually by trapping the `load` event).

    However, blocking progressive rendering almost always makes the UX worse rather than better.  FOUC is bad, but it's often not as bad as refusing to show any content at all.  That said, there may be a few select cases where the FOUC is so offensive and performance-intensive that you need to investigate this option.
* **Inline the offending JavaScript and put it beneath the affected DOM elements.**  
    This is another of those "use sparingly" techniques, because it flies in the face of best practices.  It requires you to litter your DOM with embedded code, which is bad for myriad reasons.

    However, in truly offensive cases of FOUC, this may be the "best" way to solve the problem.

    When the browser parses your document, it executes JavaScript at the exact moment it encounters it.  So to minimize FOUC, you can bring your JavaScript and the elements it affects closer together.  Here's an example:

    ```html
    <div class="fouc-affected-element">
        <script>
            // Do something with .fouc-affected-element to resolve the FOUC
            // issue
        </script>
        ...
    </div>
    ```

    Again, I need to reiterate that this is a somewhat occult technique and should be used _very sparingly_.  It results in ugly, unmaintainable code.  But it also can fix FOUC quite cleanly in some situations.

    If you _do_ choose to use this technique, be aware that `<script>` tags _block progressive rendering_.  So ensure that these inline `<script>` tags execute fast and have no external dependencies.  Don't do any Ajax calls in there.

## "Best" Practices We Disagree With:
These are strategies which are generally considered best practices elsewhere within the community, but that I feel are unessential at best, and counterproductive at worst.  Most of my objections to these "best" practices stem from code defensiveness and pragmatic time management.  I'll outline specifics below.

Many of these come from: [JS adolescence, by James Padolsey][js-adolescence].

### Combined `var` declarations.
JavaScript variables are commonly defined in a single chain, such as:

```js
var header,
    topnav,
    content;
```

As far as I'm concerned, this style of variable declaration in JavaScript is far too fragile and easily broken.

Let's walk through a realistic example.  Say we decide that we want to provide initial assignments to these variables, so we adjust them like so:

```js
var header = $('.header')
    topnav = $('.topnav'),
    content = $('.content');
```

Did you catch the problem above?  A lot of people wouldn't - it's easy to miss it at first glance.  While adding the initial assignments, we "accidentally" lost a comma at the end of the `header` declaration.

But that typo isn't as interesting as the bug it'll cause.  In JavaScript, semicolons should be added at the end of every statement, but they're not required.  The code above will generate no error messages, no warnings...but it will create a serious bug.

Because the line `var header = $('.header')` is valid JavaScript even without the trailing comma, the parser will go ahead and "insert" a semicolon for you.  By doing this, it effectively interprets the code as:

```js
var header = $('.header');
    topnav = $('.topnav'),
    content = $('.content');
```

This breaks the `topnav` and `content` declarations into a separate statement, and these are now variable declarations _without the `var` keyword_.  Those variables will now be _ejected into the global namespace_, regardless of how you tried to scope them.

I prefer to declare variables as separate statements:

```js
var firstVar;
var secondVar;
var thirdVar;
```

A lot of JavaScript developers dislike this syntax, and the objections are usually:

* It's worse for performance (because there are more bytes required)
* It's unnecessarily long and harder to read.

The performance issue is moot because it's a micro-optimization and a minifier will eliminate the difference anyway.  The length/readability issue is just a preference, not a universal constant.  And even if it were, the benefits of defensively programming this way (and the debugging time it will save) greatly outweigh any inconvenience caused by formatting.

For these reasons I recommend _repeating the `var` keyword with each variable declaration_.

### Putting all variable declarations at the top.
Many JavaScript developers prefer to list all variable declarations at the top of a block, rather than peppering them throughout.  The argument is usually that the JavaScript language hoists variables to the top of a block anyway, so you might as well write your code to reflect that.

However, I don't write my code to impress the parser.  I write it for the people after me who will have to understand it.  And I personally find code much easier to understand if variable declarations occur close to the context where they are being used.

### Caching selectors for long periods of time.
Reusing selectors is a good thing.  For example, you could refactor this:

```js
var initMenu = function()
{
    $('.menu').show();
    $('.menu').addClass('enabled');
    $('.menu').click(handleClick);
};
```

into this:

```js
var initMenu = function()
{
    var $menu = $('.menu'); // Cache the selector
    $menu.show();
    $menu.addClass('enabled');
    $menu.click(handleClick);
};
```

It's a good thing to reuse a selector multiple times, but it's much riskier to cache them _forever_.  This, for example, could get a bit dangerous:

```js
var $menu = $('.menu'); // Cache the selector FOREVER!

var initMenu = function()
{
    $menu.show();
    $menu.addClass('enabled');
    $menu.click(handleClick);
};
```

This code is fine, but be wary of the fact that on most modern webpages the DOM is constantly changing.  A selector cached for a long period of time could eventually wind up failing you.

If you have event handlers that fire much later (such as that `handleClick` handler we're referencing), you may find yourself in a situation where they attempt to use the selector and it's now invalid.  This can result in unexpected bugs that may not be easily caught.

Yes, you can shave off a few milliseconds by caching selectors, but don't cache them for _too_ long or the risk of bugs starts to increase.  I prefer to err on the side of caution and _cache selectors only for the block they're used in_.

### Obsessing over indentation, tabs vs spaces, and squiggly bracket placement.
In this document I've used [Allman style][allman-style] indentation.  I find it far more elegant and readable than other styles.  I like its Gestalt.  But I fully understand I am in the minority on this, and I would never enforce this style on anyone else.  :)

I would rather engineers be focused on writing quality semantic code that makes sense.  I don't want talent wasting hours of their time on squigglies and tabs, when we could do that job in minutes with a linter.

It just isn't worth the effort obsessing over rigid indentation/squiggly rules.  Do what you think makes sense in the given context.  If your code is readable and clear, it should speak for itself regardless of the indentation style you choose.

Just be prepared to have your formatting overwritten by my linter.  :)

### Obsessing over strict equality.

The difference between `==` and `===` is important in JavaScript, _especially_ when checking against falsiness/truthiness.

But with that exception, it's not something I lose a lot of sleep over.  Yes, you should endeavour to use `===`, but this isn't the sort of thing that I would turn into a religious war.

### Optimizations that try to "outsmart the browser."
Try to avoid using performance hacks that rely on browser idiosyncracies or the internals of the libraries you're using.

Maybe you've discovered a CSS hack that tricks particular browsers into rendering the page quicker.  Or maybe you've discovered that due to the internals of a library you're using, you can eke out a small performance boost by passing in your parameters differently.  Before you modify your code to use these tricks, think about the long-term consequences.

If the benefit is huge, then it may be worth using your optimization.  But if the benefit is minor, ask yourself if you're filling your code with hacks for little benefit.  Will future developers look at your code and understand why you did things the way you did?  Will the user even notice a speed improvement?

There are times when hacks are worth it, but bear in mind that they are ultimately hacks.  I would prefer good code that stands on its own and is not coupled to oddities inside the tools we use. 

<!-- My CSS documentation -->
[css-gotchas]: https://github.com/stevekwan/best-practices/blob/master/css/gotchas.md
[css-best-practices]:https://github.com/stevekwan/best-practices/blob/master/css/best-practices.md
[css-style-guide]:https://github.com/stevekwan/best-practices/blob/master/css/style-guide.md

<!-- My JavaScript documentation -->
[javascript-gotchas]: https://github.com/stevekwan/best-practices/blob/master/javascript/gotchas.md
[javascript-best-practices]: https://github.com/stevekwan/best-practices/blob/master/javascript/best-practices.md
[javascript-style-guide]:https://github.com/stevekwan/best-practices/blob/master/javascript/style-guide.md
[module-pattern-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/module-pattern.html
[object-function-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/object-vs-function.html
[constructor-prototype-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/constructor-vs-prototype.html
[module-pattern-experiment]: https://github.com/stevekwan/experiments/blob/master/javascript/module-pattern.html

<!-- External documentation -->
[allman]: http://en.wikipedia.org/wiki/Indent_style#Allman_style
[good-parts]: http://shop.oreilly.com/product/9780596517748.do
[constructors-confusing]: http://joost.zeekat.nl/constructors-considered-mildly-confusing.html
[jquery-api]: http://api.jquery.com/
[javascript-type]: http://vijayan.ca/blog/2012/02/21/javascript-type-model/
[partial-application]: http://benalman.com/news/2012/09/partial-application-in-javascript/
[js-adolescence]: http://james.padolsey.com/javascript/js-adolescence/
[yahoo-speed]: http://developer.yahoo.com/performance/rules.html
[javascript-hoisting]: http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting
[idiomatic-js]: https://github.com/rwldrn/idiomatic.js/
