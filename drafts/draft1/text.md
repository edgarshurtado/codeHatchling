One year ago I finished the project I'm featuring in this post. I know it's been a while, however there's a good reason why bringing this project now. In my year of professional career a cycle has been closed. Since I relocate in Valladolid, I've been working mainly as a PHP developer. Been working with PHP has been great, and I've learned to love the language. Nevertheless, I missed JavaScript a lot. Luckily, an opportunity was oppened in front of me. A new team of developers was formed with a full new stack of technologies. Currently, I'm already developing with this team and the Front End framework they use is Vue.js.

Is this framework which made me think about posting my Vocational Training last project. Because, even though I'm very new with Vue.js it is familiar to me. Its phylosophy is working with web components, and that's exactly what I made 2 years ago. A complete website organized exclusively in web components.

#

What's exactly a web component? Well, accordingly to MDN:

> Web Components consists of several separate technologies. You can think of Web Components as reusable user interface widgets that are created using open Web technology. They are part of the browser, and so they do not need external libraries like jQuery or Dojo. An existing Web Component can be used without writing code, simply by adding an import statement to an HTML page. Web Components use new or still-developing standard browser capabilities.

In my own words, I'd say that web components allows us to extend the HTML language, creating our own elements keeping all the logic and styling encapsulated. A great way of thinking about this is as I've heard a couple of times to Monica Dinculescu. What if we could implement our own input element? Well, that's exactly what web component are for (I recommend Monica's blog-post [An intro to web components with otters](https://meowni.ca/posts/web-components-with-otters/) for further reading). 

I just realized that I haven't explained the project itself, its goals. This project called *Bioinfo-apps* was made for my internship at the CIPF, in a Bioinformatics department (You can read about my first week with some thoughts about Polymer [here](http://edgarsh.es/thougts/cipf-internship-week-1-meeting-polymer/)). So, this department had a very old fashioned web site to show the applications they offer to the scientific comunity. My goal was to improve the appearence of the site and the proof of concept I stated before. We can build from the ground up an entire site using web components.

For this project I used the Polymer library, a Google's light-weight library for creating web components. This library is great if you want to try for yourself programming some web components for a couple of reasons.

1. Makes a lot easier to create a brand new component, avoiding a great deal of boilerplate code you'd have to write otherwise.
2. It comes with a polyfill that enables web components specifications in the browsers which doesn't have native support yet.

Web components are composable, which means that we can put an element inside another element, inside another and so on... Just like we do with tha tags <table> <thead> <tbody> for example. So the idea with the project I made, was to compose all the logic of the aplication by gathering web components and creating a new one that wraps the formers controlling the interactions between them. I'd repeat this process until I had a unique component. A good simile for the project would be a matrioska.

The main component, the one who rules them all and in the darkness bind them, is <bioinf-app>. This component acts as a state manager for the app and syncs data between an external source and 2 main components:

* The <bz-collection> is a navigation bar inspired in the one Blizzard has on its launcher. This component lists all the routes available in the site and when a user clicks one, it emits an event that will be recieve by <bioinf-app> and the state of the second main web component will be updated accordingly
* The second big component is <app-info> that has all the logic and styles to nicely show any of the apps the department has. This component is the bigger and I built several web components to compose it. Like the search bar and, the one I put the bigger effort and made open source and reusable, <img-slider> (Which you can check [on my GitHub repository](https://github.com/edgarshurtado/img-slider))

If you feel curious about the project I've uploaded it on this site (just go to [edgarsh.es/projects/bioinfo-apps](http://edgarsh.es/projects/bioinfo-apps)) and if you want the code and play with it, is available as well on [github](https://github.com/edgarshurtado/bioif-apps). However, the downside of making it public now is that it comes completely outdated. Currently Polymer is in its second version and I made all this with version 1. Anyway, I think is interesting enough and I put so much effort on it.
