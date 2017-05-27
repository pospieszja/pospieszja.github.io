---
layout: post
title: Vue.js - podstawowe dyrektywy
author: Jacek Pospieszyński
date: '2017-05-27'
categories: [DSP2017,VueJS]
---
Dziś kontynuacja serii o [Vue.js](https://vuejs.org), omówię dyrektywy, których używa się najczęściej podczas pracy z tym frameworkiem.

<!--more-->
### Czym są dyrektywy Vue.js?
Dyrektywy to nic innego jak atrybut, który dodajemy do znaczników HTML, z tym, że nie są one częścią standardu HTML, a są intepretowane bezpośrednio przez **Vue.js**. Moża również powiedzieć, że są mini-funkcjami, które zmieniają zachowanie i strukturę [DOM](https://pl.wikipedia.org/wiki/Obiektowy_model_dokumentu). Każda dyrektywa rozpoczyna się od prefiksu ``v-``, po którym dodajemy nazwę dyrektywy.

### v-text
Podstawowa dyrektywa, której użyłem w niejawny sposób w [poprzednim odcinku](https://blog.pospieszynski.net/2017/05/21/vuejs-wprowadzenie), dodaje tekst (ze zmiennej podaje jako parametr atrybutu) wewnątrz elementu, do którego została przypisana dyrektywa. Dyrektywy ``v-text`` używa się niejawnie stosując zapis klamrowy (***mustache tag***) i w gruncie rzeczy daje taki sam rezultat.

{% highlight html %}
<div id="app">
  <span>{% raw %} {{message}} {% endraw %}</span>
  <span v-text="message"></span>
</div>    
{% endhighlight %} 

{% highlight javascript %}
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
{% endhighlight %} 


### v-html
Dyrektywa ``v-html`` jest bardzo podobna do ``v-text``, z tą różnicą, że pozwala na przekazanie znaczników HTML, które są następnie interpretowane przez przeglądarkę internetową. W pierwszym przypadku zostanie zwrócony dokładnie i dosłownie tekst kryjący się w zmiennej ``message``, natomiast w drugim tylko **Hello from HTML**.

{% highlight html %}
<div id="app">
  <span v-text="message"></span>
  <span v-html="message"></span>
</div>  
{% endhighlight %} 

{% highlight javascript %}
var app = new Vue({
  el: '#app',
  data: {
    message: "<strong>Hello from HTML</strong>"
  }
})
{% endhighlight %} 


### v-show
``v-show`` jest dyrektywą warunkową, która po przyjęciu argumentu, który jest prawdą, spowoduje, że element do którego jest przypisana zostanie wyświetlony użytkownikowi. W przypadku fałszu do elementu zostanie dodany styl CSS ``display:none``. Dyrektywa ``v-show`` **nie usuwa elementu z DOM**.

{% highlight html %}
<div id="app">
  <p v-show="enoughMoney">Buy it</p>
</div>  
{% endhighlight %} 

{% highlight javascript %}
var app = new Vue({
  el: '#app',
  data: {
    enoughMoney: true
  }
})
{% endhighlight %} 


### v-if, v-else
Następną dyrektywą warunkową jest ``v-if``. Na zewnątrz działa tak samo jak ``v-show``, z tą różnicą, że jeżeli warunek nie jest spełniony to element do którego się odnosi, **nie zostanie dodany do DOM**. ``v-else`` nie istnieje samodzielnie i zawsze występują w parze z ``v-if``, jest wykonywana gdy pierwszy warunek nie został spełniony.

{% highlight html %}
<div id="app">
  <p v-if="enoughMoney">Buy it</p>
  <p v-else>You don't have enough money</p>
</div>  
{% endhighlight %} 

{% highlight javascript %}
var app = new Vue({
  el: '#app',
  data: {
    enoughMoney: true
  }
})
{% endhighlight %} 


### v-pre
Pozwala na wyświetlenie tekstu zawierającego znaki ``{{`` oraz ``}}``, interpreter Vue.js ominie wszystko, co znajduje się wewnątrz taga HTML (oraz jego dzieci) opatrzonego tą dyrektywą - zostanie zwrócony pre-formatowany tekst.

{% highlight html %}
<div id="app">
  <p v-pre>{% raw %} {{ message will be not compilated }} {% endraw %}</p>
</div>  
{% endhighlight %} 


### v-once
Dyrektywa ``v-once`` powoduje, że element (oraz elementy podrzędne) zostanie przetworzony przez Vue.js tylko raz. Jakakolwiek zmiana wartości zmiennej ``message`` za pomocą konsoli czy też kodu JavaScript nie spowoduje, że zostanie ona przetwrzona i zostanie wyświetlona jej zaktualizowana wartość. Warto pamiętać, że wartość zmiennej zostanie zmieniona, natomiast nie nastąpi odświeżenie elementów DOMu.

{% highlight html %}
<div id="app">
  <p v-once>{% raw %} {{message}} {% endraw %}</p>
</div>  
{% endhighlight %} 

{% highlight javascript %}
var app = new Vue({
  el: '#app',
  data: {
    message: 'This message is genreated only once'
  }
})
{% endhighlight %} 


### To nie wszystko
Istnieje jeszcz kilka dyrektyw, które są bardziej zaawansowane, m.in.: ``v-for``, ``v-model``, ``v-bind``, w związku z tym wymagają szerszego, dokładniejszego opisu, który zostanie przygotowany w kolejnych wpisach.