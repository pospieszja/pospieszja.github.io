---
layout: post
title: ASP.NET Core - konfiguracja
author: Jacek Pospieszyński
date: '2017-05-23'
categories: [DSP2017,.NETCore]
---
Do tej pory przekazywałem bezpośrednio w kodzie ręcznie **ConnectionString**, co jest rozwiązaniem mało efektownym i efektywnym, najdelikatniej mówiąc. Do tego, jeżeli chciałbym w przyszłości używać dwóch różnych baz danych w zależności od środowiska: produkcyjne lub deweloperskie, musiałbym poszukać każdego wystąpienia **ConnectionString** i go odpowiednio zmodyfikować. Microsoft w **ASP.NET Core** ułatwił to zadanie udostępniając w **Microsoft.Extensions.Configuration** API konfiguracyjne.

<!--more-->

### Z czym to się je?
W skrócie API konfiguracyjne pozwala na odczytanie i konfigurację aplikacji za pomocą pary klucza i wartości. Do tego każda taka para może zostać pogrupowana w wielo-poziomową hierarchię.

Poniżej znajduje się domyślny plik ``appsettings.json``, w którym przechowywana jest konfiguracja utworzona wraz z nowym projektem.

{% highlight json %}
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
{% endhighlight %} 

Microsoft nie ogranicza się do plików JSON. API pozwala na odczytanie i deserializacje danych m.in. z:
* plików (INI, JSON, XML),
* linii poleceń,
* zmiennych środowiskowych,
* obiektów In-Memory,
* Azure Key Vault.

### W praktyce
Aby móc odczytać konfigurację należy utworzyć instancję ``ConfigurationBuilder``. Argumentem metody ``SetBasePath`` jest ścieżka w której znajdują się pliki konfiguracyjne, domyślnie główny katalog z projektem. Następnie do konfiguracji dodawane są dwa pliki JSON.
W pierwszym pliku ``appsettings.json`` ``optional: false`` wskazuje, że plik jest wymagany do uruchomienia aplikacji. Próba uruchomienia aplikacji bez tego pliku konfiguracyjnego zakończy się wyjątkiem. 
Drugi plik już nie jest wymagany. API konfiguracyjne będzie poszukiwać ``appsettings.<EnvironmentName>.json``, gdzie ``<EnvironmentName>`` jest wartością zmiennej środowiskowej ``ASPNETCORE_ENVIRONMENT``, więcej o tym w następnym akapicie.

{% highlight csharp %}
var builder = new ConfigurationBuilder()
    .SetBasePath(env.ContentRootPath)
    .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
    .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
    .AddEnvironmentVariables();
Configuration = builder.Build();
{% endhighlight %} 

W swoim projekcie do pliku konfiguracyjnego dodałem **ConnectionString**.

{% highlight json %}
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=localhost;Database=Databoard;User ID=sa;Password=4BEsFpaq"
  }
}
{% endhighlight %} 

Aby odczytać jego wartość i przekazać ją dalej do aplikacji mogę to zrobić na kilka sposobów:
* ``var connectionString = Configuration["ConnectionStrings:DefaultConnection"];``,
* ``var connectionString = Configuration.GetValue<string>("ConnectionStrings:DefaultConnection");``.

Powyżej przedstawiłem najprostsze rozwiązanie. Nieco bardziej skomplikowane rozwiązanie umożliwia silnego typowania, poprzez deserializację konfiguracji na obiekty klasy POCO, którą należy wcześniej utworzyć i musi ona odpowiadać strukturze pliku konfiguracyjnego. Po więcej odsyłam do [dokumentacji](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration).

### Zmienne środowiskowe
Jak już wcześniej wspomniałem można mieć pliki konfiguracyjne odpowiednie dla środowiska, w którym została uruchomiona aplikacja. Informacje o aktualnym środowisku uruchomieniowym powinna przechowywać zmienna ``ASPNETCORE_ENVIRONMENT``, którą definiuje się różnie w zależności od systemu operacyjnego:
* MS Windows - ``set ASPNETCORE_ENVIRONMENT=Development``,
* Linux - ``export ASPNETCORE_ENVIRONMENT=Development``.

>Uwaga MS Windows nie rozróżnia przy interpretacji ASPNETCORE_ENVIRONMENT wielkości znaków, Linux już natomiast tak. Dobrą praktyką jest przypisywanie wartości Development, Staging, Production.

Dzięki temu zabiegowi aplikacja sama będzie rozróżniać środowisko uruchomieniowe i stosować odpowiednią konfigurację.