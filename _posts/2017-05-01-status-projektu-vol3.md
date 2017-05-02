---
layout: post
title: Status projektu DataBoard [vol. 3]
author: Jacek Pospieszyński
date: '2017-05-01'
categories: [DSP2017, DataBoard, .NETCore]
---

W końcu nastąpił większy postęp w mojej aplikacji! Bardzo się z tego cieszę, szczególnie, że początki nie były łatwe i wywołały u mnie frustrację i tym samym niechęć do dalszej pracy nad projektem. W końcu również widać efekty w postaci wywołania API i zwrócenia wyników w JSON. Dziś opiszę co udało mi się zrealizować.

![Zootopia](https://media.giphy.com/media/3NtY188QaxDdC/giphy.gif "happy")

<!--more-->

### Wzorce w praktyce
Na wstępnie chciałbym zaznaczyć, że nie udałoby mi się tego wszystkiego zrealizować gdyby nie fantastyczny kurs [Piotrka Gankiewicza](http://piotrgankiewicz.com/) - [Becoming a software developer](http://piotrgankiewicz.com/courses/becoming-a-software-developer/). Udało mi się zastosować w praktyce wzorzec [Onion Architecture](http://jeffreypalermo.com/blog/the-onion-architecture-part-1/), a przynajmniej na tyle ile go zrozumiałem. Oczywiście wszystko to małe kroki, ale są one dla mnie bardzo ważne. Opiszę wszystko na przykładzie klasy ``User``, która jest najbardziej dopracowana.

Klasa domenowa ``User`` wygląda następująco:

{% highlight csharp %}
public class User
{
    public Guid Id { get; private set; }
    public string Email { get; private set; }
    public string Password { get; private set; }
    public bool IsActive { get; private set; }
    public DateTime CreatedAt { get; private set; }
    public DateTime UpdatedAt { get; private set; }

    public User()
    {

    }

    public User(string email, string password)
    {
        Id = Guid.NewGuid();
        SetEmail(email);
        SetPassword(password);
        IsActive = true;
        CreatedAt = DateTime.UtcNow;
        UpdatedAt = DateTime.UtcNow;
    }

    public void SetEmail(string email)
    {
        if (String.IsNullOrWhiteSpace(email))
        {
            throw new Exception("Email can't be empty.");
        }

        if (Email == email)
        {
            return;
        }
        Email = email.ToLowerInvariant();
        UpdatedAt = DateTime.UtcNow;
    }

    public void SetPassword(string password)
    {
        if (String.IsNullOrWhiteSpace(password))
        {
            throw new Exception("Password can't be empty.");
        }

        if (password.Length < 6)
        {
            throw new Exception("Password can't be shorter then 6 characters.");
        }

        if (password.Length > 20)
        {
            throw new Exception("Password can't be longer then 20 characters.");
        }

        if (Password == password)
        {
            return;
        }
        Password = password;
        UpdatedAt = DateTime.UtcNow;
    }
}
{% endhighlight %}

Jak widać walidacja danych następuje przed utworzeniem instancji klasy, zwracane są odpowiednie wyjątki. Kwestia uwierzytelniania i przechowywania hasła nie jest jeszcze rozwiązana, ale to na ten moment nie jest kluczowe.

Następnie utworzyłem repozytorium z interfejsem ``IUserRepository``, po to żeby było wiadomo jakie operacje będzie można wykonywać na obiektach domenowych, jednocześnie nie ma tutaj uzależnienia od implementacji sposobu realizowania tych zadań.

{% highlight csharp %}
public interface IUserRepository
{
        User Get(Guid userId);
        User Get(string email);
        IEnumerable<User> GetAll();
        void Add(User user);
        void Update(User user);
        void Remove(Guid userId);
}
{% endhighlight %}

Kolejnym krokiem już w projekcie ``DataBoard.Infrastructure`` utworzyłem klasę ``UserDTO`` bez zbędnych metod, która będzie transferować do projektu ``DataBoard.Web`` tylko część informacji. Zostanie ona oczywiście rozszerzona, tylko wstępnie tak wygląda.

{% highlight csharp %}
public class UserDto
{
    public Guid Id { get; set; }
    public string Email { get; set; }
}
{% endhighlight %}

W ``DataBoard.Infrastructure`` znajduje się właściwa implementacja repozytorium ``UserRepository``. Wstępnie wygląda następująco. Jest ona powiązana z bazą danych oraz Entity Framework Core. Za pomocą wbudowanych mechanizmów w ASP.NET Core [DependencyInjection](https://pl.wikipedia.org/wiki/Wstrzykiwanie_zale%C5%BCno%C5%9Bci) wstrzykuje zależność "kontekstu" bazy danych.

{% highlight csharp %}
public class UserRepository : IUserRepository
{
    private readonly DataboardContext _context;

    public UserRepository(DataboardContext context)
    {
        _context = context;
    }
    public void Add(User user)
    {
        _context.Users.Add(user);
        _context.SaveChanges();
    }

    public User Get(Guid userId)
    {
        throw new NotImplementedException();
    }

    public User Get(string email)
    {
        return _context.Users.FirstOrDefault();
    }

    public IEnumerable<User> GetAll()
    {
        return _context.Users;
    }

    public void Remove(Guid userId)
    {
        throw new NotImplementedException();
    }

    public void Update(User user)
    {
        throw new NotImplementedException();
    }
}
{% endhighlight %}

Sam kontekst wygląda następująco

{% highlight csharp %}
public class DataboardContext : DbContext
{        
    public DataboardContext(DbContextOptions<DataboardContext> options) : base(options)
    {
        Database.Migrate();
    }

    public DbSet<User> Users { get; set; }
}
{% endhighlight %}

Również w ``DataBoard.Infrastracture`` znajdują się usługi, które udostępniane są dla ``DataBoard.Web``, tak aby ten ostatni nie miał świadomości o tym skąd i w jaki sposób są pobierane dane. Jako zależności wstrzykuje tutaj interfejs ``IUserRepository`` oraz ``IMapper``. Ten drugi pozwala na transofrmację obiektu domenowego w obiekt DTO.

{% highlight csharp %}
public class UserService: IUserService
{
    private readonly IUserRepository _userRepository;
    private readonly IMapper _mapper;
    
    public UserService(IUserRepository userRepository, IMapper mapper)
    {
        _userRepository = userRepository;
        _mapper = mapper;
    }

    public UserDto Get(string email)
    {
        var user = _userRepository.Get(email);
        return _mapper.Map<User,UserDto>(user);
    }

    public IEnumerable<UserDto> GetAll()
    {
        var users = _userRepository.GetAll();
        return _mapper.Map<IEnumerable<User>,IEnumerable<UserDto>>(users);
    }
}
{% endhighlight %}

Na samym końcu "układu pokarmowego" (a może jednak na początku) znajduje się ``DataBoard.Web`` i kontroler ``UserController``. Tu również następuje wstrzyknięcie zależności, w tym wypadku wcześniej wspomnianej usługi ``IUserService``.

{% highlight csharp %}
[Route("/api/users")]
public class UserController : Controller
{
    private readonly IUserService _userService;

    public UserController(IUserService userService)
    {
        _userService = userService;
    }

    public JsonResult GetUsers()
    {
        return Json(_userService.GetAll());
    }
}
{% endhighlight %}

Po wywołaniu adresu http://localhost:5000/api/users otrzymuję listę użytkowników zapisanych w bazie danych w formacie JSON.

![json result /api/users"](/assets/2017-05-01-status-projektu-vol3/json_api_users.png "json result /api/users")

### Problemy
Najwięcej problemów przyniosło mi zastosowanie [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/). Co prawda [opisywałem na swoim blogu](https://blog.pospieszynski.net/2017/04/23/ef-core) jak można go zastosować w aplikacji, ale była ta prosta aplikacj bez podziałów na warstwy.

Po pierwsze w ``Databoard.Web`` ``Startup.cs`` musiałem wstrzyknąć kontekst bazy danych ``services.AddDbContext<DataboardContext>(options => options.UseSqlServer("Data Source=localhost;Database=Databoard;User ID=sa;Password=4BEsFpaq"));`` a w nim **ConnectionStringa**.

Kolejny problem stanowiło to, że kontekst bazy danych znajdował się w ``DataBoard.Infrastructure``, wywołanie dla tego projektu ``dotnet ef migrations add InitialCreate`` kończyło się błędem:

{% highlight bash %}
Your target project '<Target Project>' doesn't match your migrations assembly '<Migrations Assembly>'. Either change your target project or change your migrations assembly.
Change your migrations assembly by using DbContextOptionsBuilder. E.g. options.UseSqlServer(connection, b => b.MigrationsAssembly("<Target Project>'")). By default, the migrations assembly is the assembly containing the DbContext.
Change your target project to the migrations project by using the Package Manager Console's Default project drop-down list, or by executing "dotnet ef" from the directory containing the migrations project.
{% endhighlight %}

Niestety nie zrozumiałem co należy zrobić, aż natknąłem się na [to rozwiązanie na GitHub](https://github.com/aspnet/EntityFramework/issues/5900#issuecomment-229483068).

Uruchomiłem polecenie ``dotnet ef --startup-project ../DataBoard.Web/ migrations add InitialCreate`` a następnie ``dotnet ef --startup-project ../DataBoard.Web/ database update``, najpierw zostały utworzone pierwsze pliki migracyjne, a następnie została zmodyfikowana baza danych.

Dodatkowo, aby wypełnić bazę danych "dummy data" wywołuję statyczną metodę, która zostaje wywołana wraz ze startem aplikacji.

{% highlight csharp %}
public static void EnsureSeedData(this DataboardContext context)
{
    if (context.Users.Any())
    {
        return;
    }

    var users = new List<User>()
    {
        new User("user1@a.pl", "secret"),
        new User("user2@a.pl", "secret")                
    };

    context.Users.AddRange(users);
    context.SaveChanges();
}
{% endhighlight %}

### Co dalej
Backend zaczął działać, teraz muszę się skupić na frontendzie. Zbuduję podstawową główną stronę opierając się na [Bootstrapie](http://getbootstrap.com/) dołączę do tego komponenty napisane w [Vue.js](https://vuejs.org/), które będą konsumować API. Nie wiem czy jest to rozwiązanie optymalne, czy w ogóle prawidłowe, ale na pewno muszę napierać do przodu :)