---
layout: post
title: AutoMapper
author: Jacek Pospieszyński
date: '2017-04-06'
categories: [DSP2017, DataBoard, .NETCore]
---

Wraz z nowym podejściem architektonicznym zaszła przepisywania obiektów klas domenowych na obiekty typu [DTO](https://en.wikipedia.org/wiki/Data_transfer_object). W celu uproszenia sobie sprawy zastosowałem bilbiotekę [AutoMapper](http://automapper.org/), która będzie głównym gościem tego posta.

![AutoMapper](/assets/2017-04-06-automapper/automapper_logo.png "automapper")

<!--more-->

### Jak zwierzęta?

Gdy zajdzie potrzeba przepisywania/mapowania obiektów domeny na DTO można zastosować podejście ręczne. Jako przykład wezmę oklepaną do bólu klasę ``User`` oraz ``UserDto``. 

{% highlight csharp %}
public class User
{
    public Guid Id { get; private set; }
    public string Email { get; private set; }
    public string Password { get; private set; }
    public bool IsActive { get; private set; }
    public DateTime CreatedAt { get; private set; }
    public DateTime UpdatedAt { get; private set; }

    //there is more
}
{% endhighlight %}

{% highlight csharp %}
public class UserDto
{
    public Guid Id { get; set; }
    public string Email { get; set; }
}
{% endhighlight %}

Ja w swojej aplikacji chcę zwracać w swojej metodzie ``GetUser`` obiekt typu ``UserDto`` i aby to zrobić muszę recznie dokonać przepisywania właściwości.
Obiekt typu ``User`` pobierany jest z repozytorium projektu ``Databoard.Core``.

{% highlight csharp %}
public UserDto GetUser(Guid id)
{
    var user = _userRepository.GetUser(id);
    var userDto = new UserDto();
    userDto.Id = user.Id;
    userDto.Email = user.Email;

    return userDto;
}
{% endhighlight %}

Przy każdej zmianie klasy typu DTO będą musiał dokonywać zmiany w przypisywaniu wartości - co jest czasochłonne i może prowadzić do generowania błędów.
Cały proces można zautomatyzować przy użyciu biblioteki **AutoMapper**.

Prawdopodobnie powyższy kod można byłoby mapować korzystając z mechanizmu refleksji, jednak nie jest to łatwe i nie ma sensu wyważać otwartych drzwi.

### A można automatycznie!
[AutoMapper](http://automapper.org/) mała, lekka biblioteka, która pozwala na pozbycie się powyższego kodu służącego do przepisywania wartości, a zostawia czas programiście na zajmowanie się prawdziwymi problemami :-) Napisana przez [Jimmy Bogard](https://jimmybogard.com/), źródła dostępne są na [GitHub](https://github.com/AutoMapper/AutoMapper) wraz z wyczerpującą [wiki](https://github.com/AutoMapper/AutoMapper/wiki).

Aby rozpocząć pracę z bilbioteką należy ją zainstalować korzystając z Nuget - ``dotnet add package AutoMapper``. W moim przypadku dodałem referencję dla dwóch projektów ``DataBoard.Infrastructure`` oraz ``DataBoard.Web`` dla wersji 6.0.2.

W projekcie ``DataBoard.Infrastructure`` utworzyłem statyczną klasę ``AutoMapperConfig`` wraz ze statyczną metodą ``Initialize``ze zwracanym interfejsem ``IMapper``, który będzie wstrzykiwany przed wywołaniem mapowania. ``cfg.CreateMap<User, UserDto>();`` definiuje z źródło i cel mapowania.

{% highlight csharp %}
public static class AutoMapperConfig
{
    public static IMapper Initialize()
    {
        var config = new MapperConfiguration(cfg =>
        {
            cfg.CreateMap<User, UserDto>();
        });

        return config.CreateMapper();
    }
}
{% endhighlight %}

W ``Databoard.Web`` muszę w konfiguracji dodać ``services.AddSingleton(AutoMapperConfig.Initialize());`` ponieważ konfiguracja jest jedna i będzie niezmieniona przez cały cykl życia aplikacji jako procesu.

Teraz można użyć biblioteki zamiast ręcznego przepisywania właściwości. Należy pamiętać o wstrzyknięciu zależności ``IMapper`` w konstruktorze klasy, w której zdefiniowana jest metoda, w moim przypadku w klasie ``UserService``.

{% highlight csharp %}
public UserDto GetUser(Guid id)
{
    var user = _userRepository.GetUser(id);
    return _mapper.Map<User,UserDto>(user);
}
{% endhighlight %}

Powyżej przedstawiam najprostsze użycie **AutoMappera**. Pozwala ona na dużo więcej jak chociażby na mapowanie kolekcji, przypisywanie wartości domyślnych, czy też omijanie właściwości przy mapowaniu, po więcej odsyłam do [wiki](https://github.com/AutoMapper/AutoMapper/wiki).