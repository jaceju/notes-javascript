# JavaScript 物件導向基礎

## 如何表示物件

範例：

    var p1 = {
        name: '小明',
        birthday: '1980/2/28',
        'phone-number': '0912345678' // 注意最後沒有逗號
    };

* 用 `{ ... }` 包起來表示物件實體 (instance)
* 物件實體表示一個實際存在的物件
* `key: value` 表示屬性，每個屬性之間用逗號 (`.`) 區隔
* 非標準變數名稱做為 key 時，要加引號 (`'...'` 或 `"..."`)

## 操作物件實體

範例：

    console.log(p1.name);
    console.log(p1.birthday);
    console.log(p1['phone-number']);

    p1.name = '小華';
    p1['phone-number'] = '0921876543';

* 用 `.` 來取得屬性
* 用 `[...]` 來取得非標準名稱的屬性

## 物件實體的別名

範例：

    var p1 = {
        name: '小明'
    };

    console.log(p1.name); // "小明"

    var p2 = p1;
    p2.name = '小華';
    console.log(p2.name); // "小華"
    console.log(p1.name); // "小華"

* `p2` 跟 `p1` 可以視為是一個指標變數，都是指向同一個物件實體

## 用外部函式來操作物件實體

範例：

    var p1 = { /* ... */ };

    function who(p) {

        var birthday = new Date(p.birthday);
        // 1000 * 60 * 60 * 24 * 365 = 31536000000
        var age = Math.floor((new Date() - birthday) / 31536000000);

        return 'I\'m ' + p.name + ', my age is ' + age;
    }

    who(p1);

* 函式主要是將一個常用的邏輯包裝起來
* 利用帶入參數的方式來更改操作的對象 (物件實體)

## 讓物件實體負責自己的行為

範例：

    var p1 = {
        name: '小明',
        birthday: '1980/2/28',
        who: function () {

            var birthday = new Date(p1.birthday);
            var age = Math.floor((new Date() - birthday) / 31536000000);

            return 'I\'m ' + p1.name + ', my age is ' + age;
        }
    };

    console.log(p1.who());

* 如果函式應該是跟隨著物件實體，那麼可以把它改成為物件實體的屬性
* 此時跟著物件實體的函式應該改稱為「方法」

## 第一次接觸 `this`

範例：

    var p1 = {
        name: '小明',
        birthday: '1980/2/28',
        who: function () {

            var birthday = new Date(this.birthday);
            var age = Math.floor((new Date() - birthday) / 31536000000);

            return 'I\'m ' + this.name + ', my age is ' + age;
        }
    };

* 一般狀況下，在方法中的 `this` 會指回物件實體自己
* 在物件實體中需要透過 `this` 來存取自己的屬性

### 當方法中有函式時

範例：

    var p1 = {
        name: '小明',
        birthday: '1980/2/28',
        who: function () {

            // 用生日計算年齡
            function age() {
                console.log(this);
                var birthday = new Date(this.birthday);
                return Math.floor((new Date() - birthday) / 31536000000);
            }

            return 'I\'m ' + this.name + ', my age is ' + age();
        }
    };

* 在方法裡可以再包含函式
* 方法裡的函式用到的 `this` 就不再是指到原來的物件
* 在方法裡的函式中， `this` 變成了 Global 的 `window` 物件

### 改法一：用參數傳遞

* `age` 改為帶一個參數 `that` 的函式
* `age` 中的 `this` 改成 `that`
* 在 `who` 方法中把 `this` 傳給 `age`

範例：

    var p1 = {
      name: '小明',
      birthday: '1980/2/28',
      who: function () {

        function age(that) {
          var birthday = new Date(that.birthday);
          return Math.floor((new Date() - birthday) / (1000 * 60 * 60 * 24 * 365));
        }

        return 'I\'m ' + this.name + ', my age is ' + age(this);
      }
    };

    console.log(p1.who());

### 改法二：利用變數的 scope

* 利用 `that` 變數暫時記住 `this`
* `age` 中的 `this` 改成 `that`

範例：

    var p1 = {
      name: '小明',
      birthday: '1980/2/28',
      who: function () {
        
        var that = this;

        function age() {
          var birthday = new Date(that.birthday);
          return Math.floor((new Date() - birthday) / (1000 * 60 * 60 * 24 * 365));
        }

        return 'I\'m ' + this.name + ', my age is ' + age();
      }
    };

    console.log(p1.who());

## 物件中方法呼叫方法

* 把 `age()` 函式變成方法
* 在 `who` 方法中改用 `this.age()` 來呼叫

