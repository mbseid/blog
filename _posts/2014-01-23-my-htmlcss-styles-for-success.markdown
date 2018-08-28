---
layout: post
title: My HTML/CSS Styles For Success
date: 2014-01-23 17:06:00.000000000 -08:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
Over the past few years, I have built sites small and large. From my experience, and learning from my peers around me, I have developed a guide for success. With these guidelines, you can build a great site, and maintain healthy CSS.

1.  **Under specify, rather than over specify your entities. The HTML tags are your friend.**  
     Many newbies to CSS forget you can use style the tags. Use this to your advantage and only name items you need to name.
     {%gist mbseid/8324902 undername.html %}

2.  **CSS Selectors should not have a specificity calculated with more than 4 items.**  
     This does two things: first it keeps your selectors relatively easy to calculate. second, it allows items to be overridden with smaller selectors and without the dreaded !important. It keeps thing easy to read, edit and modify. In all cases, you can probably reduce your HTML/CSS to accommodate this. Bonus: It's better for performance.

3.  **Separate your javascript and CSS by using js-name classes**  
     Using your style selectors for javascript selectors will bite you eventually. Just keep them separated. You will write better code because you won't be bound to using the same selectors. Next time you refactor your HTML and Javascript, you'll thank yourself.
     {%gist mbseid/8324902 js-selectors.html %}

4.  **Create reusable components, and use them!**  
     It's just smart to have your component kit, a page where you list out all your reusable components. It makes building any page fast and easy, while allowing your site to feel consistent. Many times, I find sites deviating from the their kit, creating a nightmare for the CSS, and giving the site a discontinous feel.

5.  **Use the body tag classes! Recipe for success: "page-name base-page-styles-to-use v-1"**  
     Example: `<body data-preserve-html-node="true" class="user-acquisition admin-dashboard v-1">`  
     For every page, there is going to be some deviation from the common components. Instead of finding yourself naming each element, and styling that, you can just nest every rule in the given namespace class. The "page-name" class distinguishes all rules for that given page. You can also nest multiple pages rule in "superclass" that is described as "base-page-styles-to-user". Lastly, the version keeps your semantics and classes clean by giving you a namespace for each version.

6.  **When building a responsive site, start mobile, then build to desktop. Use as many media queries as you have breakpoints!**  
     Too often, I see a media query for each element that is being changed depending on browser size. Not only is this hard to maintain, it is bad for performance too.
     {%gist mbseid/8324902 style.less %}

##### And more to come....

Even the smallest projects still benefit from the best design guidelines. Even though there is less code than in large websites, they still applies to the same rules. Who knows, maybe they will be large projects one day, and you will be thanking yourself for using some of these practices.

I really hope these rules help. Please let me know styles you use that have really done well. I'm always learning.

Some great Presentations and Resources along my way:  
 https://speakerdeck.com/jonrohan/githubs-css-performance  
 https://github.com/styleguide/css  
 http://smacss.com/