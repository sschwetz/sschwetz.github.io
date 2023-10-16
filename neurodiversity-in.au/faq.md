---
#**************************************
lang: en-AU
layout: page
social-share: true
comments: true

typora-copy-images-to: ../assets/img/${filename}
typora-root-url: ../
#**************************************

#*************************************
#fill this if you have renamed the page
redirect_from:
#*************************************

title: Neurodiversity FAQ
subtitle: The answers to your hard hitting questions
author: Stephen
updated: 
cover-img: 
thumbnail-img: 
full-width: false

share-title: 
share-description: 
share-img: 

tags:
  -
---



{% for faq in site.faq-neurodiversity-in.au %}
  <div class="faq">
    <h2>{{ faq.question }}</h2>
    {{ faq.answer }}
  </div>
{% endfor %}