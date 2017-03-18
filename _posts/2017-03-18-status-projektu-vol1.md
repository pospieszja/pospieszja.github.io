---
layout: post
title: Status projektu DataBoard [vol. 1]
author: Jacek Pospieszyński
date: '2017-03-18'
categories: [DSP2017, DataBoard, .NETCore]
---

Niestety prace nad projektem nie idą tak jakbym sobie tego życzył. Zamiast kodować, bardzo, bardzoo, bardzooo dużo czasu spędziłem czytając różne blogi i artykuły związane z wzorcami projektowymi i architektonicznymi. Tu coś o podejściu DDD, gdzie indziej o architekturze warstwowej, a jeszcze gdzieś dalej o wzorcu repozytorium. Ze względu na brak doświadczenia komercyjnego w mojej głowie powstał niemały mętlik. Z związku z tym status projektu będzie krótki.

<!--more-->

### Moja własna architektura

W katalogu głównym projektu utworzyłem folder *Models*. A w nim pojawiły się następujące klasy:
* *Database*

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

* *Engine*
{% highlight csharp %}
public class Engine
{
    public int EngineID { get; set; }
    public string Name { get; set; }
}
{% endhighlight %}

* *User*
{% highlight csharp %}
public class User
{
    public int UserID { get; set; }
    public string Password { get; set; }
    public bool IsActive { get; set; }
    public List<Database> Databases { get; set; }
}
{% endhighlight %}
* *IDatabaseRepository*
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
* *IUserRepository*
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
* *MockUserRepository*
{% highlight csharp %}
public class MockUserRepository : IUserRepository
{
    public IEnumerable<User> Users => throw new NotImplementedException();

    public void AddUser(User user)
    {
        throw new NotImplementedException();
    }

    public void DeleteUser(int userId)
    {
        throw new NotImplementedException();
    }

    public void EditUser(User user)
    {
        throw new NotImplementedException();
    }

    public User GetUserById(int userId)
    {
        throw new NotImplementedException();
    }
}
{% endhighlight %}
* *MockDatabaseRepository*
{% highlight csharp %}
public class MockDatabaseRepository : IDatabaseRepository
{
    public IEnumerable<Database> Databases => throw new NotImplementedException();

    public void AddDatabase(User database)
    {
        throw new NotImplementedException();
    }

    public void DeleteDatabase(int databaseId)
    {
        throw new NotImplementedException();
    }

    public void EditDatabase(User database)
    {
        throw new NotImplementedException();
    }

    public User GetDatabaseById(int databaseId)
    {
        throw new NotImplementedException();
    }
}
{% endhighlight %}


