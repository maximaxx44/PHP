Лабораторная работа №5. Объектно-ориентированное программирование в PHP

Цель работы

Освоить основы объектно-ориентированного программирования в PHP на практике. 

Научиться создавать собственные классы, использовать инкапсуляцию для защиты данных, разделять ответственность между классами, а также применять интерфейсы для построения гибкой архитектуры приложения.

Условие

Необходимо разработать приложение для управления банковскими транзакциями.

Приложение должно позволять:

хранить банковские транзакции;

добавлять новые транзакции;

удалять транзакции;

искать транзакции;

сортировать транзакции;

выполнять вычисления над коллекцией транзакций;

выводить данные в виде HTML-таблицы.

В рамках лабораторной работы необходимо использовать объектно-ориентированный подход.

Все классы можно разместить:

либо в одном файле index.php для упрощения;

либо в отдельных файлах, если вы хотите организовать структуру проекта аккуратнее.

Задание 1. Включение строгой типизации

В начале файла включите строгую типизацию:
```
<?php

declare(strict_types=1);
```
Задание 2. Класс Transaction

Создайте класс Transaction, который описывает одну банковскую транзакцию.

Класс должен содержать следующие свойства:

id — уникальный идентификатор транзакции;

date — дата транзакции;

amount — сумма транзакции;

description — описание платежа;

merchant — получатель платежа.

Класс должен содержать метод: getDaysSinceTransaction(): int, который возвращает количество дней с момента транзакции до текущей даты.

Требования:

Все свойства должны быть приватными.

Значения свойств должны задаваться через конструктор.

Для получения данных создайте getter-методы.

[!TIP]

Для работы с датами используйте класс DateTime.

Задание 3. Класс TransactionRepository

Создайте класс TransactionRepository, который будет управлять коллекцией транзакций. 

Этот класс должен отвечать только за хранение данных и базовые операции доступа к ним.

Класс должен:

хранить массив объектов Transaction

добавлять новые транзакции;

addTransaction(Transaction $transaction): void

удалять транзакции по идентификатору;

removeTransactionById(int $id): void

возвращать полный список транзакций;

getAllTransactions(): array

находить транзакцию по id.

findById(int $id): ?Transaction

Требования:

Массив транзакций должен быть приватным свойством класса.

Доступ к данным должен осуществляться только через методы класса.

Не допускается прямой доступ к массиву транзакций извне.

Для методов и свойств используйте строгую типизацию.

адание 4. Класс TransactionManager

Создайте класс TransactionManager, который будет использовать TransactionRepository для выполнения бизнес-логики.

TransactionManager не должен создавать транзакции самостоятельно и не должен хранить их внутри себя. 

Объект TransactionRepository необходимо передать в TransactionManager через конструктор:
```
public function __construct(

    private TransactionRepository $repository
) {
}
```
Класс должен реализовать следующие функции:

вычисление общей суммы всех транзакций;
```
calculateTotalAmount(): float
```
вычисление суммы транзакций за определенный период;
```
calculateTotalAmountByDateRange(string $startDate, string $endDate): float
```
подсчет количества транзакций по определенному получателю;
```
countTransactionsByMerchant(string $merchant): int
```
сортировку транзакций по дате;
```
sortTransactionsByDate(): Transaction[]
```
сортировку транзакций по сумме по убыванию.
```
sortTransactionsByAmountDesc(): Transaction[]
```
Требования:

TransactionManager не должен хранить транзакции самостоятельно.

Для получения данных он должен обращаться к объекту репозитория.

В классе должны быть методы для каждой из перечисленных функций.

При необходимости создавайте приватные вспомогательные методы.

Задание 5. Класс TransactionTableRenderer

Создайте отдельный класс TransactionTableRenderer, который отвечает только за вывод транзакций в HTML. 

Этот класс должен получать список транзакций и формировать HTML-таблицу.

Класс должен реализовать следующие функции:

render(array $transactions): string — принимает массив транзакций и возвращает строку с HTML-кодом таблицы.

Метод должен возвращать HTML-таблицу со следующими столбцами:

ID транзакции;

дата;

сумма;

описание;

название получателя;

категория получателя;

