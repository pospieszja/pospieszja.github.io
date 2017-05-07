---
layout: post
title: Status projektu DataBoard [vol. 4]
author: Jacek Pospieszyński
date: '2017-05-07'
categories: [DSP2017, DataBoard]
---

Za mną długi weekend. Czy był owocny pod względem **DSP2017**? Niestety nie. Choć z drugiej strony mogę to zrzucić na to, że między świętami normalnie pracowałem i nie mogłem się zająć projektem. Być może nie spędziłem tego czasu w edytorze kodu, natomiast parę przemyśleń na temat projektu przeszło przez głowę, szczególnie pod postacią pytań.

<!--more-->

Pierwsza rzecz jaka nastręcza mi w tym momencie trudność to jak połączyć Vue.js z całym projektem ASP.NET Core MVC. Czy [DataBoard](https://github.com/pospieszja/DataBoard) ma być projektem **SingleApplicationPage**? A jeśli tak to w jaki sposób go takim uczynić? Na początku myślałem, że utworzę statyczne widoki HTML, które będą kontrolowane przez ASP.NET, w każdym widoku będzie znajdował się komponent (oddzielny dla każdego widoku) Vue.js, który będzie wywoływał API - pobierał, przetwarzał lub zapisywał dane. Z drugiej strony mógłbym utworzyć kolejny projekt, który odpowiedzialny byłby tylko i wyłącznie za front-end. Vue.js mogłoby generować HTML na podstawie routingu bez przeładowania strony i tym samym aplikacja front-endowa stałaby się SPA.

Druga rzecz to obsługa baz danych, które użytkownicy będą dodawać celem zbierania statystyk lub też wyświetlania informacji o nich. Największy problem to provider bazy danych. Dla każdego typu bazy danych musiałbym mieć osobnego providera oraz tym samym inny zbiór zapytań SQL, które zwracałyby informacje o metadanych bazy danych. Im bardziej o tym myślę, tym więcej problemów mi to nastręcza, a jeszcze nie doszedłem do fazy implementacji jakby nie było najgłówniejszej funkcjonalności aplikacji.

Swoją drogą czytając konkursowego Slacka wyraźnie widać spadek aktywnych uczestników. Co prawdopodobnie przenosi się na brak aktywności na blogach oraz na commitach. Szkoda, bo do konkursu zgłosiło się około 1000 osób, wiele projektów było naprawdę ciekawych
