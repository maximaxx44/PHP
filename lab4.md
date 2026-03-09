Лабораторная работа №4. Массивы и Функции

Цель работы

Освоить работу с массивами в PHP, применяя различные операции: создание, добавление, удаление, сортировка и поиск. Закрепить навыки работы с функциями, включая передачу аргументов, возвращаемые значения и анонимные функции.

Задание 1. Работа с массивами

Описание

В данной части работы реализована система управления банковскими транзакциями.

Каждая транзакция хранится в массиве и содержит следующие поля:

id - уникальный идентификатор транзакции

date - дата транзакции

amount - сумма транзакции

description - описание платежа

merchant - организация, получившая платеж

Реализованы следующие функции:

подсчёт общей суммы транзакций

поиск транзакции по описанию

поиск транзакции по ID

добавление транзакции

удаление транзакции

сортировка транзакций

подсчёт дней с момента транзакции

Полный код программы:

```<?php

declare(strict_types=1);

/**

 * Массив банковских транзакций.
 
 *
 
 * @var array<int, array{
 
 *     id:int,
 
 *     date:string,
 
 *     amount:float,
 
 *     description:string,
 
 *     merchant:string
 
 * }>
 
 */
 
$transactions = [

    [
    
        'id' => 1,
        
        'date' => '2024-01-10',
        
        'amount' => 120.50,
        
        'description' => 'Payment for groceries',
        
        'merchant' => 'SuperMart',
        
    ],
    
    [
    
        'id' => 2,
        
        'date' => '2024-02-14',
        
        'amount' => 75.99,
        
        'description' => 'Dinner with friends',
        
        'merchant' => 'Local Restaurant',
        
    ],
    
    [
    
        'id' => 3,
        
        'date' => '2024-03-01',
        
        'amount' => 220.00,
        
        'description' => 'Online course payment',
        
        'merchant' => 'EduPlatform',

    ],
    
    [
    
        'id' => 4,
        
        'date' => '2024-03-15',
        
        'amount' => 45.20,
        
        'description' => 'Taxi ride',
        
        'merchant' => 'CityTaxi',
        
    ],
    
    [
    
        'id' => 5,
        
        'date' => '2024-04-05',
        
        'amount' => 310.75,
        
        'description' => 'Electronics purchase',
        'merchant' => 'TechStore',
    ],
];

/**
 * Вычисляет общую сумму всех транзакций.
 *
 * @param array<int, array{id:int, date:string, amount:float, description:string, merchant:string}> $transactions
 * @return float Общая сумма транзакций.
 */
function calculateTotalAmount(array $transactions): float
{
    $total = 0.0;

    foreach ($transactions as $transaction) {
        $total += $transaction['amount'];
    }

    return $total;
}

/**
 * Ищет транзакции по части описания.
 *
 * @param string $descriptionPart Часть описания для поиска.
 * @return array<int, array{id:int, date:string, amount:float, description:string, merchant:string}>
 */
function findTransactionByDescription(string $descriptionPart): array
{
    global $transactions;

    $result = [];

    foreach ($transactions as $transaction) {
        if (stripos($transaction['description'], $descriptionPart) !== false) {
            $result[] = $transaction;
        }
    }

    return $result;
}

/**
 * Ищет транзакцию по идентификатору с помощью foreach.
 *
 * @param int $id Идентификатор транзакции.
 * @return array{id:int, date:string, amount:float, description:string, merchant:string}|null
 */
function findTransactionById(int $id): ?array
{
    global $transactions;

    foreach ($transactions as $transaction) {
        if ($transaction['id'] === $id) {
            return $transaction;
        }
    }

    return null;
}

/**
 * Ищет транзакцию по идентификатору с помощью array_filter.
 *
 * @param int $id Идентификатор транзакции.
 * @return array{id:int, date:string, amount:float, description:string, merchant:string}|null
 */
function findTransactionByIdWithFilter(int $id): ?array
{
    global $transactions;

    $filteredTransactions = array_filter(
        $transactions,
        fn(array $transaction): bool => $transaction['id'] === $id
    );

    if (empty($filteredTransactions)) {
        return null;
    }

    return array_values($filteredTransactions)[0];
}

/**
 * Возвращает количество дней с даты транзакции до текущего дня.
 *
 * @param string $date Дата транзакции в формате YYYY-MM-DD.
 * @return int Количество дней.
 */
function daysSinceTransaction(string $date): int
{
    $transactionDate = new DateTime($date);
    $currentDate = new DateTime();

    return (int)$transactionDate->diff($currentDate)->days;
}

/**
 * Добавляет новую транзакцию в глобальный массив.
 *
 * @param int $id Уникальный идентификатор транзакции.
 * @param string $date Дата транзакции.
 * @param float $amount Сумма транзакции.
 * @param string $description Описание платежа.
 * @param string $merchant Название организации.
 * @return void
 */
function addTransaction(int $id, string $date, float $amount, string $description, string $merchant): void
{
    global $transactions;

    $transactions[] = [
        'id' => $id,
        'date' => $date,
        'amount' => $amount,
        'description' => $description,
        'merchant' => $merchant,
    ];
}

/**
 * Удаляет транзакцию по идентификатору.
 *
 * @param int $id Идентификатор транзакции.
 * @return bool true, если транзакция удалена, иначе false.
 */
function deleteTransactionById(int $id): bool
{
    global $transactions;

    foreach ($transactions as $index => $transaction) {
        if ($transaction['id'] === $id) {
            unset($transactions[$index]);
            $transactions = array_values($transactions);
            return true;
        }
    }

    return false;
}

/**
 * Сортирует транзакции по дате по возрастанию.
 *
 * @param array<int, array{id:int, date:string, amount:float, description:string, merchant:string}> $transactions
 * @return array<int, array{id:int, date:string, amount:float, description:string, merchant:string}>
 */
function sortTransactionsByDate(array $transactions): array
{
    usort(
        $transactions,
        fn(array $a, array $b): int => strtotime($a['date']) <=> strtotime($b['date'])
    );

    return $transactions;
}

/**
 * Сортирует транзакции по сумме по убыванию.
 *
 * @param array<int, array{id:int, date:string, amount:float, description:string, merchant:string}> $transactions
 * @return array<int, array{id:int, date:string, amount:float, description:string, merchant:string}>
 */
function sortTransactionsByAmountDesc(array $transactions): array
{
    usort(
        $transactions,
        fn(array $a, array $b): int => $b['amount'] <=> $a['amount']
    );

    return $transactions;
}

/**
 * Возвращает список изображений из указанной папки.
 * Поддерживаются файлы .jpg и .jpeg.
 *
 * @param string $dir Путь к директории с изображениями.
 * @return array<int, string> Список путей к изображениям.
 */
function getImagesFromDirectory(string $dir): array
{
    if (!is_dir($dir)) {
        return [];
    }

    $files = scandir($dir);

    if ($files === false) {
        return [];
    }

    $images = [];

    foreach ($files as $file) {
        if ($file === '.' || $file === '..') {
            continue;
        }

        $path = $dir . $file;

        if (is_file($path) && preg_match('/\.(jpg|jpeg)$/i', $file)) {
            $images[] = $path;
        }
    }

    return $images;
}

/*
|--------------------------------------------------------------------------
| Демонстрация работы функций
|--------------------------------------------------------------------------
*/

// Добавление новой транзакции
addTransaction(6, '2024-05-20', 99.90, 'Book purchase', 'BookShop');

// Удаление транзакции
$deleteResult = deleteTransactionById(2);

// Поиск по описанию
$transactionsByDescription = findTransactionByDescription('payment');

// Поиск по ID через foreach
$transactionById = findTransactionById(3);

// Поиск по ID через array_filter
$transactionByIdWithFilter = findTransactionByIdWithFilter(5);

// Сортировка
$transactionsSortedByDate = sortTransactionsByDate($transactions);
$transactionsSortedByAmount = sortTransactionsByAmountDesc($transactions);

// Общая сумма
$totalAmount = calculateTotalAmount($transactions);

// Изображения из папки image
$images = getImagesFromDirectory('image/');

?>
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Лабораторная работа по PHP</title>
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #0b1320;
            color: #222;
        }

        .wrapper {
            max-width: 1200px;
            margin: 0 auto;
            min-height: 100vh;
            background: #f5f5f5;
        }

        header {
            background: #111827;
            color: #fff;
            padding: 25px;
            text-align: center;
        }

        nav {
            background: #e5e7eb;
            padding: 15px;
            text-align: center;
        }

        nav a {
            text-decoration: none;
            color: #111;
            margin: 0 15px;
            font-weight: bold;
        }

        main {
            padding: 20px;
        }

        section {
            margin-bottom: 35px;
        }

        h1, h2, h3 {
            margin-top: 0;
        }

        .block {
            background: #fff;
            border: 1px solid #ccc;
            padding: 15px;
            margin-bottom: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: #fff;
        }

        th, td {
            border: 1px solid #bbb;
            padding: 10px;
            text-align: left;
        }

        th {
            background: #dbeafe;
        }

        ul {
            margin: 10px 0 0;
        }

        pre {
            background: #f3f4f6;
            padding: 10px;
            overflow-x: auto;
            border: 1px solid #ddd;
        }

        .gallery {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
            gap: 15px;
        }

        .gallery img {
            width: 100%;
            height: 220px;
            object-fit: cover;
            border: 1px solid #999;
            border-radius: 6px;
            background: #fff;
            display: block;
        }

        footer {
            background: #111827;
            color: #fff;
            text-align: center;
            padding: 20px;
        }
    </style>
</head>
<body>
<div class="wrapper">
    <header>
        <h1>Система управления транзакциями и галерея изображений</h1>
    </header>

    <nav>
        <a href="#transactions">Транзакции</a>
        <a href="#search">Поиск</a>
        <a href="#sorting">Сортировка</a>
        <a href="#gallery">Галерея</a>
    </nav>

    <main>
        <section id="transactions">
            <h2>Задание 1. Список транзакций</h2>

            <div class="block">
                <p><strong>Результат удаления транзакции с ID = 2:</strong>
                    <?= $deleteResult ? 'транзакция удалена' : 'транзакция не найдена' ?>
                </p>
            </div>

            <table border="1">
                <thead>
                <tr>
                    <th>ID</th>
                    <th>Дата</th>
                    <th>Сумма</th>
                    <th>Описание</th>
                    <th>Получатель</th>
                    <th>Дней с момента транзакции</th>
                </tr>
                </thead>
                <tbody>
                <?php foreach ($transactions as $transaction): ?>
                    <tr>
                        <td><?= htmlspecialchars((string)$transaction['id']) ?></td>
                        <td><?= htmlspecialchars($transaction['date']) ?></td>
                        <td><?= number_format($transaction['amount'], 2, '.', ' ') ?></td>
                        <td><?= htmlspecialchars($transaction['description']) ?></td>
                        <td><?= htmlspecialchars($transaction['merchant']) ?></td>
                        <td><?= daysSinceTransaction($transaction['date']) ?></td>
                    </tr>
                <?php endforeach; ?>
                <tr>
                    <td colspan="2"><strong>Общая сумма</strong></td>
                    <td colspan="4"><strong><?= number_format($totalAmount, 2, '.', ' ') ?></strong></td>
                </tr>
                </tbody>
            </table>
        </section>

        <section id="search">
            <h2>Поиск транзакций</h2>

            <div class="block">
                <h3>Поиск по части описания: "payment"</h3>
                <?php if (!empty($transactionsByDescription)): ?>
                    <ul>
                        <?php foreach ($transactionsByDescription as $item): ?>
                            <li>
                                ID <?= $item['id'] ?> —
                                <?= htmlspecialchars($item['description']) ?> —
                                <?= number_format($item['amount'], 2, '.', ' ') ?>
                            </li>
                        <?php endforeach; ?>
                    </ul>
                <?php else: ?>
                    <p>Ничего не найдено.</p>
                <?php endif; ?>
            </div>

            <div class="block">
                <h3>Поиск по ID = 3 через foreach</h3>
                <pre><?php print_r($transactionById); ?></pre>
            </div>

            <div class="block">
                <h3>Поиск по ID = 5 через array_filter</h3>
                <pre><?php print_r($transactionByIdWithFilter); ?></pre>
            </div>
        </section>

        <section id="sorting">
            <h2>Сортировка транзакций</h2>

            <div class="block">
                <h3>Сортировка по дате</h3>
                <ul>
                    <?php foreach ($transactionsSortedByDate as $item): ?>
                        <li>
                            <?= htmlspecialchars($item['date']) ?> —
                            <?= htmlspecialchars($item['description']) ?> —
                            <?= number_format($item['amount'], 2, '.', ' ') ?>
                        </li>
                    <?php endforeach; ?>
                </ul>
            </div>

            <div class="block">
                <h3>Сортировка по сумме по убыванию</h3>
                <ul>
                    <?php foreach ($transactionsSortedByAmount as $item): ?>
                        <li>
                            <?= htmlspecialchars($item['description']) ?> —
                            <?= number_format($item['amount'], 2, '.', ' ') ?>
                        </li>
                    <?php endforeach; ?>
                </ul>
            </div>
        </section>

        <section id="gallery">
            <h2>Задание 2. Галерея изображений</h2>
            <div class="block">
                <p>Изображения загружаются автоматически из папки <code>image/</code>.</p>
            </div>

            <div class="gallery">
                <?php if (!empty($images)): ?>
                    <?php foreach ($images as $image): ?>
                        <img src="<?= htmlspecialchars($image) ?>" alt="Изображение">
                    <?php endforeach; ?>
                <?php else: ?>
                    <p>В папке image/ не найдено JPG-изображений.</p>
                <?php endif; ?>
            </div>
        </section>

        <section id="answers">
            <h2>Контрольные вопросы</h2>

            <div class="block">
                <h3>Что такое массивы в PHP?</h3>
                <p>
                    Массив в PHP — это структура данных, которая позволяет хранить несколько значений
                    в одной переменной. Массивы бывают индексированными и ассоциативными.
                </p>
            </div>

            <div class="block">
                <h3>Каким образом можно создать массив в PHP?</h3>
                <p>Массив можно создать с помощью квадратных скобок:</p>
                <pre>$numbers = [1, 2, 3];</pre>
                <p>Или ассоциативный массив:</p>
                <pre>$user = ['name' => 'Ivan', 'age' => 20];</pre>
            </div>

            <div class="block">
                <h3>Для чего используется цикл foreach?</h3>
                <p>
                    Цикл foreach используется для перебора элементов массива.
                    Он позволяет последовательно обработать каждый элемент без ручной работы с индексами.
                </p>
            </div>
        </section>
    </main>

    <footer>
        <p>USM © 2024</p>
    </footer>
</div>
</body>
</html>
```

