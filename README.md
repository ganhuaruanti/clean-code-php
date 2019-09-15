# Clean Code PHP

## 目錄

  1. [介紹](#介紹)
  2. [變數](#變數)
     * [使用容易理解的變數名](#使用容易理解的變數名)
     * [同一個實體要用相同的變數名](#同一個實體要用相同的變數名)
     * [使用便於搜索的名稱 (part 1)](#使用便於搜索的名稱-part-1)
     * [使用便於搜索的名稱 (part 2)](#使用便於搜索的名稱-part-2)
     * [使用自解釋型變數](#使用自解釋型變數)
     * [避免深層嵌套，盡早返回 (part 1)](#避免深層嵌套盡早返回-part-1)
     * [避免深層嵌套，盡早返回 (part 2)](#避免深層嵌套盡早返回-part-2)
     * [少用無意義的變數名](#少用無意義的變數名)
     * [不要添加不必要上下文](#不要添加不必要上下文)
     * [合理使用參數默認值，沒必要在方法裡再做默認值檢測](#合理使用參數默認值沒必要在方法裡再做默認值檢測)
  3. [表達式](#表達式)
     * [使用恆等式](#使用恆等式)
  4. [函式](#函式)
     * [函式參數（最好少於2個）](#函式參數-最好少於2個)
     * [函式應該只做一件事](#函式應該只做一件事)
     * [函式名應體現他做了什麼事](#函式名應體現他做了什麼事)
     * [函式裡應當只有一層抽像abstraction](#函式裡應當只有一層抽像abstraction)
     * [不要用flag作為函式的參數](#不要用flag作為函式的參數)
     * [避免副作用](#避免副作用)
     * [不要寫全局函式](#不要寫全局函式)
     * [不要使用單例模式](#不要使用單例模式)
     * [封裝條件語句](#封裝條件語句)
     * [避免用反義條件判斷](#避免用反義條件判斷)
     * [避免條件判斷](#避免條件判斷)
     * [避免類型檢查 (part 1)](#避免類型檢查-part-1)
     * [避免類型檢查 (part 2)](#避免類型檢查-part-2)
     * [移除僵屍代碼](#移除僵屍代碼)
  5. [對像和數據結構 Objects and Data Structures](#對像和數據結構)
     * [使用 getters 和 setters Use object encapsulation](#使用-getters-和-setters)
     * [給對像使用私有或受保護的成員變數](#給對像使用私有或受保護的成員變數)
  6. [類](#類)
     * [少用繼承多用組合](#少用繼承多用組合)
     * [避免連貫接口](#避免連貫接口)
     * [推薦使用 final 類](#推薦使用-final-類)
  7. [類的SOLID原則 SOLID](#solid)
     * [S: 單一職責原則 Single Responsibility Principle (SRP)](#單一職責原則)
     * [O: 開閉原則 Open/Closed Principle (OCP)](#開閉原則)
     * [L: 里氏替換原則 Liskov Substitution Principle (LSP)](#里氏替換原則)
     * [I: 接口隔離原則 Interface Segregation Principle (ISP)](#接口隔離原則)
     * [D: 依賴倒置原則 Dependency Inversion Principle (DIP)](#依賴倒置原則)
  8. [別寫重複代碼 (DRY)](#別寫重複代碼-dry)
  9. [翻譯](#翻譯)

## 介紹


本文參考自 Robert C. Martin的[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)  書中的軟件工程師的原則
,適用於PHP。 這不是風格指南。 這是一個關於開發可讀、可復用並且可重構的PHP軟件指南。

並不是這裡所有的原則都得遵循，甚至很少的能被普遍接受。 這些雖然只是指導，但是都是*Clean Code*作者多年總結出來的。

本文受到 [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript) 的啟發

雖然很多開發者還在使用PHP5，但是本文中的大部分示例的運行環境需要PHP 7.1+。

## 翻譯說明

從 [php-cpm 版本](https://github.com/php-cpm/clean-code-php)的[clean-code-php](https://github.com/jupeter/clean-code-php) 簡繁轉換過後，將用詞修正成更符合繁體中文的用法。

閱讀過程中，如果遇到各種連接失效、內容老舊、術語使用錯誤和其他翻譯錯誤等問題，歡迎大家積極提交 PR。

## **變數**

### 使用容易理解的變數名

**壞：**

```php
$ymdstr = $moment->format('y-m-d');
```

**好：**

```php
$currentDate = $moment->format('y-m-d');
```

**[⬆ 返回頂部](#目錄)**

### 同一個實體要用相同的變數名

**壞：**

```php
getUserInfo();
getUserData();
getUserRecord();
getUserProfile();
```

**好：**

```php
getUser();
```

**[⬆ 返回頂部](#目錄)**

### 使用便於搜索的名稱 (part 1)

寫代碼是用來讀的。所以寫出可讀性高、便於搜索的代碼至關重要。
命名變數時如果沒有有意義、不好理解，那就是在傷害讀者。
請讓你的代碼便於搜索。

**壞：**
```php
// 448 ™ 干啥的?
$result = $serializer->serialize($data, 448);
```

**好：**

```php
$json = $serializer->serialize($data, JSON_UNESCAPED_SLASHES | JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);
```

### 使用便於搜索的名稱 (part 2)

**壞：**

```php
class User
{
    // 7 ™ 干啥的?
    public $access = 7;
}

// 4 ™ 干啥的?
if ($user->access & 4) {
    // ...
}

// 這裡會發生什麼?
$user->access ^= 2;
```

**好：**

```php
class User
{
    const ACCESS_READ = 1;
    const ACCESS_CREATE = 2;
    const ACCESS_UPDATE = 4;
    const ACCESS_DELETE = 8;

    // 默認情況下用戶 具有讀、寫和更新權限
    public $access = self::ACCESS_READ | self::ACCESS_CREATE | self::ACCESS_UPDATE;
}

if ($user->access & User::ACCESS_UPDATE) {
    // do edit ...
}

// 禁用創建權限
$user->access ^= User::ACCESS_CREATE;
```

**[⬆ 返回頂部](#目錄)**

### 使用自解釋型變數

**壞：**

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(.+?)\s*(\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

saveCityZipCode($matches[1], $matches[2]);
```

**不錯：**

好一些，但強依賴於正則表達式的熟悉程度

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(.+?)\s*(\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

[, $city, $zipCode] = $matches;
saveCityZipCode($city, $zipCode);
```

**好：**

使用帶名字的子規則，不用懂正則也能看的懂

```php
$address = 'One Infinite Loop, Cupertino 95014';
$cityZipCodeRegex = '/^[^,]+,\s*(?<city>.+?)\s*(?<zipCode>\d{5})$/';
preg_match($cityZipCodeRegex, $address, $matches);

saveCityZipCode($matches['city'], $matches['zipCode']);
```

**[⬆ 返回頂部](#目錄)**

### 避免深層嵌套，盡早返回 (part 1)

太多的if else語句通常會導致你的代碼難以閱讀，直白優於隱晦

**糟糕：**

```php
function isShopOpen($day): bool
{
    if ($day) {
        if (is_string($day)) {
            $day = strtolower($day);
            if ($day === 'friday') {
                return true;
            } elseif ($day === 'saturday') {
                return true;
            } elseif ($day === 'sunday') {
                return true;
            } else {
                return false;
            }
        } else {
            return false;
        }
    } else {
        return false;
    }
}
```

**好：**

```php
function isShopOpen(string $day): bool
{
    if (empty($day)) {
        return false;
    }

    $openingDays = [
        'friday', 'saturday', 'sunday'
    ];

    return in_array(strtolower($day), $openingDays, true);
}
```

**[⬆ 返回頂部](#目錄)**

### 避免深層嵌套，盡早返回 (part 2)

**糟糕的：**

```php
function fibonacci(int $n)
{
    if ($n < 50) {
        if ($n !== 0) {
            if ($n !== 1) {
                return fibonacci($n - 1) + fibonacci($n - 2);
            } else {
                return 1;
            }
        } else {
            return 0;
        }
    } else {
        return 'Not supported';
    }
}
```

**好：**

```php
function fibonacci(int $n): int
{
    if ($n === 0 || $n === 1) {
        return $n;
    }

    if ($n >= 50) {
        throw new \Exception('Not supported');
    }

    return fibonacci($n - 1) + fibonacci($n - 2);
}
```

**[⬆ 返回頂部](#目錄)**

### 少用無意義的變數名

別讓讀你的代碼的人猜你寫的變數是什麼意思。
寫清楚好過模糊不清。

**壞：**

```php
$l = ['Austin', 'New York', 'San Francisco'];

for ($i = 0; $i < count($l); $i++) {
    $li = $l[$i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
  // 等等, `$li` 又代表什麼?
    dispatch($li);
}
```

**好：**

```php
$locations = ['Austin', 'New York', 'San Francisco'];

foreach ($locations as $location) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch($location);
}
```

**[⬆ 返回頂部](#目錄)**

### 不要添加不必要上下文

如果從你的類名、對像名已經可以得知一些信息，就別再在變數名裡重複。

**壞：**

```php
class Car
{
    public $carMake;
    public $carModel;
    public $carColor;

    //...
}
```

**好：**

```php
class Car
{
    public $make;
    public $model;
    public $color;

    //...
}
```

**[⬆ 返回頂部](#目錄)**

### 合理使用參數默認值，沒必要在方法裡再做默認值檢測

**不好：**

不好，`$breweryName` 可能為 `NULL`.

```php
function createMicrobrewery($breweryName = 'Hipster Brew Co.'): void
{
    // ...
}
```

**還行：**

比上一個好理解一些，但最好能控制變數的值

```php
function createMicrobrewery($name = null): void
{
    $breweryName = $name ?: 'Hipster Brew Co.';
    // ...
}
```

**好：**

如果你的程序只支持 PHP 7+, 那你可以用 [type hinting](http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration) 保證變數 `$breweryName` 不是 `NULL`.

```php
function createMicrobrewery(string $breweryName = 'Hipster Brew Co.'): void
{
    // ...
}
```

**[⬆ 返回頂部](#目錄)**

## 表達式

### [使用恆等式](http://php.net/manual/en/language.operators.comparison.php)

**不好：**

簡易對比會將字符串轉為整形

```php
$a = '42';
$b = 42;

if( $a != $b ) {
   //這裡始終執行不到
}
```

對比 $a != $b 返回了 `FALSE` 但應該返回 `TRUE` !
字符串 '42' 跟整數 42 不相等

**好：**

使用恆等判斷檢查類型和數據

```php
$a = '42';
$b = 42;

if ($a !== $b) {
    // The expression is verified
}
```

The comparison `$a !== $b` returns `TRUE`.

**[⬆ 返回頂部](#目錄)**

## 函式

### 函式參數（最好少於2個）

限制函式參數個數極其重要，這樣測試你的函式容易點。有超過3個可選參數參數導致一個爆炸式組合增長，你會有成噸獨立參數情形要測試。

無參數是理想情況。1個或2個都可以，最好避免3個。再多就需要加固了。通常如果你的函式有超過兩個參數，說明他要處理的事太多了。 如果必須要傳入很多數據，建議封裝一個高級別對像作為參數。

**壞：**

```php
function createMenu(string $title, string $body, string $buttonText, bool $cancellable): void
{
    // ...
}
```

**好：**

```php
class MenuConfig
{
    public $title;
    public $body;
    public $buttonText;
    public $cancellable = false;
}

$config = new MenuConfig();
$config->title = 'Foo';
$config->body = 'Bar';
$config->buttonText = 'Baz';
$config->cancellable = true;

function createMenu(MenuConfig $config): void
{
    // ...
}
```

**[⬆ 返回頂部](#目錄)**

### 函式應該只做一件事

這是迄今為止軟件工程裡最重要的一個規則。當一個函式做超過一件事的時候，他們就難於實現、測試和理解。當你把一個函式拆分到只剩一個功能時，他們就容易被重構，然後你的代碼讀起來就更清晰。如果你光遵循這條規則，你就領先於大多數開發者了。

**壞：**

```php
function emailClients(array $clients): void
{
    foreach ($clients as $client) {
        $clientRecord = $db->find($client);
        if ($clientRecord->isActive()) {
            email($client);
        }
    }
}
```

**好：**

```php
function emailClients(array $clients): void
{
    $activeClients = activeClients($clients);
    array_walk($activeClients, 'email');
}

function activeClients(array $clients): array
{
    return array_filter($clients, 'isClientActive');
}

function isClientActive(int $client): bool
{
    $clientRecord = $db->find($client);

    return $clientRecord->isActive();
}
```

**[⬆ 返回頂部](#目錄)**

### 函式名應體現他做了什麼事

**壞：**

```php
class Email
{
    //...

    public function handle(): void
    {
        mail($this->to, $this->subject, $this->body);
    }
}

$message = new Email(...);
// 啥？handle處理一個消息干嘛了？是往一個文件裡寫嗎？
$message->handle();
```

**好：**

```php
class Email 
{
    //...

    public function send(): void
    {
        mail($this->to, $this->subject, $this->body);
    }
}

$message = new Email(...);
// 簡單明了
$message->send();
```

**[⬆ 返回頂部](#目錄)**

### 函式裡應當只有一層抽像abstraction

當你抽像層次過多時時，函式處理的事情太多了。需要拆分功能來提高可重用性和易用性，以便簡化測試。
（譯者注：這裡從示例代碼看應該是指嵌套過多）

**壞：**

```php
function parseBetterJSAlternative(string $code): void
{
    $regexes = [
        // ...
    ];

    $statements = explode(' ', $code);
    $tokens = [];
    foreach ($regexes as $regex) {
        foreach ($statements as $statement) {
            // ...
        }
    }

    $ast = [];
    foreach ($tokens as $token) {
        // lex...
    }

    foreach ($ast as $node) {
        // parse...
    }
}
```

**壞：**

我們把一些方法從循環中提取出來，但是`parseBetterJSAlternative()`方法還是很復雜，而且不利於測試。

```php
function tokenize(string $code): array
{
    $regexes = [
        // ...
    ];

    $statements = explode(' ', $code);
    $tokens = [];
    foreach ($regexes as $regex) {
        foreach ($statements as $statement) {
            $tokens[] = /* ... */;
        }
    }

    return $tokens;
}

function lexer(array $tokens): array
{
    $ast = [];
    foreach ($tokens as $token) {
        $ast[] = /* ... */;
    }

    return $ast;
}

function parseBetterJSAlternative(string $code): void
{
    $tokens = tokenize($code);
    $ast = lexer($tokens);
    foreach ($ast as $node) {
        // 解析邏輯...
    }
}
```

**好：**

最好的解決方案是把 `parseBetterJSAlternative()`方法的依賴移除。

```php
class Tokenizer
{
    public function tokenize(string $code): array
    {
        $regexes = [
            // ...
        ];

        $statements = explode(' ', $code);
        $tokens = [];
        foreach ($regexes as $regex) {
            foreach ($statements as $statement) {
                $tokens[] = /* ... */;
            }
        }

        return $tokens;
    }
}

class Lexer
{
    public function lexify(array $tokens): array
    {
        $ast = [];
        foreach ($tokens as $token) {
            $ast[] = /* ... */;
        }

        return $ast;
    }
}

class BetterJSAlternative
{
    private $tokenizer;
    private $lexer;

    public function __construct(Tokenizer $tokenizer, Lexer $lexer)
    {
        $this->tokenizer = $tokenizer;
        $this->lexer = $lexer;
    }

    public function parse(string $code): void
    {
        $tokens = $this->tokenizer->tokenize($code);
        $ast = $this->lexer->lexify($tokens);
        foreach ($ast as $node) {
            // 解析邏輯...
        }
    }
}
```

這樣我們可以對依賴做mock，並測試`BetterJSAlternative::parse()`運行是否符合預期。

**[⬆ 返回頂部](#目錄)**

### 不要用flag作為函式的參數

flag就是在告訴大家，這個方法裡處理很多事。前面剛說過，一個函式應當只做一件事。 把不同flag的代碼拆分到多個函式裡。

**壞：**
```php
function createFile(string $name, bool $temp = false): void
{
    if ($temp) {
        touch('./temp/'.$name);
    } else {
        touch($name);
    }
}
```

**好：**

```php
function createFile(string $name): void
{
    touch($name);
}

function createTempFile(string $name): void
{
    touch('./temp/'.$name);
}
```
**[⬆ 返回頂部](#目錄)**

### 避免副作用

一個函式做了比獲取一個值然後返回另外一個值或值們會產生副作用如果。副作用可能是寫入一個文件，修改某些全局變數或者偶然的把你全部的錢給了陌生人。

現在，你的確需要在一個程序或者場合裡要有副作用，像之前的例子，你也許需要寫一個文件。你想要做的是把你做這些的地方集中起來。不要用幾個函式和類來寫入一個特定的文件。用一個服務來做它，一個只有一個。

重點是避免常見陷阱比如對像間共享無結構的數據，使用可以寫入任何的可變數據類型，不集中處理副作用發生的地方。如果你做了這些你就會比大多數程序員快樂。

**壞：**

```php
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
$name = 'Ryan McDermott';

function splitIntoFirstAndLastName(): void
{
    global $name;

    $name = explode(' ', $name);
}

splitIntoFirstAndLastName();

var_dump($name); // ['Ryan', 'McDermott'];
```

**好：**

```php
function splitIntoFirstAndLastName(string $name): array
{
    return explode(' ', $name);
}

$name = 'Ryan McDermott';
$newName = splitIntoFirstAndLastName($name);

var_dump($name); // 'Ryan McDermott';
var_dump($newName); // ['Ryan', 'McDermott'];
```

**[⬆ 返回頂部](#目錄)**

### 不要寫全局函式
在大多數語言中污染全局變數是一個壞的實踐，因為你可能和其他類庫衝突
並且調用你api的人直到他們捕獲異常才知道踩坑了。讓我們思考一種場景：
如果你想配置一個數組，你可能會寫一個全局函式`config()`，但是他可能
和試著做同樣事的其他類庫衝突。

**壞：**

```php
function config(): array
{
    return  [
        'foo' => 'bar',
    ]
}
```

**好：**

```php
class Configuration
{
    private $configuration = [];

    public function __construct(array $configuration)
    {
        $this->configuration = $configuration;
    }

    public function get(string $key): ?string
    {
        return isset($this->configuration[$key]) ? $this->configuration[$key] : null;
    }
}
```

加載配置並創建 `Configuration` 類的實例

```php
$configuration = new Configuration([
    'foo' => 'bar',
]);
```

現在你必須在程序中用 `Configuration` 的實例了

**[⬆ 返回頂部](#目錄)**

### 不要使用單例模式

單例是一種 [反模式](https://en.wikipedia.org/wiki/Singleton_pattern).  以下是解釋：Paraphrased from Brian Button:
 1. 總是被用成全局實例。They are generally used as a **global instance**, why is that so bad? Because **you hide the dependencies** of your application in your code, instead of exposing them through the interfaces. Making something global to avoid passing it around is a [code smell](https://en.wikipedia.org/wiki/Code_smell).
 2. 違反了[單一響應原則]()They violate the [single responsibility principle](#single-responsibility-principle-srp): by virtue of the fact that **they control their own creation and lifecycle**.
 3. 導致代碼強耦合They inherently cause code to be tightly [coupled](https://en.wikipedia.org/wiki/Coupling_%28computer_programming%29). This makes faking them out under **test rather difficult** in many cases.
 4. 在整個程序的生命周期中始終攜帶狀態。They carry state around for the lifetime of the application. Another hit to testing since **you can end up with a situation where tests need to be ordered** which is a big no for unit tests. Why? Because each unit test should be independent from the other.

這裡有一篇非常好的討論單例模式的[根本問題((http://misko.hevery.com/2008/08/25/root-cause-of-singletons/)的文章，是[Misko Hevery](http://misko.hevery.com/about/) 寫的。

**壞：**

```php
class DBConnection
{
    private static $instance;

    private function __construct(string $dsn)
    {
        // ...
    }

    public static function getInstance(): DBConnection
    {
        if (self::$instance === null) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    // ...
}

$singleton = DBConnection::getInstance();
```

**好：**

```php
class DBConnection
{
    public function __construct(string $dsn)
    {
        // ...
    }

     // ...
}
```

創建 `DBConnection` 類的實例並通過 [DSN](http://php.net/manual/en/pdo.construct.php#refsect1-pdo.construct-parameters) 配置.

```php
$connection = new DBConnection($dsn);
```

現在你必須在程序中 使用 `DBConnection` 的實例了

**[⬆ 返回頂部](#目錄)**

### 封裝條件語句

**壞：**

```php
if ($article->state === 'published') {
    // ...
}
```

**好：**

```php
if ($article->isPublished()) {
    // ...
}
```

**[⬆ 返回頂部](#目錄)**

### 避免用反義條件判斷

**壞：**

```php
function isDOMNodeNotPresent(\DOMNode $node): bool
{
    // ...
}

if (!isDOMNodeNotPresent($node))
{
    // ...
}
```

**好：**

```php
function isDOMNodePresent(\DOMNode $node): bool
{
    // ...
}

if (isDOMNodePresent($node)) {
    // ...
}
```

**[⬆ 返回頂部](#目錄)**

### 避免條件判斷

這看起來像一個不可能任務。當人們第一次聽到這句話是都會這麼說。
"沒有`if語句`我還能做啥？" 答案是你可以使用多態來實現多種場景
的相同任務。第二個問題很常見， “這麼做可以，但為什麼我要這麼做？”
 答案是前面我們學過的一個Clean Code原則：一個函式應當只做一件事。
 當你有很多含有`if`語句的類和函式時,你的函式做了不止一件事。
 記住，只做一件事。

**壞：**

```php
class Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        switch ($this->type) {
            case '777':
                return $this->getMaxAltitude() - $this->getPassengerCount();
            case 'Air Force One':
                return $this->getMaxAltitude();
            case 'Cessna':
                return $this->getMaxAltitude() - $this->getFuelExpenditure();
        }
    }
}
```

**好：**

```php
interface Airplane
{
    // ...

    public function getCruisingAltitude(): int;
}

class Boeing777 implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude() - $this->getPassengerCount();
    }
}

class AirForceOne implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude();
    }
}

class Cessna implements Airplane
{
    // ...

    public function getCruisingAltitude(): int
    {
        return $this->getMaxAltitude() - $this->getFuelExpenditure();
    }
}
```

**[⬆ 返回頂部](#目錄)**

### 避免類型檢查 (part 1)

PHP是弱類型的,這意味著你的函式可以接收任何類型的參數。
有時候你為這自由所痛苦並且在你的函式漸漸嘗試類型檢查。
有很多方法去避免這麼做。第一種是統一API。

**壞：**

```php
function travelToTexas($vehicle): void
{
    if ($vehicle instanceof Bicycle) {
        $vehicle->pedalTo(new Location('texas'));
    } elseif ($vehicle instanceof Car) {
        $vehicle->driveTo(new Location('texas'));
    }
}
```

**好：**

```php
function travelToTexas(Vehicle $vehicle): void
{
    $vehicle->travelTo(new Location('texas'));
}
```

**[⬆ 返回頂部](#目錄)**

### 避免類型檢查 (part 2)

如果你正使用基本原始值比如字符串、整形和數組，要求版本是PHP 7+，不用多態，需要類型檢測，
那你應當考慮[類型聲明](http://php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration)或者嚴格模式。
提供了基於標准PHP語法的靜態類型。 手動檢查類型的問題是做好了需要好多的廢話，好像為了安全就可以不顧損失可讀性。
保持你的PHP 整潔，寫好測試，做好代碼回顧。做不到就用PHP嚴格類型聲明和嚴格模式來確保安全。

**壞：**

```php
function combine($val1, $val2): int
{
    if (!is_numeric($val1) || !is_numeric($val2)) {
        throw new \Exception('Must be of type Number');
    }

    return $val1 + $val2;
}
```

**好：**

```php
function combine(int $val1, int $val2): int
{
    return $val1 + $val2;
}
```

**[⬆ 返回頂部](#目錄)**

### 移除僵屍代碼

僵屍代碼和重複代碼一樣壞。沒有理由保留在你的代碼庫中。如果從來沒被使用過，就刪掉！
因為還在代碼版本庫裡，因此很安全。

**壞：**
```php
function oldRequestModule(string $url): void
{
    // ...
}

function newRequestModule(string $url): void
{
    // ...
}

$request = newRequestModule($requestUrl);
inventoryTracker('apples', $request, 'www.inventory-awesome.io');
```

**好：**

```php
function requestModule(string $url): void
{
    // ...
}

$request = requestModule($requestUrl);
inventoryTracker('apples', $request, 'www.inventory-awesome.io');
```

**[⬆ 返回頂部](#目錄)**


## 對像和數據結構

### 使用 getters 和 setters

在PHP中你可以對方法使用`public`, `protected`, `private` 來控制對像屬性的變更。

* 當你想對對像屬性做獲取之外的操作時，你不需要在代碼中去尋找並修改每一個該屬性訪問方法
* 當有`set`對應的屬性方法時，易於增加參數的驗證
* 封裝內部的表示
* 使用set*和get*時，易於增加日志和錯誤控制
* 繼承當前類時，可以復寫默認的方法功能
* 當對像屬性是從遠端服務器獲取時，get*，set*易於使用延遲加載

此外，這樣的方式也符合OOP開發中的[開閉原則](#開閉原則)

**壞：**

```php
class BankAccount
{
    public $balance = 1000;
}

$bankAccount = new BankAccount();

// Buy shoes...
$bankAccount->balance -= 100;
```

**好：**

```php
class BankAccount
{
    private $balance;

    public function __construct(int $balance = 1000)
    {
      $this->balance = $balance;
    }

    public function withdraw(int $amount): void
    {
        if ($amount > $this->balance) {
            throw new \Exception('Amount greater than available balance.');
        }

        $this->balance -= $amount;
    }

    public function deposit(int $amount): void
    {
        $this->balance += $amount;
    }

    public function getBalance(): int
    {
        return $this->balance;
    }
}

$bankAccount = new BankAccount();

// Buy shoes...
$bankAccount->withdraw($shoesPrice);

// Get balance
$balance = $bankAccount->getBalance();
```

**[⬆ 返回頂部](#目錄)**

### 給對像使用私有或受保護的成員變數

* 對`public`方法和屬性進行修改非常危險，因為外部代碼容易依賴他，而你沒辦法控制。**對之修改影響所有這個類的使用者。** `public` methods and properties are most dangerous for changes, because some outside code may easily rely on them and you can't control what code relies on them. **Modifications in class are dangerous for all users of class.**
* 對`protected`的修改跟對`public`修改差不多危險，因為他們對子類可用，他倆的唯一區別就是可調用的位置不一樣，**對之修改影響所有集成這個類的地方。**  `protected` modifier are as dangerous as public, because they are available in scope of any child class. This effectively means that difference between public and protected is only in access mechanism, but encapsulation guarantee remains the same. **Modifications in class are dangerous for all descendant classes.**
* 對`private`的修改保證了這部分代碼**只會影響當前類**`private` modifier guarantees that code is **dangerous to modify only in boundaries of single class** (you are safe for modifications and you won't have [Jenga effect](http://www.urbandictionary.com/define.php?term=Jengaphobia&defid=2494196)).

所以，當你需要控制類裡的代碼可以被訪問時才用`public/protected`，其他時候都用`private`。

可以讀一讀這篇 [博客文章](http://fabien.potencier.org/pragmatism-over-theory-protected-vs-private.html) ，[Fabien Potencier](https://github.com/fabpot)寫的.

**壞：**

```php
class Employee
{
    public $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }
}

$employee = new Employee('John Doe');
echo 'Employee name: '.$employee->name; // Employee name: John Doe
```

**好：**

```php
class Employee
{
    private $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function getName(): string
    {
        return $this->name;
    }
}

$employee = new Employee('John Doe');
echo 'Employee name: '.$employee->getName(); // Employee name: John Doe
```

**[⬆ 返回頂部](#目錄)**

## 類

### 少用繼承多用組合

正如  the Gang of Four 所著的[*設計模式*](https://en.wikipedia.org/wiki/Design_Patterns)之前所說，
我們應該盡量優先選擇組合而不是繼承的方式。使用繼承和組合都有很多好處。
這個准則的主要意義在於當你本能的使用繼承時，試著思考一下`組合`是否能更好對你的需求建模。
在一些情況下，是這樣的。

接下來你或許會想，“那我應該在什麼時候使用繼承？” 
答案依賴於你的問題，當然下面有一些何時繼承比組合更好的說明：

1. 你的繼承表達了“是一個”而不是“有一個”的關系（人類-》動物，用戶-》用戶詳情）
2. 你可以復用基類的代碼（人類可以像動物一樣移動）
3. 你想通過修改基類對所有派生類做全局的修改（當動物移動時，修改她們的能量消耗）

**糟糕的：**

```php
class Employee 
{
    private $name;
    private $email;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    // ...
}


// 不好，因為 Employees "有" taxdata
// 而 EmployeeTaxData 不是 Employee 類型的


class EmployeeTaxData extends Employee 
{
    private $ssn;
    private $salary;
    
    public function __construct(string $name, string $email, string $ssn, string $salary)
    {
        parent::__construct($name, $email);

        $this->ssn = $ssn;
        $this->salary = $salary;
    }

    // ...
}
```

**好：**

```php
class EmployeeTaxData 
{
    private $ssn;
    private $salary;

    public function __construct(string $ssn, string $salary)
    {
        $this->ssn = $ssn;
        $this->salary = $salary;
    }

    // ...
}

class Employee 
{
    private $name;
    private $email;
    private $taxData;

    public function __construct(string $name, string $email)
    {
        $this->name = $name;
        $this->email = $email;
    }

    public function setTaxData(string $ssn, string $salary)
    {
        $this->taxData = new EmployeeTaxData($ssn, $salary);
    }

    // ...
}
```

**[⬆ 返回頂部](#目錄)**

### 避免連貫接口

[連貫接口Fluent interface](https://en.wikipedia.org/wiki/Fluent_interface)是一種
旨在提高面向對像編程時代碼可讀性的API設計模式，他基於[方法鏈Method chaining](https://en.wikipedia.org/wiki/Method_chaining)

有上下文的地方可以降低代碼復雜度，例如[PHPUnit Mock Builder](https://phpunit.de/manual/current/en/test-doubles.html)
和[Doctrine Query Builder](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/query-builder.html)
，更多的情況會帶來較大代價：

While there can be some contexts, frequently builder objects, where this
pattern reduces the verbosity of the code (for example the [PHPUnit Mock Builder](https://phpunit.de/manual/current/en/test-doubles.html)
or the [Doctrine Query Builder](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/query-builder.html)),
more often it comes at some costs:

1. 破壞了 [對像封裝](https://en.wikipedia.org/wiki/Encapsulation_%28object-oriented_programming%29)
2. 破壞了 [裝飾器模式](https://en.wikipedia.org/wiki/Decorator_pattern)
3. 在測試組件中不好做[mock](https://en.wikipedia.org/wiki/Mock_object)
4. 導致提交的diff不好閱讀

了解更多請閱讀 [連貫接口為什麼不好](https://ocramius.github.io/blog/fluent-interfaces-are-evil/)
，作者 [Marco Pivetta](https://github.com/Ocramius).

**壞：**

```php
class Car
{
    private $make = 'Honda';
    private $model = 'Accord';
    private $color = 'white';

    public function setMake(string $make): self
    {
        $this->make = $make;

        // NOTE: Returning this for chaining
        return $this;
    }

    public function setModel(string $model): self
    {
        $this->model = $model;

        // NOTE: Returning this for chaining
        return $this;
    }

    public function setColor(string $color): self
    {
        $this->color = $color;

        // NOTE: Returning this for chaining
        return $this;
    }

    public function dump(): void
    {
        var_dump($this->make, $this->model, $this->color);
    }
}

$car = (new Car())
  ->setColor('pink')
  ->setMake('Ford')
  ->setModel('F-150')
  ->dump();
```

**好：**

```php
class Car
{
    private $make = 'Honda';
    private $model = 'Accord';
    private $color = 'white';

    public function setMake(string $make): void
    {
        $this->make = $make;
    }

    public function setModel(string $model): void
    {
        $this->model = $model;
    }

    public function setColor(string $color): void
    {
        $this->color = $color;
    }

    public function dump(): void
    {
        var_dump($this->make, $this->model, $this->color);
    }
}

$car = new Car();
$car->setColor('pink');
$car->setMake('Ford');
$car->setModel('F-150');
$car->dump();
```

**[⬆ 返回頂部](#目錄)**

### 推薦使用 final 類

能用時盡量使用 `final` 關鍵字:

1. 阻止不受控的繼承鏈
2. 鼓勵 [組合](#少用繼承多用組合).
3. 鼓勵 [單一職責模式](#單一職責模式).
4. 鼓勵開發者用你的公開方法而非通過繼承類獲取受保護方法的訪問權限.
5. 使得在不破壞使用你的類的應用的情況下修改代碼成為可能.

The only condition is that your class should implement an interface and no other public methods are defined.

For more informations you can read [the blog post](https://ocramius.github.io/blog/when-to-declare-classes-final/) on this topic written by [Marco Pivetta (Ocramius)](https://ocramius.github.io/).

**Bad：**

```php
final class Car
{
    private $color;
    
    public function __construct($color)
    {
        $this->color = $color;
    }
    
    /**
     * @return string The color of the vehicle
     */
    public function getColor() 
    {
        return $this->color;
    }
}
```

**Good：**

```php
interface Vehicle
{
    /**
     * @return string The color of the vehicle
     */
    public function getColor();
}

final class Car implements Vehicle
{
    private $color;
    
    public function __construct($color)
    {
        $this->color = $color;
    }
    
    /**
     * {@inheritdoc}
     */
    public function getColor() 
    {
        return $this->color;
    }
}
```

**[⬆ 返回頂部](#目錄)**

## SOLID

**SOLID** 是Michael Feathers推薦的便於記憶的首字母簡寫，它代表了Robert Martin命名的最重要的五個面對對像編碼設計原則

 * [S: 單一職責原則 (SRP)](#職責原則)
 * [O: 開閉原則 (OCP)](#開閉原則)
 * [L: 裡氏替換原則 (LSP)](#裡氏替換原則)
 * [I: 接口隔離原則 (ISP)](#接口隔離原則)
 * [D: 依賴倒置原則 (DIP)](#依賴倒置原則)


### 單一職責原則

Single Responsibility Principle (SRP)

正如在Clean Code所述，"修改一個類應該只為一個理由"。
人們總是易於用一堆方法塞滿一個類，如同我們只能在飛機上
只能攜帶一個行李箱（把所有的東西都塞到箱子裡）。這樣做
的問題是：從概念上這樣的類不是高內聚的，並且留下了很多
理由去修改它。將你需要修改類的次數降低到最小很重要。
這是因為，當有很多方法在類中時，修改其中一處，你很難知
曉在代碼庫中哪些依賴的模塊會被影響到。

**壞：**

```php
class UserSettings
{
    private $user;

    public function __construct(User $user)
    {
        $this->user = $user;
    }

    public function changeSettings(array $settings): void
    {
        if ($this->verifyCredentials()) {
            // ...
        }
    }

    private function verifyCredentials(): bool
    {
        // ...
    }
}
```

**好：**

```php
class UserAuth 
{
    private $user;

    public function __construct(User $user)
    {
        $this->user = $user;
    }
    
    public function verifyCredentials(): bool
    {
        // ...
    }
}

class UserSettings 
{
    private $user;
    private $auth;

    public function __construct(User $user) 
    {
        $this->user = $user;
        $this->auth = new UserAuth($user);
    }

    public function changeSettings(array $settings): void
    {
        if ($this->auth->verifyCredentials()) {
            // ...
        }
    }
}
```

**[⬆ 返回頂部](#目錄)**

### 開閉原則

Open/Closed Principle (OCP)

正如Bertrand Meyer所述，"軟件的工件（ classes, modules, functions 等）
應該對擴展開放，對修改關閉。" 然而這句話意味著什麼呢？這個原則大體上表示你
應該允許在不改變已有代碼的情況下增加新的功能

**壞：**

```php
abstract class Adapter
{
    protected $name;

    public function getName(): string
    {
        return $this->name;
    }
}

class AjaxAdapter extends Adapter
{
    public function __construct()
    {
        parent::__construct();

        $this->name = 'ajaxAdapter';
    }
}

class NodeAdapter extends Adapter
{
    public function __construct()
    {
        parent::__construct();

        $this->name = 'nodeAdapter';
    }
}

class HttpRequester
{
    private $adapter;

    public function __construct(Adapter $adapter)
    {
        $this->adapter = $adapter;
    }

    public function fetch(string $url): Promise
    {
        $adapterName = $this->adapter->getName();

        if ($adapterName === 'ajaxAdapter') {
            return $this->makeAjaxCall($url);
        } elseif ($adapterName === 'httpNodeAdapter') {
            return $this->makeHttpCall($url);
        }
    }

    private function makeAjaxCall(string $url): Promise
    {
        // request and return promise
    }

    private function makeHttpCall(string $url): Promise
    {
        // request and return promise
    }
}
```

**好：**

```php
interface Adapter
{
    public function request(string $url): Promise;
}

class AjaxAdapter implements Adapter
{
    public function request(string $url): Promise
    {
        // request and return promise
    }
}

class NodeAdapter implements Adapter
{
    public function request(string $url): Promise
    {
        // request and return promise
    }
}

class HttpRequester
{
    private $adapter;

    public function __construct(Adapter $adapter)
    {
        $this->adapter = $adapter;
    }

    public function fetch(string $url): Promise
    {
        return $this->adapter->request($url);
    }
}
```

**[⬆ 返回頂部](#目錄)**

### 里氏替換原則

Liskov Substitution Principle (LSP)

這是一個簡單的原則，卻用了一個不好理解的術語。它的正式定義是
"如果S是T的子類型，那麼在不改變程序原有既定屬性（檢查、執行
任務等）的前提下，任何T類型的對像都可以使用S類型的對像替代
（例如，使用S的對像可以替代T的對像）" 這個定義更難理解:-)。

對這個概念最好的解釋是：如果你有一個父類和一個子類，在不改變
原有結果正確性的前提下父類和子類可以互換。這個聽起來依舊讓人
有些迷惑，所以讓我們來看一個經典的正方形-長方形的例子。從數學
上講，正方形是一種長方形，但是當你的模型通過繼承使用了"is-a"
的關系時，就不對了。

**壞：**

```php
class Rectangle
{
    protected $width = 0;
    protected $height = 0;

    public function setWidth(int $width): void
    {
        $this->width = $width;
    }

    public function setHeight(int $height): void
    {
        $this->height = $height;
    }

    public function getArea(): int
    {
        return $this->width * $this->height;
    }
}

class Square extends Rectangle
{
    public function setWidth(int $width): void
    {
        $this->width = $this->height = $width;
    }

    public function setHeight(int $height): void
    {
        $this->width = $this->height = $height;
    }
}

function printArea(Rectangle $rectangle): void
{
    $rectangle->setWidth(4);
    $rectangle->setHeight(5);
 
    // BAD: Will return 25 for Square. Should be 20.
    echo sprintf('%s has area %d.', get_class($rectangle), $rectangle->getArea()).PHP_EOL;
}

$rectangles = [new Rectangle(), new Square()];

foreach ($rectangles as $rectangle) {
    printArea($rectangle);
}
```

**好：**

最好是將這兩種四邊形分別對待，用一個適合兩種類型的更通用子類型來代替。

盡管正方形和長方形看起來很相似，但他們是不同的。
正方形更接近菱形，而長方形更接近平行四邊形。但他們不是子類型。
盡管相似，正方形、長方形、菱形、平行四邊形都是有自己屬性的不同形狀。

```php
interface Shape
{
    public function getArea(): int;
}

class Rectangle implements Shape
{
    private $width = 0;
    private $height = 0;

    public function __construct(int $width, int $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getArea(): int
    {
        return $this->width * $this->height;
    }
}

class Square implements Shape
{
    private $length = 0;

    public function __construct(int $length)
    {
        $this->length = $length;
    }

    public function getArea(): int
    {
        return $this->length ** 2;
    }
}

function printArea(Shape $shape): void
{
    echo sprintf('%s has area %d.', get_class($shape), $shape->getArea()).PHP_EOL;
}

$shapes = [new Rectangle(4, 5), new Square(5)];

foreach ($shapes as $shape) {
    printArea($shape);
}
```

**[⬆ 返回頂部](#目錄)**

### 接口隔離原則

Interface Segregation Principle (ISP)

接口隔離原則表示："調用方不應該被強制依賴於他不需要的接口"

有一個清晰的例子來說明示範這條原則。當一個類需要一個大量的設置項，
為了方便不會要求調用方去設置大量的選項，因為在通常他們不需要所有的
設置項。使設置項可選有助於我們避免產生"胖接口"

**壞：**

```php
interface Employee
{
    public function work(): void;

    public function eat(): void;
}

class HumanEmployee implements Employee
{
    public function work(): void
    {
        // ....working
    }

    public function eat(): void
    {
        // ...... eating in lunch break
    }
}

class RobotEmployee implements Employee
{
    public function work(): void
    {
        //.... working much more
    }

    public function eat(): void
    {
        //.... robot can't eat, but it must implement this method
    }
}
```

**好：**

不是每一個工人都是雇員，但是每一個雇員都是一個工人

```php
interface Workable
{
    public function work(): void;
}

interface Feedable
{
    public function eat(): void;
}

interface Employee extends Feedable, Workable
{
}

class HumanEmployee implements Employee
{
    public function work(): void
    {
        // ....working
    }

    public function eat(): void
    {
        //.... eating in lunch break
    }
}

// robot can only work
class RobotEmployee implements Workable
{
    public function work(): void
    {
        // ....working
    }
}
```

**[⬆ 返回頂部](#目錄)**

### 依賴倒置原則

Dependency Inversion Principle (DIP)

這條原則說明兩個基本的要點：
1. 高階的模塊不應該依賴低階的模塊，它們都應該依賴於抽像
2. 抽像不應該依賴於實現，實現應該依賴於抽像

這條起初看起來有點晦澀難懂，但是如果你使用過 PHP 框架（例如 Symfony），你應該見過
依賴注入（DI），它是對這個概念的實現。雖然它們不是完全相等的概念，依賴倒置原則使高階模塊
與低階模塊的實現細節和創建分離。可以使用依賴注入（DI）這種方式來實現它。最大的好處
是它使模塊之間解耦。耦合會導致你難於重構，它是一種非常糟糕的的開發模式。

**壞：**

```php
class Employee
{
    public function work(): void
    {
        // ....working
    }
}

class Robot extends Employee
{
    public function work(): void
    {
        //.... working much more
    }
}

class Manager
{
    private $employee;

    public function __construct(Employee $employee)
    {
        $this->employee = $employee;
    }

    public function manage(): void
    {
        $this->employee->work();
    }
}
```

**好：**

```php
interface Employee
{
    public function work(): void;
}

class Human implements Employee
{
    public function work(): void
    {
        // ....working
    }
}

class Robot implements Employee
{
    public function work(): void
    {
        //.... working much more
    }
}

class Manager
{
    private $employee;

    public function __construct(Employee $employee)
    {
        $this->employee = $employee;
    }

    public function manage(): void
    {
        $this->employee->work();
    }
}
```

**[⬆ 返回頂部](#目錄)**

## 別寫重複代碼 (DRY)

試著去遵循[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) 原則.

盡你最大的努力去避免復制代碼，它是一種非常糟糕的行為，復制代碼
通常意味著當你需要變更一些邏輯時，你需要修改不止一處。

試想一下，如果你在經營一家餐廳並且你在記錄你倉庫的進銷記錄：所有
的土豆，洋蔥，大蒜，辣椒等。如果你有多個列表來管理進銷記錄，當你
用其中一些土豆做菜時你需要更新所有的列表。如果你只有一個列表的話
只有一個地方需要更新。

通常情況下你復制代碼是應該有兩個或者多個略微不同的邏輯，它們大多數
都是一樣的，但是由於它們的區別致使你必須有兩個或者多個隔離的但大部
分相同的方法，移除重複的代碼意味著用一個function/module/class創
建一個能處理差異的抽像。

用對抽像非常關鍵，這正是為什麼你必須學習遵守在[類](#類)章節寫
的SOLID原則，不合理的抽像比復制代碼更糟糕，所以務必謹慎！說了這麼多，
如果你能設計一個合理的抽像，那就這麼干！別寫重複代碼，否則你會發現
任何時候當你想修改一個邏輯時你必須修改多個地方。

**壞：**

```php
function showDeveloperList(array $developers): void
{
    foreach ($developers as $developer) {
        $expectedSalary = $developer->calculateExpectedSalary();
        $experience = $developer->getExperience();
        $githubLink = $developer->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}

function showManagerList(array $managers): void
{
    foreach ($managers as $manager) {
        $expectedSalary = $manager->calculateExpectedSalary();
        $experience = $manager->getExperience();
        $githubLink = $manager->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}
```

**好：**

```php
function showList(array $employees): void
{
    foreach ($employees as $employee) {
        $expectedSalary = $employee->calculateExpectedSalary();
        $experience = $employee->getExperience();
        $githubLink = $employee->getGithubLink();
        $data = [
            $expectedSalary,
            $experience,
            $githubLink
        ];

        render($data);
    }
}
```

**極好：**

最好讓代碼緊湊一點

```php
function showList(array $employees): void
{
    foreach ($employees as $employee) {
        render([
            $employee->calculateExpectedSalary(),
            $employee->getExperience(),
            $employee->getGithubLink()
        ]);
    }
}
```

**[⬆ 返回頂部](#目錄)**

## 翻譯

其他語言的翻譯:

* :cn: **Chinese：**
   * [php-cpm/clean-code-php](https://github.com/php-cpm/clean-code-php)
* :ru: **Russian：**
   * [peter-gribanov/clean-code-php](https://github.com/peter-gribanov/clean-code-php)
* :es: **Spanish：**
   * [fikoborquez/clean-code-php](https://github.com/fikoborquez/clean-code-php)
* :brazil: **Portuguese：**
   * [fabioars/clean-code-php](https://github.com/fabioars/clean-code-php)
   * [jeanjar/clean-code-php](https://github.com/jeanjar/clean-code-php/tree/pt-br)
* :thailand: **Thai：**
   * [panuwizzle/clean-code-php](https://github.com/panuwizzle/clean-code-php)
* :fr: **French：**
   * [errorname/clean-code-php](https://github.com/errorname/clean-code-php)
* :vietnam: **Vietnamese**
   * [viethuongdev/clean-code-php](https://github.com/viethuongdev/clean-code-php)
* :kr: **Korean：**
   * [yujineeee/clean-code-php](https://github.com/yujineeee/clean-code-php)
* :tr: **Turkish：**
   * [anilozmen/clean-code-php](https://github.com/anilozmen/clean-code-php)
   
**[⬆ 返回頂部](#目錄)**
