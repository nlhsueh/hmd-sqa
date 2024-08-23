

JUnit 
===

:point_right::point_right: [Junit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)

#### 測試先行原則

以前你是：設計 => 寫程式  => 測試。現在你應該：設計 => 寫測試碼 => 寫程式 => 測試

- 設計程式框架
- 設計測試案例：利用 JUnit 設計測試案例。
- 執行測試碼：執行 JUnit 測試碼，會跑出很多的「錯誤」。
- 撰寫程式
- 執行測試碼：執行 JUnit 測試碼，錯誤應該會逐漸減少。

#### 3A 原則

3A 指的是：安排 Arrange, 測試 Act, 驗證 Assert：

- Arrange：安排測試的環境或資料，
- Act：呼叫測試目標類別上要測試的方法。
- Assert：驗證預期結果與 Act 所得到的實際結果，是否符合。

## JUnit 簡介

JUnit 是一個用於 Java 程序的單元測試框架。它是開發人員用來測試他們的程式碼是否正確的一個重要工具。以下是一些有關 JUnit 的基本介紹：

1. 單元測試：JUnit 是一個用於單元測試的框架，這意味著它用於測試程式中的個別方法或函數，而不是整個應用程序。這有助於開發人員檢查他們的代碼的特定部分是否正確運作。
2. 測試框架：JUnit 提供了一個測試框架，可以用來編寫測試用例。測試用例是測試單元的最小單位，它包含了一個或多個測試方法，用於確定程式碼的某個方面是否正確。
3. 測試注解：JUnit 使用注解來標記測試方法，這些注解告訴框架哪些方法是測試方法。一些常見的 JUnit 測試注解包括 `@Test`（用於標記測試方法）、`@Before`（在每個測試方法執行之前執行）、`@After`（在每個測試方法執行之後執行）等。
4. 斷言：JUnit 提供了許多斷言方法，用於檢查測試結果是否符合預期。這些斷言方法可以確保測試方法產生預期的結果。
5. 測試運行器：JUnit 提供了不同的測試運行器，可以用於不同的測試需求。例如，`JUnit4` 中的默認運行器是 `BlockJUnit4ClassRunner`，而 `JUnit5` 中引入了不同的運行器，例如 `Jupiter` 測試引擎。
6. 測試套件：JUnit 允許你將多個測試用例組合成一個測試套件，並一次運行所有測試用例。
7. 整合性：JUnit 可以與許多集成開發環境（IDE）和建置工具（如Maven、Gradle）無縫整合，使測試更加方便。
8. 支援參數化測試：JUnit 5 引入了對參數化測試的支援，允許你在不同的參數組合下運行相同的測試方法。

總的來說，JUnit 是 Java 開發人員的一個重要工具，用於確保他們的程式碼在各種情況下都能正確運作。通過編寫單元測試，開發人員可以提高代碼的品質、可維護性和可靠性，並減少後續修復錯誤所需的時間和成本。


#### 一些觀念
以下是帶有程式碼部分的標記：

* **關於生命週期的方法 Lifecycle Method**: any method that is directly annotated or meta-annotated with `@BeforeAll`, `@AfterAll`, `@BeforeEach`, or `@AfterEach`.
* **測試類別 Test Class**: any top-level class, static member class, or `@Nested` class that contains at least one test method, i.e. a container. Test classes must not be abstract and must have a single constructor.
* **測試方法 Test Method**: any instance method that is directly annotated or meta-annotated with `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory`, or `@TestTemplate`. With the exception of `@Test`, these create a container in the test tree that groups tests or, potentially (for `@TestFactory`), other containers.


#### IntelliJ 與 JUnit

* Project
    * create a maven project
    * new a class `Calculator`
    * add a function `plus(int int)`
    * mouse over `plus` function, choose `Go to` >> `Test`, that will guide you to create a test class `CalculatorTest`
    * if junit is not imported; **google `maven junit5`**; add the following code to the maven setting file. 

```
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter-engine</artifactId>
        <version>5.9.1</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```
* The project structure will be:

