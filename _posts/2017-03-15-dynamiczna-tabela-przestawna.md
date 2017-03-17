---
layout: post
title: Dynamiczny zakres danych w tabeli przestawnej
author: Jacek Pospieszyński
categories: [DSP2017, Excel]
---
>A teraz coś z zupełnie innej beczki.

![alt text](/assets/2017-03-15-dynamiczna-tabela-przestawna/monty-python.jpg "monty python")

W pracy spędzam wiele czasu wykorzystując **MS Excel**. Dla wielu osób tabele przestawne, wykresy i formuły w MS Excel to po prostu zwykłe mambo-dżambo. Chciałbym im wszystkim pomóc w korzystaniu z arkuszy kalkulacyjnych, a dla siebie pozostawić archiwum rozwiązanych problemów, i tym samym rozpoczynam serię wpisów o MS Excel (deal with it).

<!--more-->

### Tabela przestawna ze stałym zakresem danych

Wybieramy z wstążki *Wstawianie->Tabela przestawna*, określamy zakres danych i po chwili do naszego arkusza zostanie wstawiona tabela przestawna ze stałym zakresem danych.

![alt text](/assets/2017-03-15-dynamiczna-tabela-przestawna/pivot_table_fixed_range.gif "tabela przestawna ze stałym zakresem")

Bardzo często takie rozwiązanie jest wystarczające. Jednak jak to w życiu bywa nie zawsze. Co jeśli będziemy chcieli wstawić nowe wiersze? Wtedy trzeba będzie przejść do edycji zakresu danych istniejącej tabeli przestawnej i zmienić zakres na nowy, rozszerzając go o nowe pozycje. Dla jednej tabeli przestawnej nie stanowi to problemu, ale gdy z tych samych danych korzysta kilka tabel przestawnych, wtedy trzeba proces powtórzyć. Czasem można o tym zapomnieć i w wyniku uzyskać nieprawidłowy raport, który pójdzie dalej w świat – a tego nigdy nie chcemy. No chyba, że pracujecie w „kreatywnym” dziale controllingu 😉

### Tabela przestawna z dynamicznym zakresem danych

Rozwiązaniem jest użycie mechanizmu umożliwiającego nazywanie pojedynczych komórek, zakresów lub formuł. W tym konkretnym przypadku nazwiemy formułę. Będzie się ona składać z następujących funkcji:
  * [ILE.NIEPUSTYCH][1]
  * [PRZESUNIĘCIE][2]

{% highlight bash %}
=PRZESUNIĘCIE(Arkusz1!$A$1;0;0;ILE.NIEPUSTYCH(Arkusz1!$A:$A);3)
{% endhighlight %}

Utworzona formuła zwraca nowy zakres danych. Zaczyna się od komórki A1, będzie wysoki na liczbę niepustych komórek w kolumnie A, a szeroki na trzy kolumny. Trzeba pamiętać, że kolumna A musi mieć ciągłość danych.

![alt text](/assets/2017-03-15-dynamiczna-tabela-przestawna/new_named_formula.gif "menadżer nazw")

Teraz możemy edytować wstawioną na początku tabelą przestawną i zmienić jest zakres wstawiając nazwę *RNG_FOR_PIVOT_TABLE*.

![alt text](/assets/2017-03-15-dynamiczna-tabela-przestawna/pivot_table_dynamic_range.gif "tabela przestawna z dynamicznym zakresem")

I tym oto sposobem możemy się cieszyć tabelą przestawną z dynamicznym zakresem danych. Wszelkie uwagi, pomysły mile widziane w komentarzach 🙂

[1]:https://support.office.com/pl-pl/article/ILE-NIEPUSTYCH-funkcja-7dc98875-d5c1-46f1-9a82-53f3219e2509
[2]:https://support.office.com/pl-pl/article/PRZESUNI%C4%98CIE-funkcja-c8de19ae-dd79-4b9b-a14e-b4d906d11b66