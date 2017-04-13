---
layout: post
title: MS SQL Server na Linuxie
author: Jacek Pospieszyński
date: '2017-04-10'
categories: [DSP2017, MSSQLServer, .NETCore]
---

W marcu 2016 roku Microsoft [ogłosił](https://blogs.microsoft.com/blog/2016/03/07/announcing-sql-server-on-linux/), że wyda SQL Server 2016 również na Linuxa i zakończy się tym samym hegemonia [Oracle](https://www.oracle.com/). Cały świat .NET stanął na równe nogi, nikt się tego nie spodziewał, a na pewno ja :) Minął rok od tego wydarzenia, a ja nie miałem okazji samodzielnie spradzić jak wygląda w praktyce SQL Sever na Linuksie. Dziś krotki wpis o tym jak zainstalować bazę danych oraz jak wywołać pierwsze polecenia.

![SQL Server on Linux](/assets/2017-04-13-sql-server-on-linux/sql-linux.png "sql server on linux")

<!--more-->

### Instalacja
>Opiszę instalację na Ubuntu 16.04, dlatego, że ElementaryOS z którego korzystam wykorzstuje właśnie tą wersję Ubuntu jako podstawę.

W pierwszej kolejności należy dodać klucz GPG oraz nowe źródło z repozytorium do MS SQL Server.
{% highlight bash %}
$ curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
$ curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server.list > /etc/apt/sources.list.d/mssql-server.list
{% endhighlight %}

Następnie przy użyciu narzędzia ``apt-get`` należy zaktualizować repozytorium oraz zainstalować MS SQL Server.
{% highlight bash %}
$ sudo apt-get update
$ sudo apt-get install mssql-server
{% endhighlight %}

Ostatnim krokim jest nadanie hasła użytkownikowi `sa`. Hasło musi się składać z co najmniej 8 znaków i być skomplikowane ;-) Po potwierdzeniu usługa MS SQL Server startuje automatycznie.
{% highlight bash %}
$ /opt/mssql/bin/mssql-conf setup
{% endhighlight %}

![SQL Server configuration](/assets/2017-04-13-sql-server-on-linux/sql-server-conf.png "sql server configuration")

Warto również zainstalować narzędzia MS SQL Server, które pozwolą na wywoływanie poleceń SQL z linii komend.
Klucz już dodaliśmy, wystarczy dodać nowe repozytorium, zaktualizować je, a następnie zainstalować toolsy.

{% highlight bash %}
$ curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
$ sudo apt-get update
$ apt-get install mssql-tools 
{% endhighlight %}

Opcjonalnie warto dodać nową zmienną środowiskową **PATH**, aby nie podawać pełnej ścieżki do plików uruchomieniowych (ja wykorzystuje **[ZSH](http://ohmyz.sh/)**).
{% highlight bash %}
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.zshrc
source ~/.zshrc
{% endhighlight %}


### Pierwsze polecenia
Jeżeli wszystkie kroki powyżej zakończyły się sukcesem można wykonać pierwsze polecenia na nowo zainstalowanej instancji SQL Server.
Używamy w tym celu narzędzia ``sqlcmd`` podając w parametrze **-S** adres serwera, **-U** nazwę użytkownika oraz **-P** hasło (zdefiniowane w poprzednim kroku) w apostrofie.

{% highlight bash %}
sqlcmd -S localhost -U SA -P '4BEsFpaq'
{% endhighlight %}

Po prawidłowym połączeniu możemy wykonywać polecenia SQL.

![sqlcmd select](/assets/2017-04-13-sql-server-on-linux/sqlcmd-select.png "sqlcmd select")

Instalacja i wstępna konfiguracja nie były trudne. W kilka minut można mieć przygotowane środowisko do pracy z SQL Server. W następnym odcinku pokażę w jaki sposób połączyć się przez aplikację napisaną w .NETCore z MS SQL Server.