範例：

    var p1 = {
      name: '小明',
      birthday: '1980/2/28',
      who: function () {
        return 'I\'m ' + this.name + ', my age is ' + this.age();
      },
      age: function () {
        var birthday = new Date(this.birthday);
        return Math.floor((new Date() - birthday) / (1000 * 60 * 60 * 24 * 365));
      }
    };

    console.log(p1.who());

* `age` 方法定義在 `who` 方法之後也沒關係

## 定義物件的類型

* `{...}` 一次只能定義一個物件實體，很難重複利用
* 用建構函式來定義一個共用類型

範例：

    var Programer = function (name, birthday, phoneNumber) {
      this.name = name;
      this.birthday = birthday;
      this['phone-number'] = phoneNumber;
      this.who = function () { /* ... */ };
      this.age = function () { /* ... */ };
    };

    var p1 = new Programer('小明', '1990-06-21', '0912345678');
    var p2 = new Programer('小華', '1991-11-03', '0922111555');

    console.log(p1);
    console.log(p2);

* 用 `new` 關鍵字來透過建構函式建立一個物件
* 建構式可以帶參數
* 要用 `this` 來指定物件成員
* 透過 `new` 呼叫建構函式時， `this` 指向建立出來的物件
* 沒有用 `new` 來呼叫建構式，這時的建構式就只是普通的函式，
  `this` 指向 Global 的 `windows` 物件。

## 再看看 `this`

範例：

    window['phone-number'] = '0999123456';

    function getPhoneNumber() {
        return this['phone-number'];
    }

    console.log(getPhoneNumber());

* 函式中的 `this` 可以看成保留的空位
* `this` 是程式執行時動態決定它要指向誰
* 預設函式中的 `this` 是指向 `window`

## 用 `apply` 來決定誰是 `this`

範例：

    var p1 = new Programer('小明', '1990/06/21', '0912345678');
    var p2 = new Programer('小華', '1991/11/03', '0922111555');

    function getPhoneNumber() {
        return this['phone-number'];
    }

    console.log(getPhoneNumber.apply(p1));
    console.log(getPhoneNumber.apply(p2));

* 函式變數會提供一個 `apply` 方法
* `apply` 會將第一個參數連結到函式中的 `this`

## 有參數的函式使用 `apply`

範例：

    function profile(label1, label2, label3) {
      var s = '';
      s += label1 + ': ' + this.name + '\n';
      s += label2 + ': ' + this.birthday + '\n';
      s += label3 + ': ' + this['phone-number'] + '\n';
      return s;
    }

    console.log(profile.apply(p1, ['Name', 'Birthday', 'Phone Number']));
    // 'Name' 對應 label1
    // 'Birthday' 對應 label2
    // 'Phone Number' 對應 label3

* `apply` 第二個參數是接一個陣列，陣列值會依序傳給函式的參數

## Prototype 初探

* JavaScript 是 Protoype-based 物件導向語言
* 可以用 Prototype 來擴充類型的屬性與方法

範例：

    var Programer = function () { /* ... */ };
    var p1 = new Programer();

    console.log(p1.profile('Name', 'Birthday', 'Phone Number'));
    // 錯誤， profile 還沒有宣告

    // 讓 profile 函式跟著 Programer 這個類型
    Programer.prototype.profile = function (label1, label2, label3) {
      /* ... */
    };

    console.log(p1.profile('Name', 'Birthday', 'Phone Number'));
    // 正確， profile 可以被所有類型為 Programer 的物件實體使用

* 建構函式的 `prototype` 屬性可以讓我們擴充類型的屬性與方法
* 這時候 `profile` 中的 `this` 就會自動對應到我們透過 `new` 建立的物件
* 修改 `prototype` 會影響所有透過 `new` 所建立的該類型物件
* 只有類型會有 `prototype` 屬性，物件實體

## Protytpe 繼承

* 類型可以透過 `prototype` 屬性來繼承另一個類型

範例：

    var Employee = function () {
      this.salary = 0;
      this.setSalary = function (salary) {
        this.salary = salary;
      };
      this.getSalary = function () {
        return this.salary;
      };
    };

    var Programer = function (name, birthday, phoneNumber) {
      /* ... */
    };

    Programer.prototype = new Employee();

    var p1 = new Programer('小明', '1980/2/28');

    p1.setSalary(22000);
    console.log(p1.getSalary());

* 必須用 `new` 初始化一個要繼承的類型，然後放到 `prototype` 屬性中
* 被繼承的類型中，裡面的 `this` 會指向透過 `new` 所建立的物件實體