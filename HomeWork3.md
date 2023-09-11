# CONTAINER_GB

## Урок 3. Введение в Docker

### Задача

### Хранение данных в контейнерах Docker часть 1.

Проверяем что наш Docker работает введя команду:
```
docker run docker/whalesay cowsay -f elephant "Hello, Docker!"
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-1.png)

Видим что наш Docker работает, начинаем нашу задачу:

Запустим контейнер из образа Ubuntu, войдем в него, потом создадим новую директорию в корне. После создаем файл и записываем в него данные. Потом смотрим наши данные в созданном контейнере.

1. С помощью команды Docker запустим наш контейнер из образа Ubuntu и зайдем в него:
```
docker run -it -h GB --name gb-test ubuntu:22.10
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-3.png)
2. Проверим наличие нашего созданного контейнера командой:
```
ls /
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-3.png)
3. Также создадим новую директорию в корневой папке командой:
```
mkdir /example
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-2.png)
4. В директории example добавим текстовый файл:
```
touch /example/passwords.txt
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-4.png)
5. Давайте в него добавим какие-нибудь данные:
```
echo "test123" >> /example/passwords.txt
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-4.png)
6. Остановим наш контейнер, запустим и зайдем в него снова при помощи команд:
```
docker stop gb-test

docker start gb-test

docker exec -it gb-test bash
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-5.png)
7. Проверим наши данные записанные в файл командой:
```
cat /example/passwords.txt
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-6.png)
Как мы можем увидеть данные записались и все работает исправно.

### Хранение данных в контейнерах Docker часть 2.

1. Создайте папку, которую мы будем готовы смонтировать в контейнер:
```
mkdir ~/docker-mount-example
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-7.png)
2. В этой папке создайте файл test.txt и наполните его данными:
```
echo "This is the host test.txt file" > ~/docker-mount-example/test.txt
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-8.png)
3. Создайте контейнер из образа ubuntu:22.10 и задайте ему имя и hostname:
```
docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount ubuntu:22.10
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-9.png)
4. Смонтируйте созданный ранее текстовый файл из домашней директории внутрь смонтированной папки в контейнере:
```
docker exec -it gb-test bash

echo "This is the root test.txt file" > /container-mount/test.txt

exit
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-10.png)
5. Посмотрите содержимое текстового файла в контейнере:
```
docker exec -it gb-test cat /container-mount/test.txt
```
![](https://github.com/Mihail148/CONTAINER_GB/blob/main/PICTURES_HW3/%D0%94%D0%973%20%D1%81%D0%BA%D1%80%D0%B8%D0%BD-11.png)

Объяснение: Мы создали контейнер и монтировали папку docker-mount-example внутрь контейнера. Затем мы монтировали файл test.txt из домашней директории внутрь этой папки в контейнере. При просмотре содержимого файла в контейнере, вы увидите данные из файла в домашней директории.
