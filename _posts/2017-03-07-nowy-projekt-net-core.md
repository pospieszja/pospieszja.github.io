---
layout: post
title: Nowy projekt w .NET Core
author: Jacek Pospieszyński
---

.NET Core umożliwia uruchomienie tego samego kawałka kodu na wielu platformach (Windows, Linux, OS X). Pomyślałem zatem, że warto byłoby z tego skorzystać przy projekcie [DataBoard](https://github.com/pospieszja/DataBoard) i nabyć nowego doświadczenia przy pracy z Linuksem. Jako system operacyjny wybrałem [Elementary OS](https://elementary.io/), a jako edytor kodu [Visual Studio Code](https://code.visualstudio.com/). Visual Studio Code jest tylko edytorem tekstu, który oczywiście można wzbogacić wieloma [rozszerzeniami](https://marketplace.visualstudio.com/vscode) (pojawi się w przyszłości na ten temat oddzielny wpis), a nie zintegrowanym środowiskiem deweloperskim jak jego starszy brat Visual Studio, i tym samym nie umożliwia wygenerowania nowego projektu .NET Core (aplikacji konsolowej czy też webowej). A jak utworzyć nowy projekt .NET Core pod Linuksem dowiecie się dalej z tego wpisu.

>.NET Core w chwili obecnej ulega częstym zmianom, dlatego chciałbym zaznaczyć, że poniższy wpis opiera się o wersję 1.0.0-rc4-004771.

Narzędzie .NET Core CLI umożliwia utworzenie projektu na podstawie kilku domyślnych szablonów.

![alt text](/img/dotnet-new.png "dotnet new")

To co mnie najbardziej interesowało to część związana aplikacjami internetowymi. Na pierwszy rzut wybrałem szablon aplikacji MVC.

{% highlight bash %}
dotnet new mvc
{% endhighlight %}