Ch02b 預防性程設
===

> 就像排骨牌一樣，我們會設許多斷點，預防錯誤的擴散。

##  斷言

本節介紹的斷言 java assertion 與例外處理都是一種防禦性的編程方法。

斷言是 java 中用來檢查程式中的假設與斷言。例如你寫一個程式判斷汽車（或火箭）的速度，不管怎麼快，也不可能比光速快。所以你可以下一個斷言：計算出來的速度必定小於光速。當這個斷言被違反了，表示你的程式可能出現問題。

其語法為：

```java 
// assert 後方接判斷句，例如 x<=100
assert x<=100; 
```

或

```java 
// 如果沒有滿足錯誤，就顯示 errMessage 訊息
// assert expression: errMessage
assert x<=100 : "錯誤的總成績" ; 
```

其中 Expression1 是一個 boolean 表示式。errMessage 是一個含有值的表示式，當斷言不成立時，會拋出 java.lang.AssertionError 的例外，並把 errMessage 的值呈現出來。

除此以外，斷言也可以幫助提升程式的可閱讀。如果你不用斷言，你的程式碼可能會出現：

:-1: 不好的寫法：
```java
if (x<0) then
   print("奇怪！");
```

這樣的程式碼，會和原有的程式邏輯攪在一起，降低了可閱讀性，造成維護上的困難。

以下這個範例說明了 assert 的應用，假設我們自己寫一個 square (平方) 的程式，採取防禦性的開發方式每一次計算後就會斷言 a>=0，提昇程式的可靠度。

```java
Scanner sc = new Scanner(System.in);
int a = sc.nextInt();
int b = square(a);
assert (a>=0): "平方後不可能 < 0";
```


#### 斷言的啟動

為了讓 assertion 不會造成程式的負擔，你可以在編譯的時候決定是否要將 assertion 啟動（enable）。

> javac -ea Calculator.java

其中 `ea` 表示 enable assertion。

