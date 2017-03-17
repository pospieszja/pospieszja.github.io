---
layout: post
title: Nowy projekt w .NET Core
author: Jacek Pospieszyński
---

.NET Core umożliwia uruchomienie tego samego kawałka kodu na wielu platformach (Windows, Linux, OS X). Pomyślałem zatem, że warto byłoby z tego skorzystać przy projekcie [DataBoard](https://github.com/pospieszja/DataBoard) i nabyć nowego doświadczenia przy pracy z Linuksem. Jako system operacyjny wybrałem [Elementary OS](https://elementary.io/), a jako edytor kodu [Visual Studio Code](https://code.visualstudio.com/). Visual Studio Code jest tylko edytorem tekstu, który oczywiście można wzbogacić wieloma [rozszerzeniami](https://marketplace.visualstudio.com/vscode) (pojawi się w przyszłości na ten temat oddzielny wpis), a nie zintegrowanym środowiskiem deweloperskim jak jego starszy brat Visual Studio, i tym samym nie umożliwia wygenerowania nowego projektu .NET Core (aplikacji konsolowej czy też webowej). A jak utworzyć nowy projekt .NET Core pod Linuksem dowiecie się dalej z tego wpisu.

<!--more-->

>.NET Core w chwili obecnej ulega częstym zmianom, dlatego chciałbym zaznaczyć, że poniższy wpis opiera się o wersję 1.0.0-rc4-004771.

Narzędzie .NET Core CLI umożliwia utworzenie projektu na podstawie kilku domyślnych szablonów.

![alt text](/img/dotnet-new.png "dotnet new")

To co mnie najbardziej interesowało to część związana aplikacjami internetowymi. Na pierwszy rzut wybrałem szablon aplikacji MVC.

{% highlight bash %}
$ dotnet new mvc
{% endhighlight %}

Generowanie projektu trwało 409 ms,  a w Visual Studio Code moim oczom ukazała się lista plików.

![alt text](/img/vscode-dotnet-new-mvc.png "vscode dotnet new mvc")

Jak widać powyżej, powstał szkielet pełnego projektu MVC. Niestety nie wiedziałem dokładnie co, jak i dlaczego zostało wygenerowane, nie wspominając o zawartości plików klas i zależności. Wobec tego wybrałem drugą opcję i utworzyłem pusty projekt aplikacji internetowej.

{% highlight bash %}
$ dotnet new web
{% endhighlight %}

Tym razem już po 86 ms został wygenerowany, najprostszy z możliwych, startowy projekt aplikacji internetowej.

![alt text](/img/vscode-dotnet-new-web.png "vscode dotnet new web")

Dalej wywołałem:

{% highlight bash %}
$ dotnet restore
$ dotnet run
{% endhighlight %}

Pierwsze polecenie pobiera zależności (zapisane w pliku *DataBoard.csproj*), za pomocą menadżera pakietów [Nuget](https://www.nuget.org/), wymagane do uruchomienia aplikacji. Drugie uruchamia aplikację, a w tym konkretnym przypadku również lokalny serwer (*Kestrel*).

![alt text](/img/dotnet-run.png "dotnet run")

Jeżeli wszystko przebiegło pomyślnie po wpisaniu w przeglądarce adresu *http://localhost:5000* powinna się ukazać poniższa strona.

![alt text](/img/hello-world.png "hello world")

Kolekcje domyślnych szablonów można już teraz zwiększyć o szablony SinglePageApplication, zostało to opisane na blogu [.NET WebDev](https://blogs.msdn.microsoft.com/webdev/2017/02/14/building-single-page-applications-on-asp-net-core-with-javascriptservices/). Co więcej każdy może utworzyć swój własny szablon korzystając z [tego](https://github.com/dotnet/templating/wiki/%22Runnable-Project%22-Templates) opisu.

P.S.

Alternatywnie można skorzystać z narzędzia [Yeoman](https://docs.microsoft.com/en-us/aspnet/core/client-side/yeoman), które również pozwala na utworzenie projektu .NET Core na podstawie szablonów. Natomiast ja osobiście się zraziłem do niego, ponieważ w którymś momencie .NET Core przestał korzystać z project.json na rzecz *.csproj (pliki zawierające wszystkie metadane dotyczące projektu oraz zależności), a Yeoman nie zdążył dostosować szablonów do najnowszych zmian w .NET Core, uruchomienie świeżo utworzonego projektu kończyło się niepowodzeniem z powodu nieznalezienia pliku *.csproj.