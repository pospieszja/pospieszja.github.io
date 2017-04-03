---
layout: post
title: Status projektu DataBoard [vol. 2]
author: Jacek Pospieszyński
date: '2017-04-03'
categories: [DSP2017, DataBoard, .NETCore]
---

Nie pisałem do tej pory wiele o projekcie, bo tak naprawdę nie było o czym. Skrótowo status projektu można podsumować: brak postępów. Niestety, ale prowadzenie bloga pochłania wiele cennego zasobu, czyli wolnego czasu. Niemniej jednak w projekcie pojawiły się zmiany i dziś w kilku słowach o nich.

![The Walking Dead](https://media.giphy.com/media/7gufvFLL3gxa/giphy.gif "random zombie")

<!--more-->

### Zmiany w architekturze
Ślędzę od samego początku kurs [Piotrka Gankiewicza](http://piotrgankiewicz.com/) - [Becoming a software developer](http://piotrgankiewicz.com/courses/becoming-a-software-developer/), który w tym miejscu gorąco polecam, bo jest to według mnie kawał dobrze wykonanej pracy. W [epizodzie X](http://piotrgankiewicz.com/2017/03/30/becoming-a-software-developer-episode-x/) (nareszcie) rozpoczął budowę aplikacji i opowiada w przystępny sposób o wzorcach architektonicznych. Tym samym zmobilizował mnie do podjęcia kroków naprawczych we własnym projekcie.

Po pierwsze utworzyłem nowego brancha o jakże wiele mówiącej nazwie ``develop``, w którym znajdują się wszystkie zmiany bieżące, jeżeli coś przetestuje i będzie działać poprawnie trafi wtedy do brancha ``master``. W tym momencie cały projekt został podzielony na trzy mniejsze zgodnie ze wzorcem [Onion Architecture](http://jeffreypalermo.com/blog/the-onion-architecture-part-1/):
* DataBoard.Core
* DataBoard.Infrastructure
* DataBoard.Web
* DataBoard.Tests

Od razu, na wstępie, napiszę w jaki sposób można dodać referencję do własnych projektów bez udziału Visual Studio. Sam spędziłem wiele czasu na znalezieniu tej informacji, być może tworzyłem błędne frazy zapytań w Google. Należy otworzyć plik *.csproj a w nim dodać (tak to wyglądało w moim przypadku) następująca linię: ``<Import Project="..\DataBoard.Core\DataBoard.Core.csproj"/>``. Po zapisaniu pliku można wywołać polecenie ``dotnet restore`` i sprawdzić tym samym poprawność wprowadzonej zmiany.

``DataBoard.Core`` niezależny projekt, który jest nieświadomy istnienia pozostałych. Będą się w nim znajdowały klasy domenowe wraz z repozytoriami, które będą wstrzykiwane do pozostałych projektów.

``DataBoard.Infrastructure`` projekt powiązany z ``DataBoard.Core`` w nim będzie znajdować się interfejs do bazy danych (myślę o zastosowaniu [Dappera](https://github.com/StackExchange/Dapper) jako [ORM](https://en.wikipedia.org/wiki/Object-relational_mapping), ale to jeszcze wymaga przemyślenia), wszystkie usługi CRUD oraz anemiczne klasy [DTO](https://en.wikipedia.org/wiki/Data_transfer_object), które będą przechowywały dane, bez żadnych metod, logiki itp. Żeby ręcznie nie mapować klas domenowych na klasy DTO zastosuję bibliotekę [AutoMapper](http://automapper.org/), która proces zautoamtyzuje i uchroni (w założeniu) aplikację od błędów.

``DataBoard.Web`` gwóźdź programu powiązany tylko z ``DataBoard.Infrastructure``, nieświadomy gdzie i w jaki sposób przechowywane są dane. Na ten moment jest to projekt ASP.NET Core MVC, ale myślę, co by nie przekształcić go na API. Wówczas chciałbym w kontrolerze zwracać prostego JSONa, który byłby spożywany przez zdobywającą co raz większą popularność [Vue.js](https://vuejs.org/). Dlaczego Vue.js, a nie jak na początku zakładałem [React](https://facebook.github.io/react/)? A no dlatego, że ponoć entry level jest dużo niższy dla osób nie znających JavaScript. Zatem coś dla mnie ;-)

``Databoard.Tests`` na ten moment projekt jest zupełnie pusty, ale mam nadzieję, że pusty nie pozostanie, bo mam nadzieję napisać w końcu pierwszy test :D

### Tylko tyle
Niestety przez jakieś dwa tygodnie udało mi się tylko tyle zrealizować. To nie jest tak, że mało robię, nie, nie ! :) Po prostu wiele czas poświęcam na czytanie i oglądanie materiałow na YT związanych z projketem. Mam nadzieję, że kolejny wpis o projekcie będzie bogatszy w "mięso". A teraz w ramach relaksu czas na finałowy odcinek sezonu **The Walking Dead** :D