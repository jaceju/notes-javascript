# JavaScript  基礎

## 如何在瀏覽器中執行 JavaScript

1. 放在 `<script>...</script>` 之間執行
2. 放在外部的 .js 檔案中，再以 script 標籤的 `src` 載入執行

## script 放在哪裡？

* 放在最後 (`</body>` 之前) 。
* 避免程式執行緩慢而阻斷了頁面的顯示。

## alert() 與 console.log

* alert 會停下來等使用者回應，就是阻斷的一種。
* 改用 console.log 可以避免阻斷程式執行，結果將顯示在 console 中。
* Console 可以在各家瀏覽器的開發者工具上找到。

## 變數

* 用 `var` 來宣告變數，同時給變數初始值。

      var myVar1;
      var myVar2 = 123;

* 多個變數可以用逗號 (`,`) 同時宣告。

      var myVar1, myVar2 = 123;

## 類型

### 數字 (Number)

* 有「整數」「浮點數」

### 字串 (String)

* 用單引號或雙引號包起來的稱為字串
* 字串可以用加號 (+) 將多個字串組合起來。

### 常數

* 存放不能被改變的值
* 名稱通常是用全大寫
* 以往是用變數來模擬，在 ECMAScript 5 之後可以用 `const` 關鍵字來定義
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const
* http://kangax.github.io/es5-compat-table/es6/#const

## 操作 HTML 元素

* `document.getElementById()`
* `document.querySelector()`
* `document.querySelectorAll()`
* `innerHTML`
* `onclick`
* `element.addEventListener()`

### `document.getElementById()` 

將變數與到 HTML 上有 id 屬性的元素綁在一起。 (注意大小寫)

    <div id="id-name">HTML內容</div>

    var element = document.getElementById("id-name");

### `document.querySelector()`

利用 CSS 規則語法來取得單一個 HTML 元素。

    <div id="id-name">HTML內容</div>

    var element = document.querySelector("#id-name"); // # 開頭表示 CSS 的 id 

### `document.querySelectorAll()`

利用 CSS 規則語法來取得一組 HTML 元素群。

    <div class="class-name">HTML內容</div>
    <div class="class-name">HTML內容</div>
    <div class="class-name">HTML內容</div>

    var elements = document.querySelectorAll(".class-name"); // . 開頭表示 CSS 的 class
    var element1 = elements[0]; // 可以用陣列的形式來取得元素群的單一元素

### `innerHTML` 

* 取出 HTML 元素包含的內容
* 也可以指定 HTML 內容給 HTML 元素。

範例：

      var html = element1.innerHTML;
      element2.innerHTML = html;

### `onclick` 

* 表示 HTML 元素的按下事件
* 可以接受一個匿名函式當做動作。
* 當按下按鍵時，就會觸發這個動作。
* 重複指定匿名函式的話，新的會蓋掉舊的，所以只會執行最新的。

`onclick` 裡的匿名函式中的 `this` 代表 `onclick` 所屬的 HTML 元素。

範例：

HTML:

    <a id="link" href="/path/to/resource">Test</a>

JavaScript:
    
    var link = document.getElementById('link');
    link.onclick = function () {
        console.log(this.href); // 顯示 "/path/to/resource"
    }

### `element.addEventListener()`

* 可以對事件新增一個監聽的處理函式
* 跟 `onXXXX` 不同的地方在於可以對同一事件註冊多個處理函式

範例：

HTML:

    <a id="link" href="/path/to/resource">Test</a>
    
JavaScript:

    var link = document.querySelector('#link');
    link.addEventListener('click', function () {
        console.log(this.href); // 顯示 "/path/to/resource"
    });

## 句點操作符

句點 (`.`) 後面接的是某個東西的屬性或方法

例：

* `innerHTML` 是某 HTML 元素的屬性
* `getElementById` 是 `document` 的方法

## 函式

* 函式分為指名函式及匿名函式
* 函式的宣告方式有兩種：

  1. 直接對函式命名。
  2. 將匿名函式指定給一個變數。

## 函式呼叫時機

函式依照宣告方式的不同，會影響呼叫的時機。

### 指名函式

範例：

    myFunc(); // 可以在宣告前呼叫

    function myFunc1() {
        // do something
    }

    myFunc(); // 也可以在宣告後呼叫
    
### 匿名函式指定給變數

範例：

    myFunc(); // 不可在宣告前呼叫，會發生函式未定義的錯誤

    var myFunc = function () {
        // do something
    }
    
    myFunc(); // 可以在宣告後呼叫    

## 陣列

* 陣列用方括號 (`[]`) 宣告及初始化
* 陣列索引從 0 開始。

範例：

    var myArray = ['John', 'Joe', 'Kevin'];
    console.log(myArray[0]); // "John"
    console.log(myArray[1]); // "Joe"
    console.log(myArray[2]); // "Kevin"

## for 敘述

`for` 有兩種形式，第一種是：

    for (初始化變數; 終止條件判斷; 步進式) {
        // do something
    }

範例：

    var i, myArray = [2, 56, 68, 1, 44, 92, 15, 34, 24, 78];
    
    for (i = 0; i < myArray.length; i ++) {
        console.log(myArray[i]);
    }

這裡 `for` 會多執行一次來判斷是否要進行迴圈。

第二種是：

    for (索引 in 陣列) {
        // do something
    }

範例：

    var i, myArray = [2, 56, 68, 1, 44, 92, 15, 34, 24, 78];
    
    for (i in myArray) {
        console.log(myArray[i]);
    }

注意 `myArray[i]` 才是陣列元素內容。

以上兩個方法都可以用來一一處理 HTML 元素群：

    var i, elements = document.querySelectorAll('.class-name');
    
    for (i = 0; i < elements.length; i ++) {
        var el = elements[i];
        // do something
    }
    
    for (i in elements) {
        var el = elements[i];
        // do something
    }

## if 敘述

if 用來判斷條件式是否成立，成立時就執行第一個大括號裡的程式碼，否則就執行 `else` 後的程式碼。

    if (condition) {
        // do this when condition is true
    } else {
        // do this when condition is false.
    }