![](https://hackmd.io/_uploads/Sy_9r-txp.png)

### 測試程式樣板

```java
import static org.junit.jupiter.api.Assertions.fail;
import static org.junit.jupiter.api.Assumptions.assumeTrue;

import org.junit.jupiter.api.AfterAll;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;

class StandardTests {

    @BeforeAll
    static void initAll() {
    }

    @BeforeEach
    void init() {
    }

    @Test
    void succeedingTest() {
    }

    @Test
    void failingTest() {
        fail("a failing test");
    }

    @Test
    @Disabled("for demonstration purposes")
    void skippedTest() {
        // not executed
    }

    @Test
    void abortedTest() {
        assumeTrue("abc".contains("Z"));
        fail("test should have been aborted");
    }

    @AfterEach
    void tearDown() {
    }

    @AfterAll
    static void tearDownAll() {
    }
}
```

![test life cycle](https://hackmd.io/_uploads/BkIaLWKxT.png)


#### 常用的標記

JUnit 5 中有許多重要的標記（annotations），每個注解都具有特定的意義和用途。以下是幾個重要的注解以及它們的意義：

1. `@Test`
   - 意義：將一個方法標記為測試方法。
   - 用途：用於標記要執行的單元測試方法。
2. `@BeforeEach`、`@AfterEach`
   - 意義：在每個測試方法執行之前（`@BeforeEach`）和之後（`@AfterEach`）執行指定的操作。
   - 用途：用於在每個測試方法執行前後執行初始化、清理或重置操作。
3. `@BeforeAll`、`@AfterAll`
   - 意義：在所有測試方法執行之前（`@BeforeAll`）和之後（`@AfterAll`）執行指定的操作。
   - 用途：用於在所有測試方法執行前後執行全局初始化、清理或重置操作。
4. `@DisplayName`
   - 意義：為測試方法或測試類指定自定義的顯示名稱。
   - 用途：用於提供更具描述性和易讀的測試名稱，以改善測試報告的可讀性和理解性。
5. `@Disabled`
   - 意義：標記測試方法或測試類為禁用，不會執行。
   - 用途：用於臨時禁用測試方法或測試類，通常在測試開發或維護過程中使用。
6. `@ParameterizedTest`
   - 意義：標記一個方法作為參數化測試方法，可以使用不同的輸入參數運行多次測試。
   - 用途：用於在一個測試方法中執行多次相同邏輯的測試，並驗證不同輸入對於結果的正確性。
7. `@RepeatedTest`
   - 意義：標記一個方法作為重複測試方法，可以重複運行指定次數的測試。
   - 用途：用於重複執行相同的測試多次，以驗證結果的一致性和可靠性。
8. `@Timeout`
   - 意義：指定測試方法或測試類的最大執行時間。
   - 用途：用於確保測試方法或測試類在指定的時間內完成執行，並避免無限執行的情況。


Read [更多 Junit 標記](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations)


## 斷言

:::success
寫一個 Calculator 的類別，內有 
* plus(int, int): double
* minus(int, int): double
* multiple(int, int): double

針對這些方法透過 junt 進行測試
:::

:::success
寫一個 People 的類別，內有
* getFirstName(): String
* getLastName(): String

針對這些方法透過 junt 進行測試
:::

Read [AssertionDemo (Calculator, Person)](https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions) Example

### 延遲評估訊息

在 JUnit 中，Lazily evaluating message 是一種技巧，用於在斷言（assertions）中延遲評估訊息（lazy evaluating message）的建立，僅在斷言失敗時才進行評估。這種技巧可以提高執行效能，避免不必要的訊息建立和字串連接操作，特別是當訊息建立的過程很耗時或者涉及複雜的計算時。

以下是一個使用 Lazily evaluating message 的例子：

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import org.junit.jupiter.api.Test;

public class CalculatorTest {
    
    @Test
    public void testAddition() {
        Calculator calculator = new Calculator();
        int result = calculator.add(2, 2);
        assertEquals(5, result, () -> "計算結果錯誤: " + calculator.getLastOperation());
    }
}
```

在上述示例中，我們使用了 JUnit 5 的 `assertEquals` 斷言方法，並傳遞了一個 lambda 運算式作為第三個參數。該 lambda 運算式將在斷言失敗時才執行，用於動態建立訊息。

假設 `Calculator` 類具有一個 `getLastOperation` 方法，該方法將返回上一個計算操作的字串描述。在此示例中，如果斷言失敗（即計算結果不等於 5），則會執行 lambda 運算式，並將計算器的最後一個操作訊息連接到錯誤訊息中。

這種方式可以避免在每次斷言時都執行 `getLastOperation` 方法，而僅在斷言失敗時才執行，從而節省了不必要的計算和訊息建立操作。

值得注意的是，在這種情況下，使用 lambda 運算式可以確保訊息的建立只在必要時執行。此外，lambda 運算式也提供了一種延遲訊息建立的方式，即使訊息建立過程中出現異常，也不會影響測試方法的正確執行。

這是一個展示 Lazily evaluating message 技巧的例子，可以根據具體的測試場景和需求進行適當的應用。


### 測試不中斷

在 JUnit 中，`assertAll()` 方法是一個非常有用的斷言方法，它用於同時執行多個斷言，並在所有斷言完成後報告所有失敗的斷言。這對於進行多個相關斷言的測試非常有用，因為它可以讓你一次性檢查多個條件，而不需要分開處理每個斷言。

以下是 `assertAll()` 方法的應用示例：

```java
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;

public class CalculatorTest {
    
    void plusTest() {
        System.out.println("\t Here is plus test...");

        int actualValue = c.plus(1, 1);
        int expectedValue = 2;
        assertAll("plus",
                ()-> assertEquals(expectedValue, actualValue),
                ()-> assertEquals(1, c.plus(-1, 1)),
                ()-> assertEquals(5, c.plus(1, -1))
        );

    }
```

![](https://hackmd.io/_uploads/S14VZPKea.png)


在上述示例中，我們在 `testAddition()` 測試方法中使用了 `assertAll()` 方法，並傳遞了多個 lambda 運算式作為參數。每個 lambda 運算式都代表一個斷言，用於檢查特定的條件。

`assertAll()` 方法的第一個參數是一個描述性的名稱，用於識別這組相關的斷言。接下來的參數是多個 lambda 運算式，每個運算式都是一個斷言。

當執行 `assertAll()` 方法時，它將依次執行每個斷言。如果其中任何一個斷言失敗，將記錄錯誤訊息，並將所有失敗的斷言結果一起報告。這使得你可以一次檢查多個斷言，並且在所有斷言完成後一次性獲得所有失敗的結果。

這在以下情況下特別有用：
- 測試方法中有多個相關的斷言，並且你想一次檢查它們。
- 你想要避免在第一個斷言失敗後中止測試，並希望繼續檢查其他斷言。
- 你想要清晰地顯示所有失敗的斷言，而不是一個接一個地報告。

使用 `assertAll()` 方法可以使測試代碼更簡潔、可讀性更高，並提供

:question: `assertAll()` 和多行的 `assert()` 有何不同？

#### Exercise
:::success
==算成績==	傳入一個二維陣列，`{{90,100,70}, {40,50,60}}` 表示一群學生在 math, python, eng 三科的成績。回傳一個一維陣列，其一個元素是全班所有科目的平均，接下來 math, python, eng 的全班平均。
- 請設計測試案例進行測試
:::

:::success
==找出健康的== 輸入一個一維陣列，裡面放一群 `Person` 的物件，回傳一個一維陣列，記載健康的 `Person`。`Person` 內有身高體中，並依此可以算出 `bmi`。如果 `bmi` 的值在 `18-24` 之間，則為健康。
- 請設計測試案例進行測試
- Hint: assertArrayEquals(Object[] expected, Object[] actual)
:::


### 斷言會有例外的產生

See [AssertionDemo](https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions)
* 用 `assertThrows()` 來斷言例外的發生，而不是讓測試方法拋出例外。

```java
@Test
void exceptionTesting() {
    Exception exception = assertThrows(ArithmeticException.class, () ->
    calculator.divide(1, 0));
    assertEquals("/ by zero", exception.getMessage());
}
```    

### 效能測試

在 JUnit 中，`assertTimeout()` 方法用於測試執行時間，它允許你在指定的時間範圍內執行一個測試，如果超過該時間範圍，則將其視為失敗。

以下是 `assertTimeout()` 方法的應用與示例：

應用：
- 測試長時間執行的程式碼，以確保其在預期的時間範圍內完成。
- 檢查程式碼中是否存在性能問題或無限迴圈。
- 測試多執行緒程式碼的正確性。

示例：
```java
import static org.junit.jupiter.api.Assertions.*;

import org.junit.jupiter.api.Test;

public class MyServiceTest {

    @Test
    public void testLongRunningOperation() {
        MyService myService = new MyService();

        assertTimeout(Duration.ofSeconds(5), () -> {
            String result = myService.longRunningOperation();
            assertEquals("Expected Result", result);
        });
    }
}
```

在上述示例中，我們使用了 `assertTimeout()` 方法來測試 `MyService` 類中的 `longRunningOperation()` 方法。我們希望該方法在5秒內完成並返回期望的結果。

`assertTimeout()` 方法的第一個參數是一個 `Duration` 物件，用於指定允許的最大執行時間。在這個例子中，我們設定為5秒。

第二個參數是一個 `Executable` 物件，它是一個表示要執行的程式碼塊的 lambda 運算式。在這個例子中，我們使用 lambda 運算式來執行 `longRunningOperation()` 方法，並在允許的時間範圍內進行斷言，驗證結果是否與期望相符。

如果 `longRunningOperation()` 方法在5秒內成功完成並返回預期結果，則測試通過。否則，如果超過了指定的時間範圍，則測試將被視為失敗。

使用 `assertTimeout()` 方法可以確保測試的執行時間在合理的範圍內，從而提供對程式碼執行時間的控制和驗證。這對於測試具有時間相關性或執行時間長的程式碼非常有用。




## 進階內容

### 測試結果之顯示 


See [DisplayNameDemo](https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-names)
* 可以讓測試的結果顯示更清楚，口語化
* 在 testing class 或 testing method 上面加上 `@DisplayName()` 的標記
* 需 `import org.junit.jupiter.api.DisplayName;`

```java
@DisplayName("A special test case")
class DisplayNameDemo {

    @Test
    @DisplayName("Custom test name containing spaces")
    void testWithDisplayNameContainingSpaces() {
    }
```

#### 進階：display name 的自動產生

See [DisplayNameGeneratorDemo](https://junit.org/junit5/docs/current/user-guide/#writing-tests-display-name-generator)
* DisplayNameGenerator 共有四種
    * Standard: Matches the standard display name generation behavior in place since JUnit Jupiter 5.0 was released. (?)
    * Simple: 以方法的名稱顯示，但會去掉參數與括號 (?)
    * **ReplaceUnderscores**: 以空白取代底線
    * **IndicativeSentences**: 可包含類別和測試方法的描述，並自訂兩者之間的連接。


![](https://hackmd.io/_uploads/S1xiM7nZa.png)
* `A_year_is_not_supported` 是採用去除底線的方式，直接將方法名稱作為顯示，其內的 testing method 也都會採用此方法。
* `if_it_is_negative` 因為有特別宣告 DisplayName, 所以會採用其後的參數來描述。注意因為此方法用參數化測試, `-1`, `-4` 都會進去測試，每次測試的顯示會依據 name 的內容而定 `{0}` 表示第一個參數，此為 `year`。

```java
@DisplayName("A negative value for year is not supported by the leap year computation.")
@ParameterizedTest(name = "For example, year {0} is not supported.")
@ValueSource(ints = { -1, -4 })
void if_it_is_negative(int year) {
}
```

### 巢狀測試

巢狀測試 (Nested test)

See [TestingAStackDemo](https://junit.org/junit5/docs/current/user-guide/#writing-tests-nested)

* class 內部有 class 形成結構，更方便組織測試
* 內部的 class name 上方加上 `@Nested`; 才會進行測試
* class `TestingAStackDemo` 下有 `WhenNew` class, `WhenNew` 下有 `AfterPushing` class, 他們下面有一些 testing method 會進行測試。
* 與 DisplayName 搭配，測試的報告更清楚。

class structure
![class hierarchy](https://hackmd.io/_uploads/r1rxcMh-p.png)

Result result:
![nest testing](https://hackmd.io/_uploads/r1_idz3W6.png)

:::success
==三角形面積== 輸入三個邊長，判斷是否 (1) 符合三角形規範 (2) 正三角形 (3) 等腰三角形 (4) 直角三角形 (5) 等腰直角三角形。
- 請透過 Nested 與 DisplayNames 來組織測試案例。
:::

### 參數化測試

See [ParameterizedTest doc](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests)

`@ParameterizedTest` 是 JUnit 5 中的一個注解，用於執行參數化測試（Parameterized Tests）。它允許你在一個測試方法中使用不同的輸入參數運行多次測試，以驗證相同的測試邏輯對於不同的輸入產生正確的結果。

以下是 `@ParameterizedTest` 的應用示例：

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

public class CalculatorTest {

    @ParameterizedTest
    @ValueSource(ints = {1, 2, 3, 4, 5})
    public void testAddition(int number) {
        Calculator calculator = new Calculator();
        int result = calculator.add(number, 2);
        assertEquals(number + 2, result);
    }
}
```

在上述示例中，我們使用了 `@ParameterizedTest` 注解來標記 `testAddition()` 測試方法。該方法接受一個整數參數 `number`。

使用 `@ValueSource` 注解，我們指定了整數陣列 `{1, 2, 3, 4, 5}` 作為輸入參數。這意味著 `testAddition()` 方法將使用這個陣列中的每個整數值來運行測試。

在每次執行時，`number` 參數將被設置為陣列中的一個值，並在測試方法內進行驗證。在這個例子中，我們測試了將每個 `number` 與 2 相加的結果是否等於 `number + 2`。

這樣一來，`testAddition()` 方法將以 `{1, 2, 3, 4, 5}` 中的每個數字運行五次，並在每次運行時驗證測試邏輯。

使用 `@ParameterizedTest` 可以有效地減少重複的測試程式碼，並使你能夠輕鬆地在一個測試方法中執行多次測試。這對於驗證一個邏輯對於不同輸入值的行為是否正確非常有用，同時保持測試代碼的簡潔和可讀性。

除了 `@ValueSource`，JUnit 5 還提供了其他參數提供者（Parameter Providers）來更靈活地指定參數值，例如 `@CsvSource`、`@MethodSource`、`@ArgumentsSource` 等。

- `@NullSource` null 的資料
- `@EmptySource` 空的資料
- `ValueSource` int, string 等資料
- `@EnumSource` 列舉型資料
- `CsvSource` csv 格式的資料
- `CsvSourceFile` 讀取 csv file
- `MethodSource` 透過實踐某介面的方法回傳的資料

#### test tomorrow
以下範例是對 `tomorrow()` 進行測試， csv 內部前三個參數是輸入的日期，後三個數字是預期的輸出。輸出的結果我們都轉為 String 一次比較年月日是否相同。

```java
@ParameterizedTest
@CsvSource({
    "1901, 1, 1, 1901, 1, 2",
    "1901, 2, 28, 1901, 3, 1",
    "2000, 2, 28, 2000, 2, 29",
    "2000, 12, 31, 2001, 1, 1",
    "2000, 6, 30, 2000, 7, 1",
    "2000, 7, 30, 2000, 7, 31",
})
void testTomorrow(int y1, int m1, int d1, int y2, int m2, int d2) {
    MyDate today = new MyDate(y1, m1, d1);
    MyDate expected_tomorrow = new MyDate(y2, m2, d2);
    assertEquals(expected_tomorrow.toString(), today.tomorrow().toString());
}
```
*  變數 y1, m1, d1 分別對應到 1901, 1, 1,; y2, m2, d2 分別對應到 1901, 1, 2
*  逐行讀入進行測試

test by using csv file

```java=
public class MyParaTest {
    @ParameterizedTest
    @CsvFileSource(resources = "/data.csv", numLinesToSkip = 1)
    void testWithCsvFileSourceFromClasspath(int a, int b, int r) {
        Calculator c = new Calculator();
        assertEquals(c.add(a, b), r);
    }
}
```

* .csv 檔案放在 /resource 下
* 跳過第一行: numLinesToSkip = 1

![](https://hackmd.io/_uploads/SkhM5AEza.png)

#### 參數化測試優點

1. 減少重複的測試程式碼：使用 @ParameterizedTest 可以將多次類似的測試結合到一個測試方法中，減少了重複編寫相似測試邏輯的需要。這樣一來，可以提高測試代碼的可維護性和可讀性。
2. 一次性測試多個輸入值：通過參數化測試，你可以一次性測試多個輸入值。這使得可以在一個測試方法中檢查多個輸入組合下的預期行為，從而增加了測試的全面性和可靠性。
3. 提高測試覆蓋率：使用 @ParameterizedTest 可以為各種輸入條件和邊界情況設置不同的測試案例，從而增加測試覆蓋率。這有助於捕捉潛在的錯誤或不正確的行為，並提高程式碼的品質。
4. 測試報告和結果的可讀性：由於 @ParameterizedTest 將多個測試組合在一起，測試報告和結果將更清晰和易讀。它將以一個測試方法的形式呈現每個輸入組合的結果，使得報告更容易理解並追踪測試結果。
5. 擴展性和靈活性：JUnit 5 提供了多種參數提供者（Parameter Providers），如 @ValueSource、@CsvSource、@MethodSource 等。這使得可以靈活地指定不同的參數值來驗證測試邏輯，並根據需要擴展和定制參數化測試。

總之，@ParameterizedTest 提供了一種有效的方式來處理多個輸入組合下的測試情境，減少了重複的測試程式碼，提高了測試覆蓋率和測試結果的可讀性。它還提供了靈活的擴展性，以適應各種不同的參數化測試需求。

#### Exercise

:::success
==BMI==	輸入身高體重，輸出 BMI，`bmi = h/(w**2)`
- 使用參數化測試進行測試
:::

:::success
==星期幾==	2021/1/1 是星期五，輸入月份和日期（一樣是2021年），輸出是星期幾。
1. 使用參數化測試進行測試 (CsvSource)
2. 使用 CsvSourceFile 進行測試
:::

:::success
==貨幣== 一個 Currency 的類別，內封裝金額與幣值的屬性，幣值可以有 NT 和 US 兩種幣值，其匯率為 30。如果是台幣+美金則回傳台幣，若美金+台幣則回傳為美金（由第一個決定），請設計程式與測試案例。
- 使用參數化測試進行測試

```java=
class Currency {
    int amount;
    String symbol;
    public Currency(int a, String s) {
        ...
    }
    public Currency add(Currency other) {
        ...
    }    
}

Currency nt100 = new Currency(100, "NT");
Currency us100 = new Currency(100, "US");
...
```
:::

:::success
==明天==	寫一函式，輸入是一個 `y-m-d` 的日期(2023-01-02)，輸出同樣為 `y-m-d` 的格式
- 使用參數化測試進行測試
:::




### 假設

在JUnit 5中，Assumptions（前提條件）是一種用於定義測試前提條件的機制。它允許你在執行測試之前檢查某些條件，如果條件不滿足，則可以將測試視為已經通過，跳過執行測試。這在某些情況下非常有用，例如當測試需要特定的環境或資源時，如果條件不滿足，測試就沒有意義。

JUnit 5中Assumptions類提供了多個靜態方法來執行前提條件檢查，以下是其中一些重要的方法：

1. `assumeTrue(boolean condition)`：如果條件為true，則測試繼續執行；如果條件為false，則測試被視為已通過並跳過執行。

```java
@Test
void testSomething() {
    assumeTrue(isEnvironmentReady()); // 假設環境已準備好
    // 執行測試邏輯
}
```

2. `assumeFalse(boolean condition)`：如果條件為false，則測試繼續執行；如果條件為true，則測試被視為已通過並跳過執行。

```java
@Test
void testSomething() {
    assumeFalse(isEnvironmentInvalid()); // 假設環境無效
    // 執行測試邏輯
}
```

3. `assumingThat(boolean assumption, Executable executable)`：在滿足特定前提條件時執行測試邏輯，否則將測試視為已通過並跳過執行。

```java
@Test
void testSomething() {
    assumingThat(isInDevelopmentMode(), () -> {
        // 只有在開發模式下才執行這段測試邏輯
    });
    // 其他測試邏輯
}
```

在上述示例中，使用Assumptions可以在測試執行之前進行條件檢查。如果條件滿足，測試將繼續執行；如果條件不滿足，測試將被視為已通過並跳過執行。這有助於確保測試在正確的環境或條件下執行，並減少無效或不必要的測試執行。

Assumptions的使用方法能夠提供更靈活的控制，可以根據特定的前提條件來選擇是否執行測試，從而更好地管理和組織測試案例。

## 練習

- 以下何者為真？（多選）	
	- @Test 用以標記一個測試碼;
	- @Before 用以標記測試執行前的設定與準備，多個測試僅會執行一次;
	- 僅針對有回傳值的方法進行測試，無回傳的方法則無從檢驗;
	- assertEquals() 針對預期值與真實質進行比對，對於有小數誤差的計算則無從檢驗;
	- @Exception 用以標記該測試預期會拋出例外，用以檢驗是否正確拋出例外。
	- 在 JUnit 中 @After 表示每一個@Test 測試後要執行的程式。		
	
- 以下 JUnit 測試碼執行後會輸出什麼？
```java
public class AdditionTest {
 @Test
 public void test1() {
    Addition a = new Addition();
    assertEquals(12, a.add(3,9));
    println("A");
 }	
 @Test
 public void test2() {
    Addition a = new Addition();
    assertEquals(24, a.add(12,12));
    println("B");
  }	
 @BeforeClass
  public static void beforeClass() {
    println("C");
  }	
 @Before
  public void before() {
    println("D");
  }	
 @After
 public void after () {
   println("E");
 }
 @AfterClass
 public static void afterclass() {
   println("F");
 }
}
```
- 全班分為兩大群：測試群與程式群，依據以下題目做平行開發與測試，採用測試先行的方式。
	- 加法 int add(int, int) 
	- 最大公因數 int gcd(int, int) 
	- 圓面積 double area(double) 	
	
- 寫一個程式來依據 BMI 檢驗體重是否過重，並用 JUnit 測試之。其分級如下： 
    - 體重過輕 BMI ＜ 18.5 
    - 正常範圍 18.5 <= BMI ＜24 
    - 過重 24 <= BMI ＜ 27 
    - 輕度肥胖 27 <= BMI ＜ 30 
    - 中度肥胖 30 <= BMI ＜ 35 
    - 重度肥胖 BMI >= 35 
- 寫一個程式判斷三角形。輸入的參數是三邊的長，若不符合三角形定義（任兩邊和大於第三邊）則會拋出例外。否則回傳 "一般三角形"、"等腰三角形"、"正三角形"等。開發之前，請先建立 JUnit 測試案例，並使用 @Test(expected=...) 的方式來測試例外。
- 針對一個排序程式設計測試案例 SortTest，每一次測試之前會先做一些初始化：從檔案中讀取資料，寫到陣列 data[]中，SortTest 中的 testSort() 再針對 data[] 中的資料做排序。請利用 @Before 來完成此工作。
- 寫一個無窮迴圈的程式，並使用 junit 來測試。利用 timeout 的參數來跳出迴圈。

#### mid-test-112-1
:::success
112-1 期中考題
:football: Q1 MLB 世界大賽票價 (50%)

MLB 世界大賽的票價嚇人，起碼 2 萬元起跳。假設規則如下：(1) 一般票價 20,000 (2) 比賽當天若為六日，則價格為 25,000 (3) 如果是內野票價，比上述票價再高 5,000 元; 貴賓席則貴 15,000 元。(4) 如果透過 Ticketmaster 購買可以打九折。

- 請以等價分割的「強涵蓋」設計測試案例，以表格的方式描述測試案例。
- 撰寫程式碼並用JUnit 進行完整測試，並說明測試結果與你的完成度。
:::

:::success
112-1 期中考題
:football: Q2 德州遊騎兵 (50%)

2023 美國棒球大聯盟 MLB 落幕，恭喜德州遊騎兵打敗亞利桑那響尾蛇，拿到隊史成軍 63 年以來第一次的世界大賽冠軍。 `int score(inningA[], inningB[], playerA[], playerB[])` 會回傳 A 隊勝 B 隊的分數，其中：
- `inningA[]`, `inningB[]` 分別紀錄各局的得分。原則上是打滿九局，但如果九上結束後攻者已經領先前攻者，則不需進行九下，分數以 -1 或 X 標記（不可標記為 0)。若九局結束仍然平分，則繼續進行第十局直到勝負。請檢查這兩個資料是否符合常規，若否則拋出例外。
- `playerA[]`, `playerB[]` 分別紀錄兩隊隊員的得分，A 隊隊員得分之總和應與 `inningA[]` 之個局之總和相同，依此類推。若不符合常規則拋出例外。
- 若資料檢查無誤，則回傳 A 隊勝 B 隊的分數。若為負數表示 A 隊輸，反之則 A 隊贏。不可能為零。
- 撰寫程式碼並用 JUnit 進行完整測試，並說明測試結果與你的完成度。
:::

Hint

- 九局正常結束（沒有提前）
    - inningA, inningB, playerA, playerB
    - [1,1,1,1,1,1,1,1][1,1,1,1,1,1,1,2] [2,0,1,1,1,1,0,2] [1,1,3,0,0,1,1,2] => -1 B win
    - [1,1,1,1,1,1,1,2][1,1,1,1,1,1,1,1] [1,1,3,0,0,1,1,2] [2,0,1,1,1,1,0,2] => 1 A win

- 至少九局，小於九局就拋出例外
    - inningA[] = [1,2,3]; inningB[] = [3,4,5] ⇒ Exception(“局數小於九局”)

- 九局有提前結束 (分數為 X)
    - 如果第 9 局有 X 分數者，則該隊前 8 局的分數和大於另一對的 9 局分數和; 且 X 分者，必定為後攻球隊
    - [1,1,1,1,1,1,1,1,1]  [1,1,1,1,1,1,1,3,X]⇒ -1 (差分 1; B win)
    - [1,1,1,1,1,1,1,1,1]  [1,1,1,1,1,1,1,1,X]⇒ Exception(“不合理的提前結束”)

- 延長
    - 合理正常的延長
        - [1,1,1,1,1,1,1,1,2][1,1,1,1,1,1,1,1,3] ⇒ -1 B win, 延長到 10局
        - [1,1,1,1,1,1,1,1,3][1,1,1,1,1,1,1,1,2] ⇒  1 A win, 延長到 10局
    - 無必要的延長
        - [1,1,1,1,1,1,1,2,2][1,1,1,1,1,1,1,1,3] => Exception(“沒有必要的延長局”); 九局時 A win
        - 延長局必定上下局都會打
[1,1,1,1,1,1,1,1,1,2] [1,1,1,1,1,1,1,1,1,X]⇒ Exception(“提前結束只可能出現在九下”)

- 所有全員的總得分 = 該隊為各局的總得分 
    - [1,1,1,1,1,1,1,2][1,1,1,1,1,1,1,1] => 
[1,1,3,0,0,1,1,2] [2,0,1,1,1,1,0,2] 
⇒ A隊的總分為 10, 但Ａ隊全員的總得分只有 9 => Exception(“總分不一致”)

- 必須分出勝負 （差分 !=0)
    - [1,1,1,1,1,1,1,1][1,1,1,1,1,1,1,1] => Exception(不可以和局)


