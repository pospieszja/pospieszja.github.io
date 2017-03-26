---
layout: post
title: Dr. Jekyll and Mr. Wordpress
author: Jacek Pospieszyński
date: '2017-03-26'
categories: [DSP2017, Jekyll]
---
Na [slacku](http://devspl.slack.com) dość często przewijał się temat **[Jekylla](https://jekyllrb.com/)**. Wiele osób wybrało go na starcie jako silnik swojego bloga, a inne porzuciły **[Wordpress](https://wordpress.org/)** na jego rzecz. Jako, że z konkursowym projektem jestem do tyłu nie pozostało mi nic innego jak... poświęcić parę dni i samemu przekonać się czy warto.

![logo jekyll](/assets/2017-03-26-dr-jekyll-and-dr-wordpress/jekyll-logo.png "Jekyll")

<!--more-->

### Czym jest Jekyll?
**Jekyll** to prosty generator statycznych stron HTML. Zatem nie potrzeba żadnego PHP ani żadnej bazy danych, co mocno upraszcza sprawę hostingu. Został napisany w **Ruby** przez [Toma Prestona-Wernera](https://en.wikipedia.org/wiki/Tom_Preston-Werner), współzałożyciela [GitHub](https://github.com/), co jest istotne ponieważ dzięki temu można za darmo hostować Jekylla na [GitHub Pages](https://pages.github.com/). Wystarczy utworzyć konto i repozytorium na GitHub, a potem "pushować" swoje blogaski.

### Jak zacząć?
Można zacząć na dwa sposoby. Jeżeli chcemy zacząć od samych podstaw to rozpoczynamy od zabawy z **Ruby**, potem instalacja odpowiednich gemów i utworzenie domyślnej struktury projektu. Ponieważ nie mam zainstalowanego Ruby i w najbliższym czasie nie przewiduje takiej potrzeby, to od razu przeszedłem do drugiego sposobu. Gdyby ktoś był zainteresowany szerzej sposobem numer jeden, zapraszam [tutaj](https://jekyllrb.com/docs/quickstart/).

Drugi sposób to wykorzystanie [GitHub Pages](https://pages.github.com/). Wpisujemy w google frazę "jekyll themes" i znajdujemy szablon, który spełnia nasze wymagania. Ja do tego wykorzystałem [JekyllThemes](https://jekyllthemes.io/). W większości przypadków szablon można pobrać lub "sforkować" (jakiego polskiego słowna można tu użyć??) go do swojego repozytorium.

Aby móc skorzystać z magii **GitHub Pages** nazwę repozytorium Jekylla trzeba przemianować na `username.github.io`, następnie w ustawieniach GitHub Pages wskazać z której gałęzi ma korzystać. I teraz najfajniejsza sprawa. Cała magia wykonuje się w tle, czyli statyczna strona na podstawie szablonu generuje się samoczynnie po stronie GitHub Pages bez naszej ingerencji. Każdy "commit" wywołuje uruchomienie generatora stron statycznych.

"Jądrem", że tak się wyrażę, Jekylla jest plik `_config.yml`, w którym konfiguruje się najważniejsze parametry strony: adres, nazwa, opis, sposób tworzenia linków itp.
Również można tam dodwać rozszerzenia Jekylla, najważniejsze to:
* jekyll-seo-tag - usprawnia SEO,
* jekyll-sitemap - tworzy mapę strony, którą możemy przekazać dla robocików Google.

Dla każdego bloga bardzo ważny jest system komentarzy. Jekyll nie korzysta z bazy danych i nie ma tym samym wbudowanej ich obsługi. Ale można wykorzystać z powodzeniem do tego celu [DISQUS](https://disqus.com/). Natomiast dla każdego technicznego bloga ważne jest kolorowanie składni. Jekyll wykorzystuje do tego celu [Rouge](https://github.com/jneen/rouge), obsługuje około 100 języków programowania. Dodatkowo wygenerowany przez niego HTML jest kompatybilny z generowanym przez [Pygments](http://pygments.org/) dzięki czemu [w sieci](http://richleland.github.io/pygments-css/) można znaleźć wiele styli CSS, którymi można upiększyć kodzik.

### Markdown
Aby Jekyll mógł wygenerować statyczną stronę najpierw trzeba przygotować dla niego "posiłek" w postaci pliku tekstowego zgodnego z [Markdown](https://daringfireball.net/projects/markdown/). Zatem, jeżeli chcemy dodać nowego posta nie piszemy niczego w HTML, nie korzystamy z jakichś edytorów wysiwyg. Otwieramy edytor tekstu, najlepiej wspierający składnię Markdown. Ja polecę do tego celu **Visual Studio Code**, który również pozwala na podgląd pliku.

Jeżeli chcemy dodać nagłówek wystarczy użyć `#` do tego celu i wyniku otrzymamy `<h1>`. Zwielokrotnienie liczby hashy, tworzy nagłówki kolejnych stopni. Chcemy użyc bolda, używamy **, itd. Po więcej przykładów odsyłam do [dokumentacji składni](https://daringfireball.net/projects/markdown/syntax). Jest to proste i intuicyjne :)

### Własna domena
Co jeszcze jest pięknego w GitHub Pages? Otóż możemy do naszej strony `username.github.io` podpiąć swoją własną domenę i tak też ja zrobiłem :) Wystarczy do głównego katalogu repozytorium dodać plik tekstowy o nazwie `CNAME`, a w nim wpisać nazwę swojej domeny. Następnie w konfiguracji swojej domeny dodać nowy rekord CNAME wskaujący na GitHub Pages, w moim przypadku wyglądało to tak: `blog.pospieszynski.net CNAME pospieszja.github.io`.

Jedna istotna sprawa. Ja swoją domenę zarejestrowałem na OVH, natomiast korzystam z hostingu na MyDevil. I ustawiłem dla swojej domeny serwer DNS na ten z MyDevil, tym samym musiałem dodać rekord CNAME w ustawieniach DNS na MyDevil, nie na OVH. Taka mała uwaga, ponieważ ja straciłem na to kilka godzin, nie wiedząc czemu nie działało przekierowanie domeny!

### Lepiej, gorzej?
Od paru dni ten blog korzysta z Jekylla. Jak się z tym czuje? Na pewno inaczej. To co dla mnie jest najważniejsze to kontrola nad blogiem. Tutaj w łatwy sposób mogę zmodyfikować szablon, rozumiem strukturę plików. Worpdressa nie potrafiłbym, przynajmniej bez poświęcenia dłuższego czasu, zmodyfikować pod swoje potrzeby. Dodatkowo PHP dla mnie to czarna magia. Wordpress był dla mnie zbyt dużą kobyłą, korzystałem tylko z kilku podstawowych funkcjonalności, a z Jekylla korzystam w pełni - jest tak jak lubię prosto i minimalistycznie.