![](https://hackmd.io/_uploads/rJg4NE602.png)
:barber: 在 intellij 上設定 -ea 

> 為你的一段程式碼加上斷言，enable 斷言以查核其正確。待確定程式無誤後，disable 這些斷言

```java=
package org.example.u2debug;

public class AssertionDemo {

    public static void main(String[] args) {
        AssertionDemo1 at = new AssertionDemo1();
        System.out.println(at.checkTriangle(10, 23, 11));
        System.out.println(at.checkTriangle(1, 1, 1));
        System.out.println(at.checkTriangle(2, 2, 3));
        System.out.println(at.checkTriangle(3, 2, 2));
        System.out.println(at.checkTriangle(0, -1, -2));
    }

    public String checkTriangle(int a, int b, int c) {
        if (a <= 0 || b <= 0 || c <= 0) {
            System.out.println("長度不可以是負的");
        }
        assert a > 0 && b > 0 && c > 0;
        if (a + b > c && b + c > a && c + a > b) {
            if (a == b)
                if (b == c) {
                    return "正三角形";
                } else
                    return "等腰三角形";
            else if (b == c) {
                assert (a != b);
                return "等腰三角形";
            }
            assert (a != b);
            assert (a != c);
            assert (b != c);
            return "三角形";
        }
        return "非三角形";
    }
}

```

#### 什麼時候該用斷言?

- 開發設計階段。使用 assertion 來測試，檢查程式中的錯誤。當此階段結束：即便沒有啟動 assertion, 也不會擔心系統錯誤。

- 內部的不變式 Internal Invariants。程式撰寫的過程中，有任何的不變式都可以加上 assertion。

    ```java
    if (i % 3 == 0) {
       ...
    } else if (i % 3 == 1) {
       ...
    } else {
       // 斷言前面的條件若不滿足，則 i%2 一定會等於 2
       assert i % 3 == 2 : i; 
       ...
    }
    ```

加上這樣的斷言好像沒有作用，但當 i 的值為負數時，你會發現你以為的 2 並不會成真。這就是利用斷言的好處。

- 類別的不變式 Class invariant。類別不變式表示該物件任何方法執行的前後都恆真的式子，例如一個 Stack 物件內部元素的個數一定介於 0 與最大值之間。

    ```java
    private boolean inv() {
        return (num >= 0 && num < capacity);
    }
    ```

所以我們可以在執行任何動作之後呼叫 inv() 來檢查是否發生異常：

    ```java
    push(x);
    assert inv();
    ...
    y = pop(x);
    assert inv();
    ```

- 不變的控制流 Control-Flow Invariants。對控制流程的斷言，我相信絕對不會到 05 行，因為迴圈內有一定會有一個滿足 if 的條件式。

```java
void foo() { 
   for (...) {
      if (...) return;
   }
   assert false; 
}
```

如此，萬一真的跑到 05, 系統就會拋出例外警告。

- 後置條件 Postconditions。在一段複雜的運算後，你斷定會成為真的事情，用斷言來加強。

    ```java
    sort(SORT.INC); //遞增
    if (data.length >=2) assert data[0] <= data[1];
    sort(SORT.DEC); //遞減
    if (data.length >=2) assert data[0] >= data[1];
    ```


#### 什麼時候不該使用斷言?

- 不要使用斷言來做公開方法的參數檢查。公開方法對參數的檢查本來就應該做，不要用 斷言 來檢查。公開的方法表示會有很多其他人會呼叫此方法，傳入各種不同的參數，為了避免他們傳錯，方法本來就必須測試這些參數，所以不該用斷言的方式來檢查。

例如下面的程式，我們本來就該檢查 grade 是否正確，如果用 assert 來做，當我們 enable assert  時，成績輸入錯就檢查不出來了。

```java
public void addGrade(int grade) {
   assert x <= 100: "grade > 100 error";
   sum + = grade;    
}
```

應該改成下面：

```java
public void addGrade(int grade) {
   if (grade > 100) 
       throw new Exception("grade > 100 error");
   sum + = grade;    
}
```

相反的，若是私有方法，其參數的約定式內部的，我們應該很清楚的知道不該有那樣不正確的參數傳入，所以可以用斷言。如果真的傳入奇怪的參數，就會拋出例外。

- 不要使用斷言來執行程式邏輯相關的操作。不要把程式該做的工作放在 assert 中。因為 assert 以後可能會被 disable。例如在象棋系統中，某一段邏輯我們棋子移動或是吃子，因為移動或吃子都可能違法，所以他的 return type 是 boolean, 但切記不用 assert 去判斷這些動作：

```java
public void playGame() {
   ...
   assert chess1.move(12);
   ...
   assert chess3.eat(chess1);
   ...
}
```

原因一樣：如果你 disable 了斷言，這些動作就不能執行了。


總言而之，記得這個原則：(1) 開發階段，有斷言，就寫斷言 (2) 如果我們移除了斷言，系統邏輯還是要對的，還是能夠做適當的防呆偵錯。

:::success
:basketball: 中斷應用
設計一個 People 的類別，包含姓名, 身高, 體重, 生日年等資料，以及 bmi()等方法。透過 assert 來避免開發與測試時期的錯誤

- 建立合適的建構子 (constructor) 來生成 People
- bmi() 透過檢驗 bmi 的值應該落在某一個範圍內
- 新增 People 內一個屬性 father, 其型態也是 People。People 內有一個 setBirthdayYear() 的方法來改變生日年。透過 assertion 來確保生日年的合理性。
:::


Ch02c 例外處理
===


你不能掌控的事都可能出錯，發生例外。要寫一個穩健（robust）的系統，必須非常的細心，考慮到各種可能的例外情況，各種可能出錯的環境變數，並且做出應對。

Java 中例外的種類：
  
- 需查核例外 checked exception: 你必須處理它（exception handle）或宣告它（讓別人來處理）。 如果你都沒有做，compiler 不會通過，直到你寫出你的*處理碼*。在 Java 中，Exception 下的類別，除了 RuntimeException 以外都是需查核例外。
- 非查核例外 unchecked exception: 通常是程式邏輯的錯誤，例如 NullPointerException, ArrayIndexOutOfBoundsException -- 當發現此例外，你應該修正你的程式，而不是做例外處理，所以這一類的錯誤並不強制去處理。在 Java 中，RuntimeException 下的類別都屬於 非查核例外。

:::success
:point_right: 捕捉或宣告原則 Catch or Declare Rule (CDR)：
> 對於例外你只有兩個選擇：一是處理它，二是宣告這個例外讓呼叫者處理。
:::

> 寫一個模組程式和做一件事一樣，一方面你要把事情做好，一方面你要知道如何界定工作範圍 -- 對於你不能處理的事，你得上承給他人處理。

## 捕捉後處理

這個程式中我們必須讀取 grade.txt，這是程式邏輯無法控制的事（無法確定執行環境會不會有此檔案），所以有可能產生例外。*FileNotFoundException* 是 *Exception* 的子類別，屬於 需查核例外，我們的處理方式就是呈現訊息，並且跳離系統。

```java
// use Exception
static void openFile() {
   try {
      FileInputStream f = new FileInputStream("grade.txt");
   } catch (FileNotFoundException e) {
      System.out.println("File does not exist");
      e.printStackTrace();
    }
}
```

#### People Example

```java=
public class Main {
    public static void main(String[] args) {

        People john = new People("John", 1990);
        try {
            john.setHW(170, 60);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
        float bmi = john.getBMI();
        System.out.println(bmi);

        People nick = null;
        try {
            nick = new People("name", 1.7f, 60, 1990);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
//        nick.setHW(1.7f, 60);
        bmi = nick.getBMI();
        System.out.println(bmi);

        bmi = john.getBMI();
        System.out.println(bmi);

//        john.setHW(1.7f, 60);
//        bmi = john.getBMI();
//        System.out.println(bmi);
//
//        john.setHW(1.7f, 60);
//        bmi = john.getBMI();
//        System.out.println(bmi);
    }
}

class People {
    String name;
    float height, weight, bmi;
    int birthYear;

    public People(String name, int birthYear) {
        this.name = name;
        this.birthYear = birthYear;
    }

    public People(String name, float height, float weight, int birthYear) throws Exception {
        this.birthYear = birthYear;
        this.name = name;
        setHW(height, weight);
    }

    public void setHW(float height, float weight) throws Exception {
        if (height > 2.2) {
            throw new Exception("invalid height");
        }
        this.height = height;
        this.weight = weight;
        this.bmi = weight / (height * height);
    }

    float getBMI() {
        assert this.bmi > 10 && this.bmi <= 50;
        return bmi;
    }
}
```

自己定義 Exception

```java=
public class ExceptionDemo {

    public static void main(String[] args) {
        int[] grade = {100, 2, -2};
        try {
            getGradeAverage(grade);
        } catch (WrongGradeException e) {
            e.printStackTrace();
        }
    }

    /*
     * This program will demo how to define and use your exception
     */
    static double getGradeAverage(int g[]) throws WrongGradeException {
        int sum = 0;
        for (int i = 0; i < g.length; i++) {
            if (g[i] > 100 || g[i] < 0) {
                throw new WrongGradeException();
            }
            sum += g[i];
        }
        return sum / (double) (g.length);
    }
}

class WrongGradeException extends Exception {
    public WrongGradeException(String title) {
        super(title);
    }

    public WrongGradeException() {
        super("The grade is wrong, maybe greater than 100 or less than 0");
    }
}
```

## 捕捉再拋出

下述的程式中我們的處理方式是重新拋出該例外。請注意第 1 行的 throws 有加上 s, 第 5 行的則沒有。在宣告區的 throws 是有加上 s 的。

```java 
static void openFile3() throws FileNotFoundException {
   try {
      FileInputStream f = new FileInputStream("grade.txt");
   } catch (FileNotFoundException e) {
      System.out.println("File does not exist");
      // re-throw the exception
      throw new FileNotFoundException();
   }
}
```

## 自己的例外自己定義

```java
static double getGradeAverage(int g[]) throws WrongGradeException {
    int sum = 0;
    for (int i = 0; i < g.length; i++) {
        if (g[i] > 100 || g[i] < 0) {
            throw new WrongGradeException(g[i]);
        }
        sum += g[i];
    }
    return sum / (double) (g.length);
}

// 自己定義的例外，裡面還封裝了成績分數
class WrongGradeException extends Exception {
    int wrongGrade;
    public WrongGradeException(int grade) {
        super("The grade is wrong, maybe > 100 or < 0");    
        this.wrongGrade = grade;
    }    
    public int getGrade() { 
       return wrongGrade; 
    }
}
```

:::success
People 類別內有出生年月日的屬性。請在生成方法內檢查例外（例如不合理的日期）並處理之。


```
class People {
   public People(int year, int month, int date) {
      ...
   }
   
}
```
- 同上，若這例外這是你無法處理的狀況 -- 請拋出一例外給上層（City 類別內會生成 People）來處理。
- 同上，請宣告一個 InvalidDateException 來處理之。
:::


> 把「例外」視為「常態」是專業工程師必備的態度。