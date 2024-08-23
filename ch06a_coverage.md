
JUnit coverage testing
===

## Testing coverage in Intellij

:::success
設計一個 tomorrow 的 function, 並設計測試案例已測試之。呈現出測試涵蓋度，並逐步的新增測試案例以提升涵蓋度。
:::
```java=
package nick.example;

import org.apache.commons.lang3.StringUtils;

public class Date {
    private int y;
    private int m;
    private int d;

    private static final int[] DAYS_IN_MONTH = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    public Date(int y, int m, int d) {
        this.y = y;
        this.m = m;
        this.d = d;
    }

    public int getYear() {
        return y;
    }

    public int getMonth() {
        return m;
    }

    public int getDay() {
        return d;
    }

    public Date tomorrow() {
        int year = y;
        int month = m;
        int day = d + 1;

        int daysInCurrentMonth = daysInMonth(month, isLeapYear(year));

        if (day > daysInCurrentMonth) {
            day = 1;
            month++;
            if (month > 12) {
                month = 1;
                year++;
            }
        }

        return new Date(year, month, day);
    }

    private int daysInMonth(int month, boolean isLeapYear) {
        if (month == 2 && isLeapYear) {
            return 29;
        } else {
            return DAYS_IN_MONTH[month];
        }
    }

    private boolean isLeapYear(int year) {
        return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
    }

    public String toString() {
        String[] r = {String.valueOf(y), String.valueOf(m), String.valueOf(d)};
        return StringUtils.join(r, "-");
    }
}
```

Remember to set the dependency and import the class `StringUtils` in this project

```
    <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
    <dependencies>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.0</version>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <version>5.9.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

Run the coverage testing
<img src=https://hackmd.io/_uploads/SktTMtyET.png width=450>)

You can see uncovered code
<img src=https://hackmd.io/_uploads/H1RfEF14T.png width=530>

## Maven lifecycle and Jacoco report

- Run the `maven test`, it will generate the report to Target/surefire-report
- In this report, you can see what is wrong in your code (and and test)

![image](https://hackmd.io/_uploads/S1yS_o14a.png)


## Jacoco report

把 Jacoco 的外掛加進來：

```
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0</version>
            </plugin>

            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <version>0.8.7</version>
                <configuration>
                    <excludes>
                        <exclude>org/example/Main.class</exclude>
                    </excludes>
                </configuration>

                <executions>
                    <execution>
                        <id>prepare-agent</id>
                        <goals>
                            <goal>prepare-agent</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>report</id>
                        <phase>test</phase>
                        <goals>
                            <goal>report</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```
<br><br>
打開 Maven > Plugins > jacoco > jacoco:report

<img src=https://hackmd.io/_uploads/H1P_LiJ4T.png width=500>

Run the jacoco:report

<img src=https://hackmd.io/_uploads/SyKVvnyEp.png width=480>

<br><br>
打開 target 的目錄，site 下的 jacoco

<img src=https://hackmd.io/_uploads/SkRGP2JVp.png width=450>

<br><br>

點擊 index.html, 打開瀏覽器就會看到測試報告如下：

<img src=https://hackmd.io/_uploads/SJpcwny4p.png width=800>


<br><br>
點擊就可以看到報告
* 紅色：沒有測試碼跑過該敘述
* 黃色：並不是所有branch 都經歷過
* 綠色：所有 branch 都經歷過，指令都測試過


<img src=https://hackmd.io/_uploads/S1aKqn1V6.png width=450>

---
## Exercise

:::success
:baseball::trophy: 德州遊騎兵奪冠
2023 美國棒球大聯盟 MLB 落幕，恭喜德州遊騎兵打敗亞利桑那響尾蛇，拿到隊史成軍 63 年以來第一次的世界大賽冠軍。 `int score(inningA[], inningB[], playerA[], player[])` 會回傳 A 隊勝 B 隊的分數，其中：

- `inningA[], inningB[]` 分別紀錄各局的得分。原則上是打滿九局，但如果九上結束後攻者已經領先前攻者，則不需進行九下，分數以 X 標記（不可標記為 0)。若九局結束仍然平分，則繼續進行第十局直到勝負。請檢查這兩個資料是否符合常規，若否則拋出例外。
- `playerA[]`, `playerB[]` 分別紀錄兩隊隊員的得分，A 隊隊員得分之總和應與 `inningA[]` 之個局之總和相同，依此類推。若不符合常規則拋出例外。每隊球員的人數必須大於等於 9。
- 若資料檢查無誤，則回傳 A 隊勝 B 隊的分數。若為負數表示 A 隊輸，反之則 A 隊贏。不可能為零。
- A 隊為先攻隊伍。
- 撰寫程式碼並用 JUnit 進行完整測試，並說明測試結果與你的完成度。
- 使用 coverage testing 改善你的測試涵蓋度。
- 說明第一次測試通過時的測試涵蓋度，及改善的過程。
:::


#### ex-all-pair-coverage

:::success
:football: 全成對之涵蓋度
針對 [大聯盟的票價](https://hackmd.io/eEvhtfkMQ5244UTOKYWMIQ#mid-test-112-1)，請採用全成對的測試策略進行測試
*  先以 excel 繪製測試案例設計
*  寫出測試碼
*  請檢驗測試案例之涵蓋度為何？
*  若涵蓋度不高，則補充測試案例直到百分百，並探討提升率為何
:::

#### ex-coverage
:::success
:football: 涵蓋度
針對以下程式：

```java=
public class Coverage {

    public int func(int x, int y) {
        if ((x > 10) && (y == 1))
            x = 1;
        else if ((x - y) < 2)
            x = 2;
        if (y > 10)
            x = 3;
        return x;
    }
}
```
1. 設計一測試案例達到百分百敘述涵蓋度 (SC100); 
2. 設計一測試案例達到百分百條件涵蓋度 (CC100)，但非百分百分支涵蓋度; 
3. 設計一測試案例達到百分百分支涵蓋度 (BC100)，但非百分百條件涵蓋度 (!CC100)。

:::

Hint:
```java=
class CoverageTest {

    @Disabled
    @Test
    void func_q_1() {
        // statement coverage 100
        Coverage c = new Coverage();
        int r = c.func(11, 1);
        assertEquals(1, r);

        r = c.func(0, 2);
        assertEquals(2, r);

        r = c.func(0, 11);
        assertEquals(3, r);
    }

    @Disabled
    @Test
    void func_q_2() {
        // 設計一測試案例達到百分百條件涵蓋度 (CC100)，
        // 但非百分百分支涵蓋度;
        Coverage c = new Coverage();
        // B1: TF, F
        // B2: F
        // B3: T
        int r = c.func(15, 11);
        assertEquals(3, r);

        // B1: FT, F
        // B2: T
        // B3: F
        r = c.func(0, 1);   //B2:
        assertEquals(2, r);
    }
    @Test
    void func_q_3() {

        Coverage c = new Coverage();
        // B1: TT, T
        // B2: F
        // B3: F
        int r = c.func(15, 1);
        assertEquals(1, r);

        // B1: FT, F
        // B2: T
        // B3: F
        r = c.func(0, 1);   //B2:
        assertEquals(2, r);
    }
```
    