---
layout: post
title: Vue.js - wprowadzenie
author: Jacek Pospieszyński
date: '2017-05-21'
categories: [DSP2017, VueJS]
---
W [projekcie konkursowym](https://github.com/pospieszja/DataBoard) początkowo na frontendzie chciałem zastosować Reacta. Jednak stwierdziłem, że będę mierzył z armaty do muchy. Znalazłem lżejsze rozwiązanie pasujące do założeń projektu, czyli [Vue.js](https://vuejs.org/). Do tej pory nie miałem z nim żadnej stycznośći, zatem zanim będę mógł go zastosować będę musiał się go nauczyć. I dziś wpis będzie o wprowadzeniu do [Vue.js](https://vuejs.org/).

![vuejs](/assets/2017-05-21-vuejs-wprowadzenie/vuejs-logo.png "vuejs")

<!--more-->

### Vue.js
Tak, [Vue.js](https://vuejs.org/), jest kolejną bilblioteką JavaScript, jego twórcą jest [Evan You](https://github.com/yyx990803), który pracował wcześniej dla Google nad projektem [AngularJS](https://angularjs.org/). Jego zamiarem było stworzenie nowej, lekkiej biblioteki JS, ale z najlepszymi cechami AngularJS.
**Vue.js** pozwala na budowę prostych, interaktywnych aplikacji webowych z wykorzystaniem wzorca MVC. Niewątpliwą zaletą **Vue.js** jest to, że nie trzeba posiadać dużej wiedzy na temat samego JavaScript, aby napisać podstawową aplikację typu lista ToDo.

### Instalacja
Istnieje kilka sposobów na instalację lub też użycie najnowszej wersji Vue.js w projekcie. Najprostszy z nich to dołączenie skryptu do ``<script>``:
* pobranie skryptu w wersji [deweloperskiej](https://vuejs.org/js/vue.js) lub [produkcyjnej](https://vuejs.org/js/vue.min.js),
* skorzystanie z CDN [https://unpkg.com/vue](https://unpkg.com/vue).
W obu powyższych przypadkach zmienna ``Vue`` zostanie zajerestrowana jako globalna.

Drugi sposób zalecany w rozwiązaniach biznesowych to skorzystanie z [vue-cli](https://github.com/vuejs/vue-cli), które pozwala na zainicjowanie szkieletu projektu typu SPA.

{% highlight bash %}
# install vue-cli
$ npm install --global vue-cli
# create a new project using the "webpack" template
$ vue init webpack my-project
# install dependencies and go!
$ cd my-project
$ npm install
$ npm run dev
{% endhighlight %} 

### Prosta aplikacja
Utworzyłem prosty plik ``index.html``, który wygląda następująco

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
        <div id="app">

        </div>    
    </body>
    <script src="https://unpkg.com/vue"></script>
    <script type="text/javascript" src="js/app.js"></script>
</html>        
{% endhighlight %} 

Jak widać wyżej Vue.js pobieram z repozytorium CDN. Dołączyłem również plik ``app.js``, który zawiera całą logikę odpowiedzialną za interakcję z Vue.js. Istotna jest tutaj kolejność dodawania skryptów, pierwszy musi być Vue.js, ponieważ ``app.js`` jest od niego zależny.

{% highlight javascript %}
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello from Vue!'
  }
})
{% endhighlight %} 

Co się tutdaj dzieje. Otóż tworzę nową instancję obiektu ``Vue``, w konstruktorze przypisuje do właściwości ``el`` selektor CSS, do którego będzie "przykotwiczona" cała aplikacja. Następnie w sekcji ``data`` definiuje zmienną ``message``.

Teraz dokonując małej zmiany w ``index.html`` za pomocą interpolacji mogę wyświetlić zmienną ``message`` zdefiniowaną wcześniej w ``app.js``.

{% highlight html %}
<div id="app">{{message}}</div>
{% endhighlight %}         

Powyżej pokazałem najprostsze użycie Vue.js. Jest to tylko wierzchołek podstawowych możliwości biblioteki, która pozwala m.in. na automatyczne przypisywanie modeli do danych, pętle, warunki, tworzenie szablonów itp.

Postaram się utworzyć mini-cykl, w którym będę pokazywał i objaśniał kolejne funkcje **Vue.js**.