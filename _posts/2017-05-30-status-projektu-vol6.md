---
layout: post
title: Status projektu DataBoard [vol. 6]
author: Jacek Pospieszyński
date: '2017-05-27'
categories: [DSP2017, DataBoard, Vuejs]
---
Udało mi się poznać podstawy [Vue.js](https://vuejs.org), zatem nadszedł odpowiedni moment, żeby go połączyć z konkursową [aplikacją](https://github.com/pospieszja/DataBoard). Innymi słowy łączę ASP.NET Core z Vue.js. Czy było łatwo? Dowiecie się czytając dalej.

<!--more-->
### JavaScriptServices
Udało mi się znaleźć [rozwiązanie](https://blog.kloud.com.au/2017/02/14/running-vuejs-on-aspnet-core-apps/), które pozwala na utworzenie aplikacji SPA opartej o ASP.NET Core na backendzie oraz Vue.js na frontendzie. Niestety rozwiązanie to okazało się dosyć przytłaczające, ponieważ opierało się na [Webpacku](https://webpack.github.io/), a wtedy wiedziałem o nim tylko tyle, że używają go frontendowcy do automatyzacji tasków.

Zacząłem zgłębiać to rozwiązanie i okazało się, że wszystko zmierza do połączenia Webpacka z [JavaScriptServices](https://github.com/aspnet/JavaScriptServices). JavaScriptServices jest zestawem narzędzi składającym się z następujących komponentów:
* Microsoft.AspNetCore.NodeServices,
* Microsoft.AspNetCore.SpaServices,
* Microsoft.AspNetCore.AngularServices.

Dodatkowo istniała jeszcze Microsoft.AspNetCore.ReactServices, ale cała funkcjonalość została przeniesiona do Microsoft.AspNetCore.SpaServices.

Na podstawie [dokumentacji Microsoftu](https://blogs.msdn.microsoft.com/webdev/2017/02/14/building-single-page-applications-on-asp-net-core-with-javascriptservices/) okazało się, że całość można sobie baaaardzo usprawnić poprzez instalację szablonów aplikacji SPA ``dotnet new --install Microsoft.AspNetCore.SpaTemplates::*``.

![dotnet spa templates"](/assets/2017-05-30-status-projektu-vol6/dotnet-spa-templates.png "dotnet spa templates")

Jak widać powyżej pojawiło się kilka dodatkowych szablonów, również ten interesujący mnie, po wywołaniu ``dotnet new vue`` CLI tworzy nowy projekt SPA oparty o Vue.js.
Po utworzeniu projektu uruchomiłem aplikację ``dotnet run`` wszystko działa poprawnie. Niestety absolutnie nie wiedziałem co się wydarzyło pod spodem. Dodatkowo wszystkie skrypty JavaScript zostały zastąpione TypeScript, którego nie znam.


### Razor + Vue.js
Niestety, ale musiałem znaleźć inne rozwiązane, ponieważ w gotowym szablonie działo się za dużo "magii". Najprościej było wykorzystać funkcjonalność Razora i posypać ją szczyptą Vue.js.

W widoku ``_Layout.cshtml`` dodałem framework Vue.js wraz z bilbioteką [axios](https://github.com/mzabriskie/axios), która radzi sobie z żądaniami HTTP. Co prawda istnieje również [vue-resource](https://github.com/pagekit/vue-resource), które ma taką samą funkcjonalność, jednak [sam twórca Vue.js](https://medium.com/the-vue-point/retiring-vue-resource-871a82880af4) nie zaleca jej do obsługi żądań HTTP.

{% highlight html %}
<script src="https://unpkg.com/vue"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="~/js/app.js"></script>
{% endhighlight %} 

W pliku ``app.js`` wywołuje żądanie GET na utworzonym wcześniej kontrolerze API. Zwrócony obiekt JSON zostaje przypisany do tablicy ``users``. Należy zwrócić uwagę na funkcję ``mounted()``, która jest wbudowanym zdarzeniem cyklu życia instancji Vue.js [(***ang. instance lifecycle hooks***)](https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks)

{% highlight javascript %}
var app = new Vue({
  el: '#app',
  data: {
    users: []
  },
  mounted() {
    axios.get('/api/users').then(response => { this.users = response.data })
  }
})
{% endhighlight %} 

Finalnie w widoku ``Index.cshtml`` dodałem wyświetlenie emaili wszystkich użytkowników znajdujących się w bazie danych.

{% highlight html %}
<h1>List of users</h1>
<ul v-if="users && users.length">
    <li v-for="user in users">
        <p>{{ user.email }}</p>
    </li>
</ul>
{% endhighlight %} 