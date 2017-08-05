One year ago I finished the project I'm featuring in this post. I know it's been a while, however there's a good reason why bringing this project now. In my year of professional career a cycle has been closed Since I relocate in Valladolid, I've been working mainly as a PHP developer. Been working with PHP has been great, and I've learned to love the language. Nevertheless, I missed JavaScript a lot. Luckily, an opportunity was oppened in front of me. A new team of developers was formed with a full new stack of technologies. Currently, I'm already developing with this team and the Front End framework they use is Vue.js.

Is this framework which made me think about posting my Vocational Training last project. Because, even though I'm very new with Vue.js it is familiar to me. It's phylosophy is working with web components, and that's exactly what I made 2 years ago. A complete website organized exclusively in web components.

#

I've mentioned several time the word "web component". However what's exactly a web component? Well, accordingly to MDN:

> Web Components consists of several separate technologies. You can think of Web Components as reusable user interface widgets that are created using open Web technology. They are part of the browser, and so they do not need external libraries like jQuery or Dojo. An existing Web Component can be used without writing code, simply by adding an import statement to an HTML page. Web Components use new or still-developing standard browser capabilities.

Web components allows us to extend the HTML language, creating our own elements keeping all the logic and styling encapsulated. A great way of thinking about this is as I've heard a couple of times to Monica Dinculescu. What if we could implement our own input element? That's exactly a web component (For further reading I recommend Monica's blog-post [An intro to web components with otters](https://meowni.ca/posts/web-components-with-otters/)). 

For this project I used the Polymer library, the Google's light-weight library for creating web components. This library is great if you want to try for yourself programming some web components for a couple of reasons.

1. Makes a lot easier to create a brand new component, avoiding a great deal of boilerplate code you'd have to write otherwise.
2. It comes with a polyfill that enables web components specifications in the browsers which doesn't have native support yet.

The web components specification stills on development and all major web browser vendors doesn't support it (yet).
