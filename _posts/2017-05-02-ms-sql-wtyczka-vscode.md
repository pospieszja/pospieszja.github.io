---
layout: post
title: Wtyczka MS SQL dla Visual Studio Code
author: Jacek Pospieszyński
date: '2017-05-02'
categories: [DSP2017, VSCode]
---

Microsoft udostępnił SQL Server na Linuksa, niestety zabrakło narzędzia do jego zarządzania typu [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms), które jest dostępne na Windowsa. Istnieje CLI do zarządzania bazą danych, jednak nie jest ono najwygodniejsze. Z pomocą przychodzi [rozszerzenie do Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql), które ułatwia pracę z SQL.

![Visual Studio Code"](/assets/2017-05-02-ms-sql-wtyczka-vscode/vscode-logo.png "Visual Studio Code")

<!--more-->

> Cały wpis opieram na wersji 0.3.0

### Instalacja
Rozszerzenie jest produktem Microsoftu, a jego kod dostępny na [GitHub](https://github.com/Microsoft/vscode-mssql). Pozwala na połączenie się z **MS SQL Server** oraz **Azure SQL**. Ponadto umożliwia:
* tworzenie profili połączeń,
* snippety i intelisense dla T-SQL,
* kolorowanie składni,
* wywołanie skryptów i zwracanie wyników w postaci tabelarycznej,
* możliwość zapisu wyniku do formatu JSON lub CSV.

Instalacja rozszerzenia jest banalna, wystarczy wybrać z bocznego paska menu opcje związane z zarządzaniem rozszerzeniami, następnie wyszukać ``mssql`` i dalej po prostu zainstalować.
![install extension mssql"](/assets/2017-05-02-ms-sql-wtyczka-vscode/extension-install.png "install extension mssql")

### Konfiguracja połączenia
Po instalacji dodatku po wpisaniu ``sql`` w ``Command Palette`` pojawią się nowe polecenia.

![sql commands](/assets/2017-05-02-ms-sql-wtyczka-vscode/sql-commands.png "sql commands")

Aby rozpocząć pracę z rozszerzeniem należy utworzyć pierwszy profil połączenia z bazą danych.

![create connection profile](/assets/2017-05-02-ms-sql-wtyczka-vscode/create-profile.gif "create connection profile")

Po utworzeniu połączenia można utworzyć nowy dokument i zapisać go z rozszerzeniem ``.SQL``, następnie można wykonać połączenie z wybranym profilem.

![sql connect](/assets/2017-05-02-ms-sql-wtyczka-vscode/sql-connect.gif "sql connect")


### W praktyce
Teraz gdy konfiguracja rozszerzenia została zakończona można napisać pierwsze polecenia SQL i je wywołać. Polecenie można napisać "z palca" lub z pomocą kilkunastu dostępnych snippetów. Prawdopodobnie istnieje możliwość rozszerzenia istniejącej kolekcji, jednakże nie sprawdziłem tego.

![sql snippets](/assets/2017-05-02-ms-sql-wtyczka-vscode/sql-snippets.png "sql snippets")

W celu zaprezentowania w praktyce rozszerzenia **mssql** napisałem proste polecenie T-SQL typu **DML (Data Manipulation Language)** zwracającego zawartość tabeli ``Users``. Warto pamiętać, że rozszerzenie również obsługuje polecenia tpyu **DDL (Data Definition Language)**.

![sql result](/assets/2017-05-02-ms-sql-wtyczka-vscode/sql-result.png "sql result")

Polecenie uruchamia się za pomocą skrótu **CTRL+SHIFT+E**. W wyniku otrzymałem tabelę z danymi, które jak wcześniej wspominałem można zapisać do formatu JSON lub CSV. Ponadto można zauważyć dodatkowe informacje o liczbie zwróconych krotek wraz z czasem wykonania polecenia.

