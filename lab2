Лабораторная работа №2. Установка и первая программа на PHP

Цель работы

Целью данной лабораторной работы является установка и настройка среды разработки для работы с языком программирования PHP, а также создание первой программы на PHP.

Для начала, я установил XAMPP и включил Apache

<img width="825" height="531" alt="image" src="https://github.com/user-attachments/assets/d80e7cc0-74e7-418e-aa83-2c234fdfea3c" />

В папке с XAMPP я нашел другую папку, htdocs, в которой создал 01_Introduction, и уже в ней был создан файл index.php

<img width="1028" height="625" alt="image" src="https://github.com/user-attachments/assets/b5a023ff-2578-4ccc-aa37-5dc53e64e5d1" />

Внутри файла находится текст: "<?php

echo "Привет, мир!";
"

Вывод по http://localhost/01_Introduction такой:

<img width="1912" height="194" alt="image" src="https://github.com/user-attachments/assets/c35507b4-9953-4186-bae1-798627f1a80e" />

После чего содержимое файла было заменено на: "<?php

echo "Hello, World with echo!<br />";

print "Hello, World with print!";

?>"

Результат:

<img width="1917" height="179" alt="image" src="https://github.com/user-attachments/assets/f240a251-4531-48be-8f7d-383332a472a4" />

Далее содержимое заменено на: "<?php

$days = 288;

$message = "Все возвращаются на работу!";

echo $days . " дней. " . $message . "<br />";

echo "$days дней. $message<br />";

print $days . " дней. " . $message . "<br />";

?>"

Результат:

<img width="1918" height="188" alt="image" src="https://github.com/user-attachments/assets/ac9dbef1-2d03-4205-b2e7-ae8dc92560c5" />

После этого я заменил содержимое файла на: "<?php

phpinfo();

?>"

Обновлённая страница выглядит так:

<img width="1917" height="975" alt="image" src="https://github.com/user-attachments/assets/99a638ac-08c9-473d-9372-8e62a6d84340" />

Ответы на контрольные вопросы:

Какие способы установки PHP существуют?

PHP можно установить отдельно с официального сайта php.net, используя готовые сборки вроде XAMPP, OpenServer или WAMP, а также через Docker или менеджеры пакетов (например, apt или brew).

Как проверить, что PHP установлен и работает?

Проверить установку можно, запустив команду php -v в командной строке, чтобы увидеть версию PHP.
Также можно создать файл с функцией phpinfo() и открыть его через браузер — если отображается страница с настройками PHP, значит всё работает корректно.

Чем отличается оператор echo от print?

Оператор echo может выводить несколько значений и работает немного быстрее, поскольку не возвращает значение. print выводит только одно значение и возвращает 1, поэтому его можно использовать внутри выражений.
