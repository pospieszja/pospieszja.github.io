---
layout: post
title: Testy, testy, testy
author: Jacek Pospieszyński
date: '2017-05-16'
categories: [DSP2017, DataBoard]
---
W końcu napisałem swoje pierwsze testy. Może nie pierwsze testy w życiu, bo kiedyś na studiach miałem okazję używać asercji w testach jednostkowych, ale to nie były wyrafinowane przykłady, ot taka wzmianka. A dziś pełen profesjonalizm - testy integracyjne w natarciu, a może EndToEnd, tu nie mam pewności ;-)

<!--more-->

Celem było napisanie testów sprawdzająych poprawność wywołania API dla ``UserController``. Po pierwsze utworzyłem nowy projekt o nazwie ``DataBoard.Tests.EndToEnd`` za pomocą polecenia ``dotnet new xunit``. Tym samym jako bibliotekę wspomagającą testowanie wskazałem [xUnit](https://xunit.github.io/). Dodatkowo aby móc skorzystać z serwera testowego dodałem zależność ``Microsoft.AspNetCore.TestHost``. Finalnie należy dodać następujące biblioteki wymagane do testów ASP.NET Core.

{% highlight xml %}
<PackageReference Include="xunit" Version="2.2.0"/>
<PackageReference Include="xunit.runner.visualstudio" Version="2.2.0"/>
<PackageReference Include="Microsoft.AspNetCore.TestHost" Version="1.1.2"/>
{% endhighlight %}    

Kolejnym krokiem jest uruchomienie serwera testowego, który wykorzystuje klasę ``Startup`` z projektu ``DataBoard.Web``.

{% highlight csharp %}
public class UsersControllerTest
{
    private readonly TestServer _server;
    private readonly HttpClient _client;

    public UsersControllerTest()
    {
        _server = new TestServer(new WebHostBuilder()
            .UseStartup<Startup>());
        _client = _server.CreateClient();
    }
}
{% endhighlight %}    

Teraz pozostało tylko utworzenie testów. Każda testowa metoda musi zostać oznaczona atrybutem ``Fact``.

Pierwszy test sprawdza czy zwracany jest poprawny użytkownik na podstawie adresu e-mail. Porównuję zwrócony e-mail z żądanym, dodatkowo weryfikuję kod odpowiedzi.

{% highlight csharp %}
[Fact]
public async Task given_valid_email_user_should_exist()
{
    var email = "user1@a.pl";

    var response = await _client.GetAsync($"/users/{email}");
    response.EnsureSuccessStatusCode();

    var responseString = await response.Content.ReadAsStringAsync();
    var user = JsonConvert.DeserializeObject<UserDto>(responseString);

    Assert.Equal(email, user.Email);
    Assert.Equal(HttpStatusCode.OK, response.StatusCode);
}
{% endhighlight %}   


Drugi test sprawdza, czy przy próbie żądania użytkownika na podstawie nie istniejącego adresu e-mail zostanie zwrócony status **404**.

{% highlight csharp %}
public async Task given_valid_email_user_should_not_exist()
{
    var email = "not@exist.com";

    var response = await _client.GetAsync($"/users/{email}");

    Assert.Equal(HttpStatusCode.NotFound, response.StatusCode);
}
{% endhighlight %}   


Trzeci test sprawdza wywołanie żądania POST - utworzenie nowego użytkownika. Ten test był najtrudniejszy, ponieważ do metody ``PostAsync`` poza URI musiałem przekazać obiekt typu [HttpContent](https://msdn.microsoft.com/en-us/library/system.net.http.httpcontent(v=vs.118).aspx), który jest klasą abstrakcyjną i powinien zawierać tak naprawdę treść żądania HTTP. W "zwykłym" ASP.NET istnieje metoda [PostAsJsonAsync](https://msdn.microsoft.com/en-us/library/hh944521(v=vs.118).aspx), niestety .NET Core jest ubogi o nią. Jako rozwiązanie problemu utworzyłem pomocniczą klasę ``JsonContent``, która dziedziczy po ``StringContent``, a ta po ``HttpContent``. W konstruktorze przekazuje obiekt do serializacji. Natomiast sama metoda testowa sprawdza status HTTP oraz zwrócony nagłówek żądania HTTP.

{% highlight csharp %}
public class JsonContent : StringContent
{
    public JsonContent(object obj) : base(JsonConvert.SerializeObject(obj), Encoding.UTF8, "application/json")
    {

    }
}
{% endhighlight %}   

{% highlight csharp %}
[Fact]
public async Task given_unique_email_user_should_be_created()
{
    var command = new CreateUser();
    command.Email = "user888@mail.com";
    command.Password = "secret";

    var payload = new JsonContent(command);
    var response = await _client.PostAsync("users/", payload);

    Assert.Equal(HttpStatusCode.Created, response.StatusCode);
    Assert.Equal(response.Headers.Location.ToString(), $"/users/{command.Email}");
}
{% endhighlight %}   

Samo uruchomienie testów jest proste, wystarczy wywołać polecenie ``dotnet test``.

![dotnet test](/assets/2017-05-16-pierwsze-testy/dotnet-test.png "dotnet test")


Na pewno moje testy nie są idealne i wymagają poprawek, ale mają niepodważalną zaletę **po prostu są** ;-) Z rzeczy, które należy zmienić to na pewno wywołanie testów na repozytorium **InMemory**, a nie bezpośrednio na bazie danych, ponieważ ostatni test ``given_unique_email_user_should_be_created`` wykona się tylko raz dla żądanego adresu e-mail. Muszę doczytać w jaki sposób dla projektu testowego zmienić repozytorium i zrefaktorować kod, ale to już treść na następny odcinek.