количество дней с момента транзакции.

Требования:

HTML-код должен генерироваться внутри класса.

В основном файле должен выполняться только вызов метода render() и вывод результата через echo.

Класс рекомендуется объявить как final.

Задание 6. Начальные данные

Создайте не менее 10 объектов Transaction. Каждая транзакция должна содержать:

разные даты;

разные суммы;

разные описания;

разных получателей.

После создания объектов добавьте транзакции в TransactionRepository.

Задание 7. Интерфейс TransactionStorageInterface

После завершения основной реализации сделайте архитектуру более гибкой.

Создайте интерфейс TransactionStorageInterface.

Интерфейс должен содержать методы:

addTransaction(Transaction $transaction): void

removeTransactionById(int $id): void

getAllTransactions(): array

findById(int $id): ?Transaction

Требования:

TransactionRepository должен реализовывать интерфейс.

TransactionManager должен принимать через конструктор уже интерфейс, а не конкретный класс.

После изменения конструктор TransactionManager должен выглядеть следующим образом:
```
public function __construct(
    private TransactionStorageInterface $repository
) {
}
```

Выполнение:

Используемый код:
```
<?php
declare(strict_types=1);

ini_set('display_errors', '1');
error_reporting(E_ALL);

/**
 * Класс, описывающий одну банковскую транзакцию.
 */
class Transaction
{
    /**
     * @param int $id Уникальный идентификатор транзакции
     * @param string $date Дата транзакции
     * @param float $amount Сумма транзакции
     * @param string $description Описание платежа
     * @param string $merchant Получатель платежа
     */
    public function __construct(
        private int $id,
        private string $date,
        private float $amount,
        private string $description,
        private string $merchant
    ) {
    }

    /**
     * Возвращает идентификатор транзакции.
     *
     * @return int
     */
    public function getId(): int
    {
        return $this->id;
    }

    /**
     * Возвращает дату транзакции.
     *
     * @return string
     */
    public function getDate(): string
    {
        return $this->date;
    }

    /**
     * Возвращает сумму транзакции.
     *
     * @return float
     */
    public function getAmount(): float
    {
        return $this->amount;
    }

    /**
     * Возвращает описание платежа.
     *
     * @return string
     */
    public function getDescription(): string
    {
        return $this->description;
    }

    /**
     * Возвращает получателя платежа.
     *
     * @return string
     */
    public function getMerchant(): string
    {
        return $this->merchant;
    }

    /**
     * Возвращает количество дней с момента транзакции до текущей даты.
     *
     * @return int
     * @throws Exception
     */
    public function getDaysSinceTransaction(): int
    {
        $transactionDate = new DateTime($this->date);
        $currentDate = new DateTime();

        return (int)$transactionDate->diff($currentDate)->days;
    }
}

/**
 * Интерфейс хранилища транзакций.
 */
interface TransactionStorageInterface
{
    /**
     * Добавляет транзакцию в хранилище.
     *
     * @param Transaction $transaction
     * @return void
     */
    public function addTransaction(Transaction $transaction): void;

    /**
     * Удаляет транзакцию по идентификатору.
     *
     * @param int $id
     * @return void
     */
    public function removeTransactionById(int $id): void;

    /**
     * Возвращает все транзакции.
     *
     * @return Transaction[]
     */
    public function getAllTransactions(): array;

    /**
     * Ищет транзакцию по идентификатору.
     *
     * @param int $id
     * @return Transaction|null
     */
    public function findById(int $id): ?Transaction;
}

/**
 * Класс-репозиторий для хранения транзакций.
 */
class TransactionRepository implements TransactionStorageInterface
{
    /**
     * @var Transaction[]
     */
    private array $transactions = [];

    /**
     * Добавляет транзакцию в массив.
     *
     * @param Transaction $transaction
     * @return void
     */
    public function addTransaction(Transaction $transaction): void
    {
        $this->transactions[] = $transaction;
    }

    /**
     * Удаляет транзакцию по идентификатору.
     *
     * @param int $id
     * @return void
     */
    public function removeTransactionById(int $id): void
    {
        foreach ($this->transactions as $key => $transaction) {
            if ($transaction->getId() === $id) {
                unset($this->transactions[$key]);
                break;
            }
        }

        $this->transactions = array_values($this->transactions);
    }

    /**
     * Возвращает полный список транзакций.
     *
     * @return Transaction[]
     */
    public function getAllTransactions(): array
    {
        return $this->transactions;
    }

    /**
     * Ищет транзакцию по идентификатору.
     *
     * @param int $id
     * @return Transaction|null
     */
    public function findById(int $id): ?Transaction
    {
        foreach ($this->transactions as $transaction) {
            if ($transaction->getId() === $id) {
                return $transaction;
            }
        }

        return null;
    }
}

/**
 * Класс бизнес-логики для работы с транзакциями.
 */
class TransactionManager
{
    /**
     * @param TransactionStorageInterface $repository
     */
    public function __construct(
        private TransactionStorageInterface $repository
    ) {
    }

    /**
     * Вычисляет общую сумму всех транзакций.
     *
     * @return float
     */
    public function calculateTotalAmount(): float
    {
        $total = 0.0;

        foreach ($this->repository->getAllTransactions() as $transaction) {
            $total += $transaction->getAmount();
        }

        return $total;
    }

    /**
     * Вычисляет сумму транзакций за указанный период.
     *
     * @param string $startDate
     * @param string $endDate
     * @return float
     * @throws Exception
     */
    public function calculateTotalAmountByDateRange(string $startDate, string $endDate): float
    {
        $start = new DateTime($startDate);
        $end = new DateTime($endDate);
        $total = 0.0;

        foreach ($this->repository->getAllTransactions() as $transaction) {
            $currentDate = new DateTime($transaction->getDate());

            if ($currentDate >= $start && $currentDate <= $end) {
                $total += $transaction->getAmount();
            }
        }

        return $total;
    }

    /**
     * Подсчитывает количество транзакций по получателю.
     *
     * @param string $merchant
     * @return int
     */
    public function countTransactionsByMerchant(string $merchant): int
    {
        $count = 0;

        foreach ($this->repository->getAllTransactions() as $transaction) {
            if (mb_strtolower($transaction->getMerchant()) === mb_strtolower($merchant)) {
                $count++;
            }
        }

        return $count;
    }

    /**
     * Сортирует транзакции по дате по возрастанию.
     *
     * @return Transaction[]
     * @throws Exception
     */
    public function sortTransactionsByDate(): array
    {
        $transactions = $this->repository->getAllTransactions();

        usort(
            $transactions,
            function (Transaction $first, Transaction $second): int {
                $firstDate = new DateTime($first->getDate());
                $secondDate = new DateTime($second->getDate());

                return $firstDate <=> $secondDate;
            }
        );

        return $transactions;
    }

    /**
     * Сортирует транзакции по сумме по убыванию.
     *
     * @return Transaction[]
     */
    public function sortTransactionsByAmountDesc(): array
    {
        $transactions = $this->repository->getAllTransactions();

        usort(
            $transactions,
            fn(Transaction $first, Transaction $second): int => $second->getAmount() <=> $first->getAmount()
        );

        return $transactions;
    }
}

/**
 * Класс для вывода транзакций в HTML-таблицу.
 */
final class TransactionTableRenderer
{
    /**
     * Формирует HTML-таблицу по списку транзакций.
     *
     * @param Transaction[] $transactions
     * @return string
     */
    public function render(array $transactions): string
    {
        $html = '<table border="1" cellpadding="8" cellspacing="0" style="border-collapse: collapse; width: 100%; margin-bottom: 20px;">';
        $html .= '<tr style="background-color: #f2f2f2;">';
        $html .= '<th>ID транзакции</th>';
        $html .= '<th>Дата</th>';
        $html .= '<th>Сумма</th>';
        $html .= '<th>Описание</th>';
        $html .= '<th>Название получателя</th>';
        $html .= '<th>Категория получателя</th>';
        $html .= '<th>Количество дней с момента транзакции</th>';
        $html .= '</tr>';

        foreach ($transactions as $transaction) {
            $html .= '<tr>';
            $html .= '<td>' . htmlspecialchars((string)$transaction->getId()) . '</td>';
            $html .= '<td>' . htmlspecialchars($transaction->getDate()) . '</td>';
            $html .= '<td>' . htmlspecialchars(number_format($transaction->getAmount(), 2, '.', '')) . '</td>';
            $html .= '<td>' . htmlspecialchars($transaction->getDescription()) . '</td>';
            $html .= '<td>' . htmlspecialchars($transaction->getMerchant()) . '</td>';
            $html .= '<td>' . htmlspecialchars($this->getMerchantCategory($transaction->getMerchant())) . '</td>';
            $html .= '<td>' . htmlspecialchars((string)$transaction->getDaysSinceTransaction()) . '</td>';
            $html .= '</tr>';
        }

        $html .= '</table>';

        return $html;
    }

    /**
     * Определяет категорию получателя.
     *
     * @param string $merchant
     * @return string
     */
    private function getMerchantCategory(string $merchant): string
    {
        $merchantLower = mb_strtolower($merchant);

        return match (true) {
            str_contains($merchantLower, 'market'),
            str_contains($merchantLower, 'shop'),
            str_contains($merchantLower, 'store') => 'Покупки',

            str_contains($merchantLower, 'cafe'),
            str_contains($merchantLower, 'pizza'),
            str_contains($merchantLower, 'restaurant') => 'Еда',

            str_contains($merchantLower, 'bank') => 'Финансы',

            str_contains($merchantLower, 'fuel'),
            str_contains($merchantLower, 'gas'),
            str_contains($merchantLower, 'transport') => 'Транспорт',

            str_contains($merchantLower, 'education'),
            str_contains($merchantLower, 'course') => 'Образование',

            default => 'Другое',
        };
    }
}

/*
|--------------------------------------------------------------------------
| Создание начальных данных
|--------------------------------------------------------------------------
*/

$repository = new TransactionRepository();

$transactionList = [
    new Transaction(1, '2025-03-01', 1250.50, 'Покупка продуктов', 'Green Market'),
    new Transaction(2, '2025-03-03', 320.00, 'Кофе и десерт', 'Cafe Aroma'),
    new Transaction(3, '2025-03-05', 7800.00, 'Покупка наушников', 'Tech Store'),
    new Transaction(4, '2025-03-07', 150.75, 'Поездка на такси', 'City Transport'),
    new Transaction(5, '2025-03-09', 2100.00, 'Оплата коммунальных услуг', 'Local Bank'),
    new Transaction(6, '2025-03-11', 950.20, 'Ужин с друзьями', 'Pizza House'),
    new Transaction(7, '2025-03-13', 430.00, 'Заправка автомобиля', 'Fuel Station'),
    new Transaction(8, '2025-03-15', 1899.99, 'Покупка одежды', 'Fashion Shop'),
    new Transaction(9, '2025-03-17', 250.00, 'Обед', 'Cafe Aroma'),
    new Transaction(10, '2025-03-19', 5600.00, 'Оплата курса обучения', 'Education Center'),
];

foreach ($transactionList as $transaction) {
    $repository->addTransaction($transaction);
}

$manager = new TransactionManager($repository);
$renderer = new TransactionTableRenderer();

/*
|--------------------------------------------------------------------------
| Демонстрация поиска
|--------------------------------------------------------------------------
*/

$searchedTransaction = $repository->findById(3);

/*
|--------------------------------------------------------------------------
| Демонстрация вычислений
|--------------------------------------------------------------------------
*/

$totalAmount = $manager->calculateTotalAmount();
$totalInRange = $manager->calculateTotalAmountByDateRange('2025-03-05', '2025-03-15');
$merchantTransactionCount = $manager->countTransactionsByMerchant('Cafe Aroma');

/*
|--------------------------------------------------------------------------
| Демонстрация удаления
|--------------------------------------------------------------------------
*/

$transactionBeforeDelete = $repository->findById(5);
$repository->removeTransactionById(5);
$transactionAfterDelete = $repository->findById(5);

/*
|--------------------------------------------------------------------------
| Данные после удаления
|--------------------------------------------------------------------------
*/

$allTransactionsAfterDelete = $repository->getAllTransactions();
$sortedByDate = $manager->sortTransactionsByDate();
$sortedByAmountDesc = $manager->sortTransactionsByAmountDesc();

?>
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Лабораторная работа №5</title>
</head>
<body style="font-family: Arial, sans-serif; margin: 20px;">
    <h1>Лабораторная работа №5</h1>
    <h2>Управление банковскими транзакциями</h2>

    <h3>1. Поиск транзакции по ID</h3>
    <?php if ($searchedTransaction !== null): ?>
        <p>
            Найдена транзакция:
            ID <?= htmlspecialchars((string)$searchedTransaction->getId()) ?>,
            дата <?= htmlspecialchars($searchedTransaction->getDate()) ?>,
            сумма <?= htmlspecialchars(number_format($searchedTransaction->getAmount(), 2, '.', '')) ?>,
            описание <?= htmlspecialchars($searchedTransaction->getDescription()) ?>,
            получатель <?= htmlspecialchars($searchedTransaction->getMerchant()) ?>.
        </p>
    <?php else: ?>
        <p>Транзакция не найдена.</p>
    <?php endif; ?>

    <h3>2. Вычисления по коллекции транзакций</h3>
    <p><strong>Общая сумма всех транзакций:</strong> <?= htmlspecialchars(number_format($totalAmount, 2, '.', '')) ?></p>
    <p><strong>Сумма транзакций за период с 2025-03-05 по 2025-03-15:</strong> <?= htmlspecialchars(number_format($totalInRange, 2, '.', '')) ?></p>
    <p><strong>Количество транзакций для получателя Cafe Aroma:</strong> <?= htmlspecialchars((string)$merchantTransactionCount) ?></p>

    <h3>3. Демонстрация удаления транзакции</h3>
    <p><strong>До удаления транзакция с ID 5:</strong> <?= $transactionBeforeDelete !== null ? 'найдена' : 'не найдена' ?></p>
    <p><strong>После удаления транзакция с ID 5:</strong> <?= $transactionAfterDelete !== null ? 'найдена' : 'не найдена' ?></p>

    <h3>4. Все транзакции после удаления</h3>
    <?= $renderer->render($allTransactionsAfterDelete) ?>

    <h3>5. Транзакции, отсортированные по дате</h3>
    <?= $renderer->render($sortedByDate) ?>

    <h3>6. Транзакции, отсортированные по сумме по убыванию</h3>
    <?= $renderer->render($sortedByAmountDesc) ?>
</body>
</html>
```
Ответы на контрольные вопросы:

