---
layout: post
title: Status projektu DataBoard [vol. 5]
author: Jacek Pospieszyński
date: '2017-05-14'
categories: [DSP2017, DataBoard]
---

W związku z tym, że nadal nie wiem jak rozwiązać problem z interfejsem użytkownika skupiłem się na backendzie. I dziś mogę zaprezentować zmiany, które zaszły w [aplikacji](https://github.com/pospieszja/DataBoard). Nie ma ich wiele, ale każdy postęp cieszy :)

<!--more-->
### Rejestracja użytkownika
Po pierwsze wzbogaciłem klasę ``UserService`` o nową metodę ``Register``, która pozwala na rejestrację użytkowników w systemie. Założenie jest takie, że w systemie mogą istnieć użytkownicy z unikalnym adresem email. Zatem jeśli nowy użytkownik użyje adresu email, który został już użyty przy rejestracji, wtedy aplikacja zwróci wyjątek. Następnie tworzę nową instancję klasy ``User`` i dodaje ją do bazy danych za pomocą repozytorium.

{% highlight csharp %}
public void Register(string email, string password)
{
    var user = _userRepository.Get(email);

    if(user != null)
    {
        throw new Exception($"User with {email} already exist.");
    }

    user = new User(email, password);
    _userRepository.Add(user);
}
{% endhighlight %}

### Zmiany w kontrolerze, kod statusu HTTP
Dalsze zmiany pojawiły się w kontrolerze obsługującym użytkowników ``UserController``.
Ze zmian kosmetycznych usunąłem prefiks ``/api``, który był zbędny. Obecnie atrybut obsługujący routing wygląda tak: ``[Route("/users")]``.

{% highlight csharp %}
public IActionResult Get()
{
    var users = _userService.GetAll();
    return Ok(users);
}

[HttpGet("{email}")]
public IActionResult Get(string email)
{
    var user = _userService.Get(email);
    if (user == null)
    {
        return NotFound();
    }
    return Ok(user);
}

[HttpPost]
public IActionResult Post([FromBody]CreateUser request)
{
    if (request == null)
    {
        return BadRequest();
    }

    _userService.Register(request.Email, request.Password);

    return Created($"/users/{request.Email}", request);
}
{% endhighlight %}


Po pierwsze każda metoda w tej chwili zwraca obiekt typu ``IActionResult`` jest to zgodne z dobrymi praktykami MVC i powoduje, że nie trzeba określać dokładnego zwracanego typu.

Po drugie w tej chwili każda metoda zwraca kod statusu HTTP, co jest zastosowaniem kolejnego dobrego wzorca, ponieważ będzie zwracany za każdym razem poprawny status kodu żądania HTTP.

Zgodnie z [tym](https://pl.wikipedia.org/wiki/Kod_odpowiedzi_HTTP) istnieje 5 podstawowych grup odpowiedzi HTTP:
* informacyjne,
* powodzenie,
* przekierowanie,
* błąd aplikacji klienta
* błąd serwera HTTP.

Oczywiście to programista decyduje o tym w jakiej sytuacji jaki kod powinien być zwrócony.
W moim przypadku dla pierwszego żadania zawsze zwracam ``Ok()``, co oznacza kod **200**, czyli żądanie zakończyło się powodzeniem. W drugim żądaniu, jeśli użytkownik o podanym adresie email nie został znaleziony za pomocą metody ``NotFound()``, które zwraca odpowiedź o kodzie **404**. W trzecim żądaniu zwracam ``BadRequest()`` jeśli polecenie nie zostanie poprawnie zinterpretowane - kod błędu **400** oraz ``Created()``, jeśli nowy użytkownik zostanie poprawnie utworzony, co równoznaczne jest z kodem **201**.

### Pierwsze żądanie POST
Brak interfejsu użytkownika spowodował, że musiałem znaleźć rozwiązanie na zrealizowanie żądania POST, dokładnie rzec biorąc rejestrację nowego użytkownika aplikacji. Jako narzędzie do testów sprawdził się [POSTMAN](https://www.getpostman.com/). Dzięku niemu zrealizowałem pierwsze żądanie POST, które obsługiwane jest przez aplikację poprawnie, oczywiście po drodze nie obyło się bez małych problemów. Istotny jest tutaj atrybut ``[FromBody]``, który mapuje obiekt JSON, na obiekt POCO. Jako dowód zrzut ekranu z POSTMANA, jak widać na ekranie cała operacja zakończyła się sukcesem, został zwrócony kod odpowiedzie **201**.

![Postman](/assets/2017-05-14-status-projektu-vol5/postman.png "Postman")