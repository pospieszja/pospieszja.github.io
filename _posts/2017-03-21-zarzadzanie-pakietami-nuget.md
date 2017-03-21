---
layout: post
title: Zarządzanie pakietami Nuget w .NET Core
author: Jacek Pospieszyński
date: '2017-03-21'
categories: [DSP2017, DataBoard, .NETCore]
---
Visual Studio 2015 przyzwyczaił mnie do prostego przeglądania, wyszukiwania i instalacji pakietów Nuget. Wbudowany menadżer pakietów był dla mnie wystarczający i nie potrzebowałem żadnych alternatyw. Pracując przy projekcie DataBoard na ElementaryOS i z Visual Studio Code musiałem znaleźć rozwiązanie, które zastąpi mi ręczną edycję pliku `*.csproj`.

![alt text](/assets/2017-03-21-zarzadzanie-pakietami-nuget/nuget-logo.gif "nuget.org")

<!--more-->

### Na pomoc .NET Core Tools
I na pomoc przychodzi .NET Core Tools, który umożliwia instalację paczek Nuget za pomocą polecenia `dotnet add package`. Po uruchomieniu polecenia następuje pobranie wybranej paczki, sprawdzenie czy jest ona kompatybilna z projektem, jeśli tak to do pliku `*.csproj` zostanie dopisany nowy element `<PackageReference>`.

![alt text](/assets/2017-03-21-zarzadzanie-pakietami-nuget/dotnet add package.gif "dotnet add package")

Oczywiście jeśli istnieje możliwość dodania nowej paczki to musi istnieć możliwość jej usunięcia. Można to zrealizować poprzez polecenie `dotnet remove package`. Wybrana paczka zostanie usuniętą z projektu oraz zostanie zaktualizowany plik `*.csproj`.

![alt text](/assets/2017-03-21-zarzadzanie-pakietami-nuget/dotnet remove package.gif "dotnet remove package")

Polecenie `dotnet add package` umożliwia również wybranie wersji pakietu (-v) czy też instalacje pakietu bez sprawdzenia kompatybilności z projektem (-n). Po więcej odsyłam bezpośrednio na strony [Microsoft](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/dotnet-add-package). 

### Przeszukiwanie repozytorium Nugeta
Niestety, ale aktualnie .NET Core Tools nie umożliwia przeszukiwania zasobów Nugeta. Żeby zainstalować paczkę trzeba znać wcześniej jej nazwę lub wyszukać ją w [repozytorium Nugeta](http://www.nuget.org/). Może to być uciążliwie, szczególnie jeśli jesteśmy nowi w świecie .NET.

Przeglądając istniejące [rozszerzenia](https://marketplace.visualstudio.com/vscode) dla Visual Studio Code, natknąłem się na [NuGet Package Manager](https://marketplace.visualstudio.com/items?itemName=jmrog.vscode-nuget-package-manager), które rozwiązuje ten problem.
Jest kompatybilne z wersją .NET Core 1.1. Pozwala na dodawanie/usuwanie oraz wyszukiwanie (również po częściowej nazwie) paczek za pomocą Command Palette (CTRL+SHIFT+P).

![alt text](/assets/2017-03-21-zarzadzanie-pakietami-nuget/vscode add package.gif "vscode add package")

![alt text](/assets/2017-03-21-zarzadzanie-pakietami-nuget/vsc remove package.gif "vscode remove package")


Ja osobiście preferuje rozszerzenie Visual Studio Code, a Wy jak sobie radzicie?