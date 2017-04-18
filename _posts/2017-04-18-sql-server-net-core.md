---
layout: post
title: MS SQL Server + .NET Core
author: Jacek Pospieszyński
date: '2017-04-18'
categories: [DSP2017, MSSQLServer, .NETCore]
---

[Kontynuacja](https://blog.pospieszynski.net/2017/04/13/sql-server-na-ubuntu) wpisu o **MS SQL Server**. Dziś króciutki wpis o tym jak wywołać polecenie SQL na nowo utworzonej bazie danych.

<!--more-->

Aby móc rozpocząć pracę z MS SQL Server należy dodać do swojego projektu bibliotekę umożliwiającą komunikację z bazą danych. Ja wykorzystam standardową bibliotekę Microsoft - [System.Data.SqlClient](https://www.nuget.org/packages/System.Data.SqlClient/). Dodaje ją do projektu poleceniem ``dotnet add package System.Data.SqlClient``, a następnie wywołuje ``dotnet restore``, tym samym bilbioteka jest już gotowa do użycia.

Przede wszystkim należy utworzyć ``ConnectionString``, a pomoże w tym klasa ``SqlConnectionStringBuilder``, która ułatwia zbudowanie wcześniej wspomnianego ``stringa``.
Tworzę nowy obiekt, a następnie przypisuje wartości najważniejszym właściwościom, czyli:
* DataSource - adres serwera
* UserID - nazwa użytkownika
* Password - hasło w/w użytkownika

Utworzony w ten sposób ``ConnectionString`` zostaje przekazany do konstruktura ``SqlConnection``, który otwiera połączenie z bazą danych.

Jako przykładowe polecenie SQL wykonam zwykłego ``selecta``, który zwróci nazwy baz danych. Polecenie SQL można zapisać w zmiennej typu ``string``, a następnie przekazać do konstruktura ``SqlCommand`` wraz z otwartym połączeniem. Następnie odczytuje strumień wierszy zwrócony przez polecenie SQL.


{% highlight csharp %}
try
{
    // Build connection string
    SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
    builder.DataSource = "localhost";  
    builder.UserID = "sa";             
    builder.Password = "4BEsFpaq";     

    // Connect to SQL
    Console.Write("Connecting to SQL Server ... ");
    using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
    {
        connection.Open();
        Console.WriteLine("Done.");
        string sql = "SELECT name FROM sys.Databases;";

        using (SqlCommand command = new SqlCommand(sql, connection))
        {
            using (SqlDataReader reader = command.ExecuteReader())
            {
                while (reader.Read())
                {
                    Console.WriteLine("{0}", reader.GetString(0));
                }
            }
        }
    }
}
catch (SqlException e)
{
    Console.WriteLine(e.ToString());
}
{% endhighlight %}

W następnej części opiszę wykorzystanie **[EntityFramework](https://docs.microsoft.com/en-us/ef/core/index)**.