1. Зачем нужна строгая типизация в PHP и как она помогает при разработке?
   
Строгая типизация в PHP позволяет явно контролировать типы данных, используемых в программе, и предотвращает ошибки, связанные с их некорректным использованием.
Она делает код более надежным, понятным и предсказуемым. Благодаря этому разработчик быстрее находит и исправляет ошибки.

2. Что такое класс в объектно-ориентированном программировании и какие основные компоненты класса вы знаете?
   
Класс в объектно-ориентированном программировании — это шаблон для создания объектов, который объединяет данные и методы для работы с ними.
Он состоит из свойств, методов, конструктора и модификаторов доступа. Классы позволяют структурировать код и упрощают его повторное использование.

3. Объясните, что такое полиморфизм и как он может быть реализован в PHP.
   
Полиморфизм — это возможность использовать один интерфейс для работы с разными типами объектов.
В PHP он реализуется с помощью интерфейсов, наследования и абстрактных классов. Это позволяет писать более гибкий и расширяемый код.

4. Что такое интерфейс в PHP и как он отличается от абстрактного класса?
   
Интерфейс в PHP задаёт набор методов, которые класс обязан реализовать, но не содержит их реализации.
В отличие от него, абстрактный класс может включать как абстрактные, так и уже реализованные методы.
Интерфейсы позволяют задать общий контракт для разных классов.

5. Какие преимущества дает использование интерфейсов при проектировании архитектуры приложения? Объясните на примере данной лабораторной работы.

Использование интерфейсов делает архитектуру приложения более гибкой и независимой от конкретных реализаций. 
Это позволяет легко заменять одни компоненты другими без изменения остального кода.
В данной работе это видно на примере замены хранилища транзакций без изменения логики менеджера.
