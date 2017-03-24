---
layout: post
title: Dynamiczna seria danych dla wykresu w MS Excel
author: Jacek Pospieszyński
date: '2017-03-24'
categories: [DSP2017, Excel]
---
Kontynuuje serię z poradnikami dla MS Excel. Ostatnio było o dynamicznych zakresach dla tabel przestawnych, a dziś opiszę w jaki sposób utworzyć wykres, którego seria danych będzie się zmieniać dynamicznie, bez ręcznej edycji.

![logo ms excel](/assets/2017-03-24-dynamiczny-wykres/excel-logo.png "MS Excel")

<!--more-->

### Statyczny wykres
Chcemy zestawić na wykresie kolumny 'Wykonanie' oraz 'Wysyłka' za marzec 2017. Nie ma żadnego problemu. Zaznaczymy nagłówki wraz z danymi, a następnie ze wstążki *Wstawianie* wybieramy wykres typu kolumnowego. W wyniku otrzymujemy wykres jak poniżej.

![statyczny wykres](/assets/2017-03-24-dynamiczny-wykres/fixed-chart.png "statyczny wykres")

Czasami zachodzi potrzeba wyświetlenia na wykresie tylko fragmentu danych. Jeżeli wykres składa się tylko z kilku serii danych, nie jest to problem, można ręcznie wyedytować każdą serię i zmienić jej zakresy (należy również pamiętać o zmianie zakresu danych dla etykiet osi). Jednak gdy wykres składa się z wielu serii danych staje się to już kłopotliwe, szczególnie jeżeli takich zmian dokonujemy często.

### Dynamiczny wykres
Załóżmy, że chcemy ograniczyć wykres dwoma datami (komórki **G2** i **G3**). Aby utworzyć taki dynamiczny wykres należy zaprzęgnąć do pracy **Menadżer nazw**. Dla każdej serii danych oraz dla etykiety osi trzeba utworzyć nazwane formuły, odpowiednio:
* seria danych **rngWykonanie**
`=INDEKS($B:$B;PODAJ.POZYCJĘ($G$2;$A:$A;0)):INDEKS($B:$B;PODAJ.POZYCJĘ($G$3;$A:$A;0))`
* seria danych **rngWysyłka**
`=INDEKS($C:$C;PODAJ.POZYCJĘ($G$2;$A:$A;0)):INDEKS($C:$C;PODAJ.POZYCJĘ($G$3;$A:$A;0))`
* etykieta osi **rngData**
`=INDEKS($A:$A;PODAJ.POZYCJĘ($G$2;$A:$A;0)):INDEKS($A:$A;PODAJ.POZYCJĘ($G$3;$A:$A;0))`

Co się dzieje powyżej? Otóż dla każdej z formuł zwracany jest nowy zakres danych dla każdej z kolumn **A**, **B** i **C**. W tym wypadku od 11 do 21 wiersza. Czyli uzyskujemy **A11:A21**, **B11:B21**, **C11:C21**. Całą robotę odwala formuła `PODAJ.POZYCJĘ()`, która znajduje wiersze spełniające warunki daty.

Zawartość **Menedżera nazw** powinna wyglądać tak jak poniżej.

![menadżer nazw](/assets/2017-03-24-dynamiczny-wykres/name-manager.png "menadżer nazw")

Następnie po dodaniu nazwanych formuł pora na edycję wykresu.

![edycja wykresu](/assets/2017-03-24-dynamiczny-wykres/chart-edit-data-series.gif "edycja wykresu")

A tak prezentuje się nasz końcowy efekt pracy, czyli piękny, dynamiczny wykres ;-)

![dynamiczny wykres](/assets/2017-03-24-dynamiczny-wykres/dynamic-chart.gif "dynamiczny wykres")