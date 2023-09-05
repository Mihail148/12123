# CONTAINER_GB

## Урок 1. Механизмы пространства имен

### Задача.
Необходимо продемонстрировать изоляцию одного и того же приложения (как решено на семинаре - командного интерпретатора) в различных пространствах имен.

### Как сдавать домашнее задание
Формат сдачи ДЗ: предоставить доказательства выполнения задания посредством ссылки на google-документ с правами на комментирование/редактирование.
Результатом работы будет: текст объяснения, логи выполнения, история команд и скриншоты (важно придерживаться такой последовательности).

### Задание.
Запустим Bash в новом пространстве при помощи команды(так как команда требует привилегии пользователя, то выполняем команду с sudo):
```
sudo unshare -pf -n --mount-proc bash
```
![Начало кода](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW1/%D0%94%D0%971%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-1.png)

В параллельном терминале смотрим, что произошло:
```
ps -afx
```
...
Наблюдаем несколько родительских процессов, которые породили одинаковые процессы Bash. Заметим, что PID у нового процесса 3421.
...
Для проверки и наглядности смотрим той же командой ps -afx в изолированном терминале
...
Здесь мы сожем видеть, что изолированная оболчка видит всего два процесса (и то, второй процесс сразу же, после выполнения команды, исчезнет). При этом PID процессов начинается с 1.

Далее, из этого же терминала, пробуем запустить пинг любого сайта, например ya.ru.
...
Наблюдаем, что пинг до указанного сайта не может быть осуществлен, так как сеть в этом пронстранстве имен имеется только локальная, т.е. localhost.

А теперь, ту же команду запустим из параллельного терминала (который не изолирован).
...
Видим, что пакеты до сайта уходят нормально и ответ от сервера принимается.

Теперь, в оба терминала отправляем команду $ hostname, что бы увидеть наш хост.
```
hostname
```
...
Как мы можем заметить - хоcт и там и там одинаков. А теперь, в изолированном терминале выполняем команду:
```
sudo unshare -u bash
```
Команда $ unshare запускает программу (опционально) в новом namespace. Флаг -u говорит ей запустить bash в новом UTS namespace. Обратите внимание, что наш новый процесс bash указывает на другой файл UTS, тогда как все остальные остаются прежними.

Одним из следствий того, что мы только что проделали, является то, что теперь мы можем изменить системный hostname из нашего нового процесса bash и это не повлияет ни на какой другой процесс в системе. Изменим наш хост в изолированном терминале, например:
```
hostname geekbrains
```
Эта команда никак не затронула хост основной системы. Можем проверить это, выполнив hostname в первом терминале и увидев, что имя хоста там не изменилось.
...
Этими манипуляциями мы доказали, что научились запускать процессы в разных пространствах с возможностью(опционально) изолировать нужные нам направления.
Сделал студент Geekbrains Ракицкий Михаил.