Объяснение основных блоков программы

Массив транзакций

В начале программы создаётся массив $transactions, содержащий список транзакций.

Каждый элемент массива представляет собой ассоциативный массив с данными о транзакции.

Пример элемента:
```
[
"id" => 1,
"date" => "2024-01-10",
"amount" => 120.50,
"description" => "Payment for groceries",
"merchant" => "SuperMart"
]
```
Вычисление общей суммы транзакций

Функция calculateTotalAmount() перебирает массив транзакций с помощью цикла foreach и суммирует значения поля amount.
```
foreach ($transactions as $transaction) {
$total += $transaction["amount"];
}
```
Поиск транзакции по описанию

Функция findTransactionByDescription() ищет транзакции, содержащие определённую часть текста в описании.

Для этого используется функция stripos(), которая выполняет поиск строки без учета регистра.

Поиск транзакции по ID

Реализован двумя способами:

Через foreach

Программа перебирает все элементы массива и проверяет совпадение ID.

Через array_filter

Используется встроенная функция PHP для фильтрации массива.

array_filter($transactions, fn($transaction) => $transaction["id"] === $id);

Добавление транзакции

Функция addTransaction() добавляет новую транзакцию в массив.

$transactions[] = [...]

Это добавляет новый элемент в конец массива.

Удаление транзакции

