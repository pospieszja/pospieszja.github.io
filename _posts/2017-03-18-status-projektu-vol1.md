---
layout: post
title: Status projektu DataBoard [vol. 1]
author: Jacek Pospieszyński
date: '2017-03-18'
categories: [DSP2017, DataBoard, .NETCore]
---

Niestety prace nad projektem nie idą tak jakbym sobie tego życzył. Zamiast kodować, spędziłem bardzo, bardzoo, bardzooo dużo czasu czytając znalezione w Internecie blogi i artykuły związane z wzorcami projektowymi i architektonicznymi. Tu coś o podejściu DDD, gdzie indziej o architekturze warstwowej, a jeszcze gdzieś dalej o wzorcu repozytorium. Ze względu na brak doświadczenia komercyjnego w mojej głowie powstał niemały mętlik, który nie przyczynił sie do postępu prac. Z związku z tym status projektu będzie krótki.

![alt text](https://media.giphy.com/media/KI9oNS4JBemyI/giphy.gif "mind blown")

<!--more-->

### Moja własna architektura

W katalogu głównym projektu utworzyłem folder *Models*. A w nim pojawiły się następujące klasy:
* ***Database*** - klasa reprezentująca bazę danych, której "zdrowie" będzie badane przez aplikację

{% highlight csharp %}
public class Database
{
    public int DatabaseID { get; set; }
    public string Name { get; set; }
    public string IPAdress { get; set; }
    public bool IsActive { get; set; }
    public User Owner { get; set; }
    public virtual Engine Engine { get; set; }
}
{% endhighlight %}

* ***Engine*** - klasa reprezentująca silnik bazy danych, myślę, że w początkowej fazie projektu pozostanę tylko przy **MS SQL Server** (MVP przede wszystkim)

{% highlight csharp %}
public class Engine
{
    public int EngineID { get; set; }
    public string Name { get; set; }
}
{% endhighlight %}

* ***User*** - klasa reprezentująca użytkownika, muszę coś wymyślić w kwestii przechowywania hasła, bo *string* nie jest dobrym pomysłem ;-)

{% highlight csharp %}
public class User
{
    public int UserID { get; set; }
    public string Password { get; set; }
    public bool IsActive { get; set; }
    public List<Database> Databases { get; set; }
}
{% endhighlight %}

* ***IDatabaseRepository*** - klasa reprezentująca operację CRUD na obiekcie *Database*

{% highlight csharp %}
public interface IDatabaseRepository
{
    IEnumerable<Database> Databases { get; }
    
    User GetDatabaseById(int databaseId);
    void AddDatabase(User database);
    void EditDatabase(User database);
    void DeleteDatabase(int databaseId);
}
{% endhighlight %}

* ***IUserRepository*** - klasa reprezentująca operację CRUD na obiekcie *User*

{% highlight csharp %}
public interface IUserRepository
{
    IEnumerable<User> Users { get; }

    User GetUserById(int userId);

    void AddUser(User user);
    void EditUser(User user);
    void DeleteUser(int userId);
}
{% endhighlight %}

* ***MockUserRepository***
* ***MockDatabaseRepository***

>Nie zaprezentowałem ciała klas "mockujących", żeby nie zakłócić czytelności. Implementują one interfejsy odpowiednio *IUserRepository* oraz *IMockRepository*.

W założeniu aplikacja powinna dodatkowo zbierać historyczne dane o stanie bazy danych, ale w tym momencie funkcjonalność ta nie znajduje się w obrębie MVP ;-)

Po spędzeniu wielu godzin z "architekturą" niestety nie wymyśliłem nic ciekawszego. Myślę, że wraz z postępem w projekcie nadejdzie potrzeba refaktoryzacji.
Jeżeli macie pomysły na poprawę powyższego stanu rzeczy czekam na komentarze :-)