Функция deleteTransactionById() ищет транзакцию по ID и удаляет её с помощью unset().

После удаления используется array_values() для перенумерации индексов массива.

Сортировка транзакций

Для сортировки используется функция usort().

Сортировка по дате
```
usort($transactions, fn($a, $b) => strtotime($a["date"]) <=> strtotime($b["date"]));
Сортировка по сумме
usort($transactions, fn($a, $b) => $b["amount"] <=> $a["amount"]);
```

Задание 2. Работа с файловой системой

Описание

В данной части работы реализован вывод изображений из директории image.

Используется функция scandir(), которая получает список файлов из указанной папки.

Пример кода:
```
$dir = 'image/';
$files = scandir($dir);

foreach ($files as $file) {

if ($file != "." && $file != "..") {

$path = $dir . $file;

echo "<img src='$path' width='200'>";
}
}
```
Программа перебирает файлы в папке и выводит каждое изображение на веб-страницу.

Контрольные вопросы

Что такое массивы в PHP?

Массив - это структура данных, которая позволяет хранить несколько значений в одной переменной.

Массивы могут быть индексированными (элементы имеют числовые индексы (0, 1, 2…)) или ассоциативными (элементы имеют именованные ключи).

Каким образом можно создать массив в PHP?

Массив можно создать с помощью квадратных скобок [] или функции array(). Внутри массива перечисляются элементы, разделённые запятыми.

Для чего используется цикл foreach?

Цикл foreach используется для перебора элементов массива. Он позволяет последовательно обработать каждый элемент без использования индексов.
