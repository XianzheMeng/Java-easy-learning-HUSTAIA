# Java 期末考试速查大全（开卷）

一个文件搞定。上半部分=编程题模板（抄+改）；下半部分=改错题手册（查毛病）。
编程题按【编程目录】定位抄模板；改错题按【改错目录】定位查错因。

═══════════════════════════════════════════════

# 第一部分　编程题模板（抄了改）

═══════════════════════════════════════════════

## 编程目录

- **模板1** 文件读取（逐行 / 边读边算 / 多数字一行 / Scanner / 混合内容 / 统计）
- **模板2** 文件写入（逐行 / 列表 / 文本 / 追加 / BufferedWriter）
- **模板3** 读+排序+写（组合题）
- **模板4** JDBC 数据库（连接/增删改查）
- **模板5** Swing GUI（5步骤 / Look&Feel / 三种加组件 / 五种布局 / 按钮弹窗）
- **模板6** 事件处理（implements / 多监听器 / Adapter / 内部类匿名类）
- **模板7** 多线程（Thread / Runnable）
- **模板8** 常用小工具（异常、集合、字符串）

## 模板1　文件读取

### 1A 逐行读 + 转数字 + 排序输出（最经典）

```java
import java.io.*;
import java.util.*;
public class ReadFile { // ★类名=文件名
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        try {
            BufferedReader br = new BufferedReader(
                new FileReader("D:/Sort.txt")); // ★文件路径
            String line;
            while ((line = br.readLine()) != null) { // 逐行读,读到null结束
                if (!line.trim().isEmpty()) {
                    list.add(Integer.parseInt(line.trim())); // 字符串转整数
                }
            }
            br.close();
            Collections.sort(list); // 升序排序
            // 降序: Collections.sort(list, Collections.reverseOrder());
            for (int num : list) { // 输出
                System.out.println(num);
            }
        } catch (IOException e) { // ★try必须配catch
            e.printStackTrace();
        }
    }
}
```

> ⚠ try 和 catch 必须成对！文件操作不写 try-catch 编译不过。

### 1B 边读边算（求和 / 最大 / 平均）

```java
BufferedReader br = new BufferedReader(new FileReader("D:/in.txt"));
String line;
int sum = 0, count = 0, max = Integer.MIN_VALUE;
while ((line = br.readLine()) != null) {
    if (!line.trim().isEmpty()) {
        int n = Integer.parseInt(line.trim());
        sum += n; count++; // 累加、计数
        if (n > max) max = n; // 找最大
    }
}
br.close();
System.out.println("和="+sum+" 最大="+max+" 平均="+(double)sum/count);
```

### 1C 一行有多个数字（空格/逗号分隔）

```java
BufferedReader br = new BufferedReader(new FileReader("D:/in.txt"));
String line;
while ((line = br.readLine()) != null) {
    String[] parts = line.split(" "); // ★空格拆;逗号用 split(",")
    for (String p : parts) {
        if (!p.trim().isEmpty())
            System.out.println(Integer.parseInt(p.trim()));
    }
}
br.close();
```

### 1D 用 Scanner 读（直接读数字，不用 parseInt）

```java
import java.util.Scanner;
import java.io.File;
try {
    Scanner sc = new Scanner(new File("D:/in.txt"));
    while (sc.hasNextInt()) { // 还有整数就继续
        int n = sc.nextInt(); // 直接读int
        System.out.println(n);
    }
    sc.close();
} catch (Exception e) { // Scanner读文件catch这个
    e.printStackTrace();
}
// 读整行: while(sc.hasNextLine()){ String s = sc.nextLine(); }
```

注意：Scanner 读文件的异常是 FileNotFoundException，用 Exception 兜住最省事。

### 1E 混合内容（每行"姓名 年龄"这种，拆开存）

```java
BufferedReader br = new BufferedReader(new FileReader("D:/data.txt"));
String line;
while ((line = br.readLine()) != null) {
    String[] parts = line.split(" "); // 按空格拆
    String name = parts[0]; // 第1个:姓名
    int age = Integer.parseInt(parts[1]); // 第2个:年龄
    System.out.println(name + " 年龄 " + age);
}
br.close();
```

### 1F 统计行数 / 单词数

```java
BufferedReader br = new BufferedReader(new FileReader("D:/in.txt"));
String line;
int lineCount = 0, wordCount = 0;
while ((line = br.readLine()) != null) {
    lineCount++;
    if (!line.trim().isEmpty())
        wordCount += line.split("\\s+").length; // 按空白拆算单词
}
br.close();
System.out.println("行数="+lineCount+" 单词数="+wordCount);
```

## 模板2　文件写入

### 2A 逐行写数字（最常考）

```java
import java.io.*;
public class WriteNumbers {
    public static void main(String[] args) {
        int[] nums = {35, 12, 88, 7, 49}; // ★要写的数字
        try {
            PrintWriter pw = new PrintWriter(
                new FileWriter("D:/out.txt")); // ★输出路径
            for (int n : nums) {
                pw.println(n); // 一行一个,println自动换行
            }
            pw.close(); // 必须close,否则可能没写进去
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

> ⚠ 写数字用 `println(n)` 自动换行；千万别用 `write(n)`（会把数字当字符编码写，出乱码）。

### 2B 从 ArrayList 写

```java
PrintWriter pw = new PrintWriter(new FileWriter("D:/out.txt"));
for (int n : list) pw.println(n);
pw.close();
```

### 2C 写文本字符串

```java
PrintWriter pw = new PrintWriter(new FileWriter("D:/out.txt"));
pw.println("第一行");
pw.println("第二行");
pw.close();
```

### 2D 追加写（不覆盖，在末尾加）

```java
// FileWriter 第二个参数 true = 追加;不写或false = 覆盖
PrintWriter pw = new PrintWriter(new FileWriter("D:/out.txt", true));
pw.println("追加的一行");
pw.close();
```

### 2E 用 BufferedWriter 写

```java
BufferedWriter bw = new BufferedWriter(new FileWriter("D:/out.txt"));
for (int n : nums) {
    bw.write(String.valueOf(n)); // write要写字符串,数字先转String
    bw.newLine(); // 手动换行
}
bw.close();
```

## 模板3　读 + 排序 + 写（组合题，高频）

```java
import java.io.*;
import java.util.*;
public class SortAndWrite {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        try {
            // 1.读
            BufferedReader br = new BufferedReader(new FileReader("D:/in.txt"));
            String line;
            while ((line = br.readLine()) != null) {
                if (!line.trim().isEmpty())
                    list.add(Integer.parseInt(line.trim()));
            }
            br.close();
            // 2.排序
            Collections.sort(list);
            // 3.写
            PrintWriter pw = new PrintWriter(new FileWriter("D:/out.txt"));
            for (int n : list) pw.println(n);
            pw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 模板4　JDBC 数据库（骨架死记）

六步：加载驱动 → 连接 → 建Statement → 执行SQL → 处理结果 → 关闭。

```java
import java.sql.*;
public class TestDB {
    public static void main(String[] args) {
        try {
            // 1.加载驱动
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver"); // ★驱动(见表)
            // 2.连接: jdbc:类型://IP:端口;databaseName=库名
            String url = "jdbc:sqlserver://127.0.0.1:3130;databaseName=HUB"; // ★IP/端口/库
            Connection conn = DriverManager.getConnection(url, "Hust", "Hust"); // ★用户,密码
            Statement stmt = conn.createStatement();
            // 3.插入(增删改用 executeUpdate)
            String insert = "INSERT INTO Student(sCode, sName, sPenguin) " +
                "VALUES('20230001', '你的名字', '你的QQ')"; // ★表/字段/值
            stmt.executeUpdate(insert);
            // 4.查询(用 executeQuery 返回 ResultSet)
            ResultSet rs = stmt.executeQuery("SELECT * FROM Student"); // ★表名
            // 5.遍历输出
            while (rs.next()) {
                System.out.println(
                    rs.getString("sCode") + " " + // ★列名
                    rs.getString("sName") + " " +
                    rs.getString("sPenguin"));
            }
            // 6.关闭
            rs.close(); stmt.close(); conn.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 各数据库驱动名 + URL（替换第1、2步）

| 数据库 | 驱动类名 | URL |
|---|---|---|
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` | `jdbc:sqlserver://IP:端口;databaseName=库名` |
| MySQL | `com.mysql.cj.jdbc.Driver` | `jdbc:mysql://IP:端口/库名` |
| Oracle | `oracle.jdbc.driver.OracleDriver` | `jdbc:oracle:thin:@IP:端口:库名` |

其它SQL（换掉第3步，都用 executeUpdate）：
- 更新：`UPDATE Student SET sName='新名' WHERE sCode='20230001'`
- 删除：`DELETE FROM Student WHERE sCode='20230001'`

死记骨架：`Class.forName(驱动)` → `getConnection(url,用户,密码)` → `createStatement()` → 增删改 `executeUpdate` / 查 `executeQuery` → `while(rs.next()){rs.getString("列")}` → `close`

## 模板5　Swing GUI（完全贴课件）

### 5-0 GUI 设计 5 步骤（课件核心）

1. 引入 Swing 包 `import javax.swing.*;`
2. 选 Look&Feel 外观（可选）
3. 创建并设置窗口容器 JFrame
4. 创建并添加组件
5. 显示窗口 `setVisible(true)`

### 5-1 最简 GUI（HelloWorld）

```java
import javax.swing.*;
public class HelloWorldSwing {
    public static void main(String[] args) {
        JFrame frame = new JFrame("HelloWorldSwing"); // ★标题
        JLabel label = new JLabel("Hello World!"); // ★标签内容
        frame.add(label);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(200, 70);
        frame.setVisible(true);
    }
}
```

### 5-2 设置 Look&Feel（课件强调：要 try-catch）

```java
try {
    UIManager.setLookAndFeel(
        "com.sun.java.swing.plaf.windows.WindowsLookAndFeel"); // Windows风格
    // 其它可选值:
    // UIManager.getCrossPlatformLookAndFeelClassName() // Java风格,不用try
    // UIManager.getSystemLookAndFeelClassName() // 当前系统风格,不用try
    // "javax.swing.plaf.metal.MetalLookAndFeel" // Java风格
} catch (Exception e) {
    e.printStackTrace();
}
// 也可在创建窗口前: JFrame.setDefaultLookAndFeelDecorated(true);
```

### 5-3 向 JFrame 加组件的三种方式（课件原文）

```java
// 方式1: 通过内容面板
frame.getContentPane().add(new JLabel("Hello!"));
// 方式2: 建JPanel装组件,再setContentPane
JPanel contentPane = new JPanel();
JButton b = new JButton("确定");
contentPane.add(b);
frame.setContentPane(contentPane);
// 方式3(JDK1.5+,最常用): 直接add,自动转给内容面板
frame.add(b);
```

### 模板5补充　五种布局管理器（课件每种都考）

改布局只需改 `setLayout(new XxxLayout(...))` 那一行。

#### A FlowLayout 流式（从左到右、自动换行；Panel默认）

```java
setLayout(new FlowLayout()); // 居中(默认)
// setLayout(new FlowLayout(FlowLayout.LEFT, 10, 5)); // 左对齐,水平间距10,垂直5
add(new JButton("Button 1"));
add(new JButton("Button 2"));
```

对齐参数：FlowLayout.LEFT / CENTER / RIGHT（缺省居中）。

#### B BorderLayout 边界（5个区；Frame默认）

```java
setLayout(new BorderLayout());
add(new JButton("North"), "North"); // 上
add(new JButton("South"), "South"); // 下
add(new JButton("East"), "East"); // 右
add(new JButton("West"), "West"); // 左
add(new JButton("Center"),"Center"); // 中
```

五个区：North/South/East/West/Center。

#### C GridLayout 网格（n行m列,等大;左到右上到下填充）

```java
setLayout(new GridLayout(3, 2)); // 3行2列
add(new JButton("1")); add(new JButton("2"));
add(new JButton("3")); add(new JButton("4"));
add(new JButton("5")); add(new JButton("6"));
// 行优先填充;rows或cols可填0表示任意数目;先满足行
```

#### D CardLayout 卡片（叠放,一次显示一张,可切换）

```java
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
public class CardDemo extends JFrame implements ActionListener {
    JPanel cards;
    CardLayout cl = new CardLayout();
    public CardDemo() {
        setLayout(new BorderLayout());
        JPanel cp = new JPanel();
        JButton bt = new JButton("卡片切换");
        bt.addActionListener(this); // 按钮注册监听
        cp.add(bt);
        add("North", cp);
        cards = new JPanel();
        cards.setLayout(cl); // cards用CardLayout
        JPanel p1 = new JPanel(); p1.add(new JButton("Button 1"));
        JPanel p2 = new JPanel(); p2.add(new JTextField("TextField", 20));
        cards.add("卡片1", p1);
        cards.add("卡片2", p2);
        add("Center", cards);
    }
    public void actionPerformed(ActionEvent e) {
        cl.next(cards); // 显示下一张卡片
        // 其它: cl.first/previous/last/show(cards,"名字")
    }
    public static void main(String[] args) {
        CardDemo w = new CardDemo();
        w.setSize(300,200);
        w.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        w.setVisible(true);
    }
}
```

#### E GridBagLayout / BoxLayout（课件只提名，了解即可）

- **GridBagLayout**：网格包，组件可不等大、可跨行跨列。
- **BoxLayout**：箱式，组件排成一行或一列（Swing新增）。

选哪个布局（课件原文）：
- 一行紧凑显示 → FlowLayout
- 大小相同成行成列 → GridLayout
- 充满容器空间 → BorderLayout 或 GridBagLayout

### 模板5补充2　按钮点击 + 弹窗（综合，应对GUI编程题）

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
public class MyGUI extends JFrame {
    public MyGUI() {
        setTitle("窗口标题"); // ★标题
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(null); // 绝对布局,手动摆位置
        String[] names = {"楼A", "楼B", "楼C"}; // ★按钮/楼名
        int[][] pos = {{50,50},{200,50},{50,150}}; // ★每个x,y
        for (int i = 0; i < names.length; i++) {
            JButton btn = new JButton(names[i]);
            btn.setBounds(pos[i][0], pos[i][1], 120, 60); // x,y,宽,高
            add(btn);
            btn.addActionListener(new ActionListener() { // 匿名类监听
                public void actionPerformed(ActionEvent e) {
                    JOptionPane.showMessageDialog(null, "表拦我！我要复学！！"); // ★弹窗文字
                }
            });
        }
        setVisible(true);
    }
    public static void main(String[] args) {
        new MyGUI();
    }
}
```

弹窗：`JOptionPane.showMessageDialog(null, "内容");`
课件其它对话框：showInputDialog(输入)、showConfirmDialog(确认)、showOptionDialog(选项)。
用 setBounds 手动摆位置时必须 setLayout(null)。

## 模板6　事件处理（课件重头戏，4种写法）

事件模型3要素：事件(Event)、事件源(组件)、监听器(Listener)。
机制：组件注册监听器 `addXxxListener(监听器)` → 事件发生 → 自动调监听器的方法。

### 写法1　让类自己实现监听器接口（implements）

```java
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
public class TestButton {
    public static void main(String[] args) {
        JFrame f = new JFrame("Test");
        f.setSize(200, 100);
        f.setLayout(new FlowLayout());
        f.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        JButton b = new JButton("Press Me!");
        b.addActionListener(new ButtonHandler()); // 注册监听器
        f.add(b);
        f.setVisible(true);
    }
}
// 单独的监听器类
class ButtonHandler implements ActionListener {
    public void actionPerformed(ActionEvent e) {
        System.out.println("被点击了");
        System.out.println("按钮文字:" + e.getActionCommand());
    }
}
```

### 写法2　多监听器 / 一个类实现多个监听接口

```java
// 一个类同时实现多个监听接口
public class TwoListener implements MouseMotionListener, MouseListener {
    JFrame f;
    JTextField tf;
    public void go() {
        f = new JFrame("Two Listener");
        tf = new JTextField(30);
        f.add(tf, BorderLayout.SOUTH);
        f.addMouseMotionListener(this); // 注册鼠标移动监听
        f.addMouseListener(this); // 注册鼠标点击监听
        f.setSize(300,200);
        f.setVisible(true);
    }
    // MouseMotionListener 的方法(2个)
    public void mouseDragged(MouseEvent e) {}
    public void mouseMoved(MouseEvent e) {}
    // MouseListener 的方法(5个,必须全写)
    public void mouseEntered(MouseEvent e) { tf.setText("鼠标进入"); }
    public void mouseClicked(MouseEvent e) {}
    public void mouseReleased(MouseEvent e) {}
    public void mousePressed(MouseEvent e) {}
    public void mouseExited(MouseEvent e) {}
    public static void main(String[] args) { new TwoListener().go(); }
}
```

implements 监听接口必须实现它的所有方法,一个不能少（这是改错题考点）。

### 写法3　适配器 Adapter（只重写需要的方法）

```java
// Adapter已把接口所有方法空实现,继承它只重写要的
public class MouseClickHandler extends MouseAdapter {
    public void mouseClicked(MouseEvent e) {
        System.out.println("点击坐标:" + e.getX() + "," + e.getY());
    }
    // 其它方法不用写,继承MouseAdapter的空实现
}
```

课件原文：用户定义的监听器只能继承一个适配器类，且继承后不能再继承其它类。

### 写法4　内部类 / 匿名类（最常用，避免多继承限制）

```java
// 匿名类:当场写一个没名字的监听器
public class MyGUI {
    public MyGUI() {
        JButton btn = new JButton("按钮");
        btn.addMouseListener(new MouseAdapter() { // 匿名类
            public void mouseClicked(MouseEvent e) {
                System.out.println("被点击");
            }
        }); // 结尾 });
    }
}
```

内部类方式：在类里定义内部类继承Adapter，既用Adapter又避开"主类已继承JFrame不能再继承"的限制。

### 常见事件、监听接口、方法对照

| 事件 | 监听接口 | 主要方法 |
|---|---|---|
| 按钮点击 ActionEvent | ActionListener | actionPerformed |
| 鼠标点击 MouseEvent | MouseListener | mouseClicked/Pressed/Released/Entered/Exited(5个) |
| 鼠标移动 MouseEvent | MouseMotionListener | mouseDragged/mouseMoved(2个) |
| 键盘 KeyEvent | KeyListener | keyPressed/keyReleased/keyTyped(3个) |
| 窗口 WindowEvent | WindowListener | windowClosing等(7个) |
| 选项 ItemEvent | ItemListener | itemStateChanged |

对应的 Adapter（方法多的接口才有）：MouseAdapter、KeyAdapter、WindowAdapter、MouseMotionAdapter。
（ActionListener 只1个方法，没有 Adapter。）

## 模板7　Swing 画图（Graphics 绘图）

关键：要点击的→用按钮；纯画的图形(线/矩形/形状)→重写 paintComponent 画。

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
public class DrawDemo extends JFrame {
    public DrawDemo() {
        setTitle("画图窗口");
        setSize(500, 350);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        JPanel panel = new JPanel() {
            protected void paintComponent(Graphics g) {
                super.paintComponent(g); // 必须第一行(清背景)
                g.setColor(Color.GRAY);
                g.fillRect(0, 150, 500, 25); // 实心矩形(如桥面): x,y,宽,高
                g.drawLine(0, 150, 500, 150); // 线段: x1,y1,x2,y2
                g.drawRect(60, 175, 12, 40); // 空心矩形(如桥墩)
                g.setColor(Color.BLACK);
                g.drawString("高架桥", 230, 145); // 写字: 内容,x,y
            }
        };
        panel.setLayout(null);
        String[] names = {"东九楼", "东一楼"}; // ★楼名(按钮)
        int[][] pos = {{60,40},{300,40}};
        for (int i = 0; i < names.length; i++) {
            JButton btn = new JButton(names[i]);
            btn.setBounds(pos[i][0], pos[i][1], 130, 70);
            panel.add(btn);
            btn.addActionListener(new ActionListener() {
                public void actionPerformed(ActionEvent e) {
                    JOptionPane.showMessageDialog(null, "表拦我！我要复学！！"); // ★弹窗文字
                }
            });
        }
        add(panel);
        setVisible(true);
    }
    public static void main(String[] args) {
        new DrawDemo();
    }
}
```

Graphics 画图方法：
- `g.setColor(Color.RED)` 设颜色(RED/BLUE/GRAY/BLACK/GREEN/YELLOW/WHITE)
- `g.fillRect(x,y,宽,高)` 实心矩形 / `g.drawRect(x,y,宽,高)` 空心矩形
- `g.drawLine(x1,y1,x2,y2)` 线段
- `g.fillOval(x,y,宽,高)` 实心椭圆 / `g.drawOval(...)` 空心椭圆
- `g.fillPolygon(int[] x, int[] y, int n)` 实心多边形 / `g.drawPolygon(...)` 空心
- `g.drawArc(x,y,宽,高,起始角,弧度)` 圆弧
- `g.drawString("字",x,y)` 写文字
- `g.setFont(new Font("宋体", Font.BOLD, 20))` 设字体

画图什么时候刷新：动态改了内容后调 `repaint()` 触发重绘。

## 模板8　多线程

### 写法A：继承 Thread

```java
public class MyThread extends Thread {
    private String name;
    public MyThread(String name) { this.name = name; }
    public void run() { // ★重写run
        for (int i = 0; i < 5; i++)
            System.out.println(name + ": " + i);
    }
    public static void main(String[] args) {
        new MyThread("线程A").start(); // 用start()不是run()!
        new MyThread("线程B").start();
    }
}
```

### 写法B：实现 Runnable

```java
public class MyRunnable implements Runnable {
    private String name;
    public MyRunnable(String name) { this.name = name; }
    public void run() { // ★实现run
        for (int i = 0; i < 5; i++)
            System.out.println(name + ": " + i);
    }
    public static void main(String[] args) {
        new Thread(new MyRunnable("线程A")).start();
        new Thread(new MyRunnable("线程B")).start();
    }
}
```

> ⚠ 启动用 `start()` 不是 `run()`！run()只是普通方法调用，不开新线程（改错题常考）。

## 模板8　常用小工具

集合 ArrayList：

```java
ArrayList<Integer> list = new ArrayList<>();
list.add(5);                                       // 加
list.get(0);                                       // 取第0个
list.size();                                       // 个数
list.remove(0);                                    // 删第0个
Collections.sort(list);                            // 升序
Collections.sort(list, Collections.reverseOrder());// 降序
Collections.max(list);                             // 最大
Collections.min(list);                             // 最小
```

字符串常用：

```java
s.length();                  // 长度
s.charAt(0);                 // 第0个字符
s.substring(1,3);            // 截取[1,3)
s.split(" ");                // 按空格拆成数组
s.equals(t);                 // 比内容(不能用==)
s.trim();                    // 去首尾空格
s.toUpperCase()/toLowerCase();// 大小写
Integer.parseInt(s);         // 字符串转int
String.valueOf(n);           // int转字符串
```

异常处理结构：

```java
try {
    // 可能出错的代码
} catch (异常类型 e) {
    e.printStackTrace(); // 打印错误
} finally {
    // 不管出不出错都执行(可选)
}
```

### 拿到编程题"改哪里"对照表

| 模板 | 改这几处 |
|---|---|
| 1 读文件 | 文件路径、转换(parseInt/不转)、处理(排序/求和/统计) |
| 2 写文件 | 输出路径、写的内容、覆盖还是追加(true) |
| 3 读排写 | 输入路径、输出路径 |
| 4 JDBC | 驱动名、IP/端口/库名、用户密码、表名、字段、SQL |
| 5 GUI按钮 | 标题、按钮名、位置、弹窗文字 |
| 6 画图 | 画什么(fillRect/drawLine)、楼名、弹窗文字 |
| 7 多线程 | 线程名、run()里做什么 |

═══════════════════════════════════════════════

# 第二部分　改错题手册（查毛病）

═══════════════════════════════════════════════

## 改错目录

- **A 类与继承**：A1顶层类private／A2重写缩小权限／A3final类被继承／A4final方法被重写／A5多个public类／A6类名≠文件名／A7构造没调super
- **B 抽象abstract**：B1抽象方法带{}／B2有抽象方法类没abstract／B3abstract+final／B4abstract+private/static／B5new抽象类／B6子类没实现全部抽象方法
- **C 接口interface**：C1实现方法不是public／C2接口方法带方法体／C3改接口字段／C4没实现全部／C5extends/implements搞反／C6接口成员加private
- **D final**：D1变量二次赋值／D2final引用改内容(陷阱)／D3构造方法final／D4lambda改外部变量
- **E static**：E1static方法访问非static／E2static用this／E3非static内部类有static／E4static当重写
- **F 内部类**：F1直接new成员内部类／F2匿名类语法／F3匿名类改局部变量
- **G 重写多态**：G1重写签名不符／G2static当重写／G3字段无多态
- **H 常见语法**：H1==比字符串／H2数组越界/length／H3包装类==／H4switch漏break／H5构造方法写返回类型／H6start用成run

## A. 类与继承

### A1 顶层类用 private/protected

```java
private class Foo {} // ❌
```

错因：顶层类只能 public 或 default(不写)。改：去掉private→ `class Foo`。
（注意：内部类可以用四种权限，这条只针对顶层类。）

### A2 重写方法缩小权限

```java
class P { public void f(){} }
class C extends P { void f(){} } // ❌ public缩成default
```

错因：重写权限只能放大或不变，不能缩小(破坏多态)。改：`public void f(){}`。
权限大小：private < default < protected < public。

### A3 final 类被继承

```java
final class A{} class B extends A{} // ❌
```

改：去掉A的final，或B别继承。

### A4 final 方法被重写

```java
class P{ final void f(){} }
class C extends P{ void f(){} } // ❌
```

改：去掉父类方法的final。

### A5 一个文件多个 public 类

```java
public class A{} public class B{} // ❌
```

错因：一个.java最多一个public类。改：只留一个public，其余去掉。

### A6 public 类名≠文件名

错因：public类名必须和文件名完全一致(含大小写)。改：改类名或文件名使一致。

### A7 子类构造没正确调 super

```java
class P{ P(int x){} } // 父类只有带参构造
class C extends P{ C(){} } // ❌ 默认super()但父类没无参构造
```

改：C构造里第一行写 `super(参数);`（super必须是第一句）。

## B. 抽象 abstract

### B1 抽象方法带方法体(含空{})

```java
abstract void f() {} // ❌ 空{}也不行
abstract void f();   // ✅
```

改：去掉{}用分号。

### B2 有抽象方法但类没abstract

```java
class A{ abstract void f(); } // ❌
```

改：`abstract class A`。

### B3 abstract + final

```java
final abstract class A{} // ❌ 矛盾
```

改：去掉一个(通常去final)。

### B4 abstract + private/static

```java
private abstract void f(); // ❌
static abstract void f();  // ❌
```

错因：abstract要被重写，private看不见、static不重写，都矛盾。改：去掉private/static。

### B5 直接 new 抽象类

```java
abstract class A{} A a=new A(); // ❌
```

改：用子类 `new 子类()` 或匿名类 `new A(){实现方法}`。

### B6 子类没实现全部抽象方法又没声明abstract

```java
abstract class A{ abstract void f(); abstract void g(); }
class B extends A{ void f(){} } // ❌ 缺g()
```

改：实现g()，或B也声明abstract。

## C. 接口 interface

### C1 实现接口方法不是public

```java
interface I{ void f(); }
class C implements I{ void f(){} } // ❌ 默认权限=缩小
```

改：`public void f(){}`。

### C2 接口方法带方法体(传统)

```java
interface I{ void f(){} } // ❌
```

改：`void f();`。要带实现用 `default void f(){}`。

### C3 修改接口字段

```java
interface I{ int X=10; } I.X=20; // ❌ X是public static final常量
```

改：不能改。

### C4 implements多个没实现全部

```java
class C implements A,B{} // ❌ A、B方法都要实现
```

改：所有方法都实现(都加public)。

### C5 extends/implements 搞反

```java
class C extends MyInterface{}    // ❌ 接口用implements
class C implements MyClass{}     // ❌ 类用extends
```

规则：类实现接口→implements；类继承类→extends；接口继承接口→extends。

### C6 接口成员加private/protected

```java
interface I{ protected void f(); } // ❌
```

改：接口方法只能public(默认)，去掉修饰符。

## D. final

### D1 final变量二次赋值

```java
final int x=5; x=6; // ❌
```

改：只能赋值一次。

### D2 final引用改内容(陷阱:其实合法)

```java
final int[] arr={1,2,3};
arr[0]=99;      // ✅ 合法!改内容
arr=new int[5]; // ❌ 改引用才错
```

判断：改 arr[i]/对象内容→不算错；改 arr=新对象→错。

### D3 构造方法用final

```java
final Foo(){} // ❌ 构造方法不能final(也不能abstract/static)
```

改：去掉final。

### D4 lambda/匿名类改外部局部变量

```java
int c=0; Runnable r=()->{ c++; }; // ❌
```

改：只能读不能改；要累加用数组或成员变量绕开。

## E. static

### E1 static方法访问非static成员

```java
class A{ int x; static void f(){ System.out.println(x); } } // ❌
```

错因：static属于类、没this，访问不了实例成员。改：x改static，或f()改非static，或方法内new对象访问。

### E2 static方法用this

```java
static void f(){ this.x=1; } // ❌ static无this
```

改：去掉this。

### E3 非static内部类有static成员(旧版本)

```java
class Outer{ class Inner{ static int x; } } // ❌ Java16前
```

改：Inner改 `static class`，或x去掉static。(Java16+已放开)

### E4 static方法当重写

```java
class P{ static void f(){} }
class C extends P{ @Override static void f(){} } // ❌
```

错因：static是隐藏不是重写，不能@Override。改：去掉@Override。

## F. 内部类

### F1 直接new成员内部类(没外部对象)

```java
Outer.Inner i=new Outer.Inner(); // ❌
```

改：

```java
Outer o=new Outer();
Outer.Inner i=o.new Inner(); // ✅
```

(若Inner是static内部类，`new Outer.Inner()` 才对。)

### F2 匿名类/适配器语法

```java
btn.addMouseListener(new MouseAdapter { // ❌ 少()
    public void mouseClicked(MouseEvent e){}
});
```

改：`new MouseAdapter()` 带括号，结尾 `});`。

### F3 匿名类改局部变量

```java
int n=5;
new Runnable(){ public void run(){ n=9; } }; // ❌
```

改：只读不改。

## G. 重写 vs 重载 / 多态

### G1 重写签名不符

```java
class P{ void f(int x){} }
class C extends P{ void f(String x){} } // 这是重载不是重写
```

要点：重写要名字+参数+返回值一致；参数变了=重载。改：参数与父类一致。

### G2 static当重写

见E4，static同名是隐藏。

### G3 字段无多态

```java
class P{ int x=1; } class C extends P{ int x=2; }
P p=new C(); System.out.println(p.x); // 输出1不是2!
```

要点：方法看实际对象类型(多态)；字段看引用声明类型(无多态)。p.x按P算=1。

## H. 常见语法

### H1 == 比字符串内容

```java
if(s1==s2) // ❌ 比地址
```

改：`s1.equals(s2)`。

### H2 数组越界/length

```java
int[] a=new int[5]; a[5]=1; // ❌ 下标0~4
a.length()                  // ❌ 数组是length属性,无括号
```

改：下标0~length-1；数组用 `a.length`(无括号)，字符串才是 `s.length()`。

### H3 包装类 ==

```java
Integer a=128,b=128; if(a==b) // ❌ 超出-128~127缓存,==为false
```

改：用 `.equals()` 或 `a.intValue()==b.intValue()`。

### H4 switch漏break

```java
switch(x){ case 1: doA(); case 2: doB(); } // ❌ 没break会贯穿
```

改：每个case末尾加 `break;`。

### H5 构造方法写返回类型

```java
class A{ void A(){} } // ❌ 这是普通方法不是构造!
```

错因：构造方法不能有返回类型(连void都不行)。改：去掉void→ `A(){}`。

### H6 启动线程用了run

```java
new MyThread().run(); // ❌ run只是普通调用,不开新线程
```

改：`new MyThread().start();`。

## 改错题 30 秒排查顺序

1. 看权限：顶层类有没private/protected(A1)；重写有没缩小(A2/C1)
2. 看abstract：抽象方法带没带{}(B1)；该不该abstract(B2)；冲不冲突(B3/B4)
3. 看final：变量改没改(D1)；引用vs内容(D2陷阱)
4. 看static：碰没碰实例成员/this(E1/E2)
5. 看接口：implements方法public没(C1)；extends/implements反没(C5)
6. 看构造：写没写返回类型(H5)；super对不对(A7)
7. 看==：字符串/包装类用没用==(H1/H3)
8. 看new：成员内部类给外部对象没(F1)；抽象类被new没(B5)
9. 看线程：start写成run没(H6)
10. 看多态：字段按引用类型(G3)；switch漏break(H4)

═══════════════════════════════════════════════
编程抄第一部分，改错查第二部分，开卷通杀。
═══════════════════════════════════════════════

# Java 基本数据类型与 String 互相转换完整总结

## 一、int 类型与 String 转换（计算器题目专用前置总结）

### 1. int → String（给文本框 setText 场景）

设 `int num = 123;`

```java
// 字符串拼接（作业最简写法）
String s1 = num + "";
// String.valueOf() 标准通用写法
String s2 = String.valueOf(num);
// Integer.toString() 专用静态方法
String s3 = Integer.toString(num);
// 自动装箱后调用 toString()（不可直接 num.toString()）
Integer obj = num; // 自动装箱
String s4 = obj.toString();
```

### 2. String → int（读取文本框 getText 场景）

设 `String str = "123";`

```java
// Integer.parseInt()（最常用，返回基础 int）
int n1 = Integer.parseInt(str);
// Integer.valueOf()（返回 Integer 对象，自动拆箱）
Integer n2 = Integer.valueOf(str);
int num = n2;
```

注意：字符串非数字会抛出 NumberFormatException。

### 3. 易错点

int 是基础数据类型，不存在 `.toString()` 方法，直接写 `res.toString()` 编译报错；
只有装箱后的 Integer 对象才能调用 `.toString()`。

## 二、Java 8 种全部基本数据类型 ↔ String 转换大全

8 种基础类型：byte、short、int、long、float、double、char、boolean

### （一）所有基本类型 → String

**方案 1：字符串拼接（作业首选，最简单）** 任意基础类型拼接空字符串，自动转为字符串：

```java
byte b = 1; short s = 2; int i = 3; long l = 4L;
float f = 5.5F; double d = 6.6; char c = 'A'; boolean bl = true;
String sb = b + ""; String ss = s + ""; String si = i + ""; String sl = l + "";
String sf = f + ""; String sd = d + ""; String sc = c + ""; String sbl = bl + "";
```

**方案 2：String.valueOf(变量)（项目标准通用写法）** 统一 API，支持全部 8 种基本类型与对象：

```java
String sb = String.valueOf(b); String ss = String.valueOf(s); String si = String.valueOf(i);
String sl = String.valueOf(l); String sf = String.valueOf(f); String sd = String.valueOf(d);
String sc = String.valueOf(c); String sbl = String.valueOf(bl);
```

**方案 3：对应包装类静态方法 Xxx.toString(值)** 每种基础类型对应专属包装类：Byte/Short/Integer/Long/Float/Double/Character/Boolean：

```java
String sb = Byte.toString(b); String ss = Short.toString(s); String si = Integer.toString(i);
String sl = Long.toString(l); String sf = Float.toString(f); String sd = Double.toString(d);
String sc = Character.toString(c); String sbl = Boolean.toString(bl);
```

**方案 4：装箱后调用对象 .toString()（冗余，不推荐）** 基础类型自动装箱为包装对象后调用方法，基础类型本身无法调用 `.toString()`：

```java
int num = 10; Integer obj = num; // 自动装箱
String str = obj.toString();
```

### （二）String → 对应基本数据类型（读取输入文本场景）

通用规则：`包装类.parseXxx(字符串)`，返回基础数据类型：

```java
String numStr = "123"; String boolStr = "true"; String charStr = "a";
// 1. String → byte
byte b = Byte.parseByte(numStr);
// 2. String → short
short s = Short.parseShort(numStr);
// 3. String → int（计算器核心）
int i = Integer.parseInt(numStr);
// 4. String → long
long l = Long.parseLong(numStr);
// 5. String → float
float f = Float.parseFloat("3.14");
// 6. String → double
double d = Double.parseDouble("6.66");
// 7. String → boolean
boolean bl = Boolean.parseBoolean(boolStr);
// 8. String → char（无 parseChar 方法，特殊处理）
char c = charStr.charAt(0); // 取字符串第一个字符，要求字符串长度为1
```

备选方案：`Xxx.valueOf(String)`，返回包装对象，自动拆箱（底层本质依旧调用 parseXxx 方法，多一步装箱操作）：

```java
int num = Integer.valueOf("123");
double d = Double.valueOf("2.5");
```

## 三、特殊类型转换注意事项

- **char 字符类型**：不存在 `Character.parseChar()` 方法；字符串转字符只能使用 `str.charAt(0)`，要求输入字符串长度必须为 1。
- **boolean 布尔类型**：仅当字符串严格等于 "true"（大小写不敏感）时，转换结果为 true；其余任意字符串（如 "abc"、"123"）转换结果均为 false。
- **浮点型 float/double**：传入的字符串必须是合法数字 / 小数格式，否则抛出 NumberFormatException。
- **整数范围限制 byte/short/int/long**：`Byte.parseByte()` 数字范围仅 -128 ~ 127，超出范围抛异常；`Short.parseShort()` 范围 -32768 ~ 32767；超出对应类型取值范围时，会抛出数字格式异常。

## 四、做题速记最优写法

1. 基础类型转字符串（文本框 setText）
   - 作业简写：`变量 + ""`
   - 规范工程写法：`String.valueOf(变量)`
2. 字符串转基础数字（读取输入框 getText）
   - 统一使用 parse 系列方法：整数 `Integer.parseInt(str)`；小数 `Double.parseDouble(str)`
3. 高频避坑提醒
   - 基础类型（int/byte/short 等）不能直接调用 `.toString()`；
   - char 无 parse 转换方法，使用 `charAt(0)`；
   - 非数字字符串转数字会直接抛出异常，复杂项目需要加 try-catch 捕获。

# Java 异常「给代码写输出」开卷速查

用法：先用【第一部分】四条铁律判断程序走向，再用【第二部分】查具体异常的 `+e` / `getMessage()` 输出，最后用【第三部分】套各种结构(finally/嵌套/多catch/自定义/throws链)的固定输出模式。

## 第一部分：写输出前先过的铁律

1. `"xxx="+e` 或 `println(e)` → 打印 `e.toString()` → 全限定类名 + ": " + 消息。例：`java.lang.ArrayIndexOutOfBoundsException: 10`。消息为 null 时只打类名，无冒号。
2. `e.getMessage()` → 只有消息体，无类名。消息可能是 null (直接打印出 "null")。
3. `e.getClass().getName()` → 只有类名 `java.lang.XxxException`；`getSimpleName()` → 只有 `XxxException`(无包名)。
4. 程序走向
   - 异常被 catch 接住 → 不崩，catch 后代码照常执行。
   - 没有匹配的 catch → 异常上抛，当前方法终止，沿调用链找；到 main 还没人接 → 控制台打印 stack trace，程序退出码非0，该异常点之后所有正常代码都不执行。
   - try 里抛异常那行之后的语句：跳过，不执行。
   - finally 永远执行(除非 System.exit() 或 JVM 崩溃)，哪怕 try/catch 里有 return。

## 第二部分：常见异常 toString / getMessage 输出对照

> ⚠ 版本差异：数组/字符串越界的消息文本在 Java 旧版本与新版本不同。以你们考试标准答案为准。给的标答 `: 10` 是「只取下标」的写法(老版本风格)，下表「toString」按此填，新版本另注。

| 触发代码 | 异常全名 | `+e` (toString) | `getMessage()` |
|---|---|---|---|
| `arr[10]=7;` (len 3) | `java.lang.ArrayIndexOutOfBoundsException` | `...ArrayIndexOutOfBoundsException: 10` | 老: `10`；新: `Index 10 out of bounds for length 3` |
| `5/0` (整数) | `java.lang.ArithmeticException` | `...ArithmeticException: / by zero` | `/ by zero` |
| `Integer.parseInt("abc")` | `java.lang.NumberFormatException` | `...NumberFormatException: For input string: "abc"` | `For input string: "abc"` |
| `s=null; s.length()` | `java.lang.NullPointerException` | 老: `...NullPointerException`(无消息,无冒号)；新(14+): `...NullPointerException: Cannot invoke "String.length()" because "s" is null` | 老: `null`；新: 那句完整描述 |
| `"abc".charAt(10)` | `java.lang.StringIndexOutOfBoundsException` | `...StringIndexOutOfBoundsException: ...` | 老: `String index out of range: 10`；新: `Index 10 out of bounds for length 3` |
| `"abc".substring(5)` | `java.lang.StringIndexOutOfBoundsException` | 同上 | `begin 5, end 3, length 3`(新) / `String index out of range: -2`(老) |
| `(Integer)(Object)"x"` | `java.lang.ClassCastException` | `...ClassCastException: class java.lang.String cannot be cast to class java.lang.Integer ...` | 同(去掉前缀类名部分) |
| `new int[-1]` | `java.lang.NegativeArraySizeException` | `...NegativeArraySizeException: -1` | `-1` |
| `Object[] o=new String[1]; o[0]=1;` | `java.lang.ArrayStoreException` | `...ArrayStoreException: java.lang.Integer` | `java.lang.Integer` |
| `Class.forName("X.Y")` 不存在 | `java.lang.ClassNotFoundException` | `...ClassNotFoundException: X.Y` | `X.Y` |
| 遍历中改集合 | `java.util.ConcurrentModificationException` | `...ConcurrentModificationException` | `null` |
| 不可变集合 add | `java.lang.UnsupportedOperationException` | `...UnsupportedOperationException` | `null` |
| `Thread.sleep` 被中断 | `java.lang.InterruptedException` | `...InterruptedException: sleep interrupted` | `sleep interrupted` |
| 栈溢出(无限递归) | `java.lang.StackOverflowError` | `...StackOverflowError` | `null` |
| `iterator.next()` 无元素 | `java.util.NoSuchElementException` | `...NoSuchElementException` | `null` |
| 自定义 `throw new XxxException("msg")` | 你的全限定类名 | `包名.XxxException: msg` | `msg` |
| 自定义 `throw new XxxException()` (无参) | 你的全限定类名 | `包名.XxxException`(无冒号) | `null` |

记忆要点：消息内容 = 「触发值」。越界→下标；parseInt→那个串；cast→源类型→目标类型；数组存类型错→存入对象的类型；自定义→构造器传的字符串。

## 第三部分：各种代码结构的输出模式

### 1. try-catch-finally 执行顺序

```java
try{ A; throw; B; } catch(e){ C; } finally{ D; } E;
```

输出顺序：A → C → D → E(B 被跳过；D 必执行；异常被接住所以 E 也执行)。
若没被 catch 接住：A → D，然后异常上抛，E 不执行。

### 2. finally 里有 return / try 里有 return

- try 里 return 前，finally 仍先执行，再真正返回。
- finally 里有 return → 会覆盖 try/catch 的 return 值和待抛异常(吞掉异常，考点!)。

```java
try{ return 1; } finally{ return 2; } // 实际返回 2
try{ throw new RuntimeException(); } finally{ return 3; } // 不抛异常,返回3
```

### 3. 多 catch：从上往下匹配第一个，父类要放后面

```java
try{ 5/0; }
catch(ArithmeticException e){ println("算术"); } // ← 命中这个
catch(Exception e){ println("通用"); }
```

输出：`算术`。只进一个 catch，不会继续往下。
> ⚠ 若父类 Exception 写在子类前面 → 编译错误(不是运行输出)，写"compile error"。

### 4. 多异常合并 catch(Java 7+)

```java
catch(IOException | SQLException e){ ... } // 合法
```

合并的几个类之间不能有父子关系，否则编译错误。

### 5. 嵌套 try：内层没接到的抛给外层

```java
try{
    try{ 5/0; } catch(NumberFormatException e){ println("内"); }
    println("内层后"); // 不执行
} catch(ArithmeticException e){ println("外"); }
```

输出：`外`(内层 catch 类型不匹配，异常穿透到外层；"内层后"被跳过)。

### 6. 异常在方法间传播(throws 链)

```java
void a() throws Exception { b(); println("a尾"); } // a尾不执行
void b() throws Exception { throw new Exception("X"); }
// main: try{ a(); } catch(Exception e){ println(e.getMessage()); }
```

输出：`X`。b 抛出→a 不接(只 throws)→a 的"a尾"不执行→传到 main 的 catch。

### 7. 异常链 getCause()

```java
catch(Exception e){
    println(e.getMessage()); // 当前异常消息
    println(e.getCause());   // 底层异常的 toString,没有则 null
}
// 抛出方:throw new RuntimeException("外层", new IOException("内层"));
```

`getMessage()` → `外层`；`getCause()` → `java.io.IOException: 内层`。

### 8. printStackTrace 输出形式(题目若问"打印什么")

```
java.lang.ArithmeticException: / by zero
    at 类名.方法(文件:行号)
    at ...
```

第一行 = toString()；后面每行 `\tat ...` 是栈帧。考试通常只要求写第一行 + 「后接调用栈」即可。

### 9. 自定义异常完整例

```java
class MyException extends Exception {
    public MyException(String m){ super(m); }
}
// throw new MyException("出错了");
// catch(MyException e){
//   println(e); → 包名.MyException: 出错了
//   println(e.getMessage()); → 出错了
// }
```

`super(m)` 传进去的就是 message；没传则 `getMessage()` 为 null。

### 10. checked vs unchecked(影响"编译能否过")

- **Checked**(IOException, SQLException, ClassNotFoundException, InterruptedException, 自定义 extends Exception)：必须 try-catch 或 throws，否则编译错误。
- **Unchecked**(RuntimeException 及其子类：NPE, 各种越界, ArithmeticException, ClassCast, NumberFormat...)：不强制处理，不写也能编译，运行时才抛。

题目让你判断"能否编译"：checked 异常没处理 → compile error；unchecked → 编译通过，运行时输出异常。

## 第四部分：套你那道原题(对照验证)

```java
int arr[]=new int[3];
arr[10]=7; // 抛 AIOOBE,赋值未成功,跳进catch
// catch:
println("index out of bound!!"); // 字面量
println("Exception="+e);         // "Exception=" + e.toString()
// catch后:
println("end of main()");        // 异常被接住,程序没崩,继续
```

输出：

```
index out of bound!!
Exception=java.lang.ArrayIndexOutOfBoundsException: 10
end of main()
```

核对三处：① 带包名 `java.lang.` ② 是下标 10 不是长度 3 ③ 最后一行必出。

## 一页极简口诀

- `+e` = 类名:消息 ｜ `getMessage()` = 只消息(可能 null)
- 消息 = 触发值(下标/串/类型)
- catch 接住→后面照跑 ｜ 没接住→方法终止+栈轨迹
- finally 必执行，finally 里 return 会吞异常和覆盖返回值
- 多catch 只进一个；父类在子类前=编译错
- checked 不处理=编译错；unchecked 不处理=运行时抛
- 嵌套/throws：本层不接就往外抛，抛出点后的语句全跳过

# Swing GUI 公式化速查(开卷直接拼)

用法：GUI 程序 = 骨架 + 布局 + 组件 + 事件，四层各挑一块往里套。
先抄【第0部分骨架】，再按题目要求从后面各部分挑零件填进去。

## 第 0 部分：万能骨架(任何题先抄这个)

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
public class Demo extends JFrame {
    public Demo() {
        setTitle("标题");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 关窗即退出
        setLocationRelativeTo(null); // 窗口居中
        setLayout(new BorderLayout()); // ← 这里换布局
        // ← 这里 new 组件、add 进去
        // ← 这里加事件监听
    }
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> new Demo().setVisible(true));
    }
}
```

四句固定开头的作用(常考填空)：
- `setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE)` 不写 → 点 X 窗口消失但程序不退出。
- `setLocationRelativeTo(null)` → 居中；不写 → 左上角。
- `setVisible(true)` 必须最后调，且不写窗口不显示。
- `setSize` 或 `pack()` 二选一；`pack()` 按组件首选大小自适应。

## 第 1 部分：布局(setLayout 换这一行)→ 决定组件怎么排

| 想要的效果 | 套这行 | add 方式 |
|---|---|---|
| 东南西北中 五区 | `setLayout(new BorderLayout(水平间距,垂直间距))` | `add(组件, BorderLayout.NORTH/SOUTH/EAST/WEST/CENTER)` |
| 一行从左到右排,挤满换行 | `setLayout(new FlowLayout())` | `add(组件)` 直接加,按顺序排 |
| 网格 m行n列,每格等大 | `setLayout(new GridLayout(行,列,水平间距,垂直间距))` | `add(组件)` 按从左到右、从上到下填 |
| 不设布局,绝对定位 | `setLayout(null)` | 组件需 `setBounds(x,y,w,h)` |
| 卡片堆叠(一次显一张) | `setLayout(new CardLayout())` | 配合 `show()` 切换 |

默认布局(不写 setLayout 时)：
- JFrame 默认 BorderLayout
- JPanel 默认 FlowLayout

BorderLayout 要点：
- 每个区只能放一个组件；放多个要先塞进 JPanel。
- CENTER 会吸收所有剩余空间；NORTH/SOUTH 占满宽度、高度由组件定；EAST/WEST 反之。
- 只 add 到 CENTER 不写区域 = `add(c)` 默认就是 CENTER。

## 第 2 部分：嵌套套路(复杂界面=面板套面板)

核心公式：外层 BorderLayout 分大区，每个大区放一个 JPanel，JPanel 内部再用自己的布局排组件。

```java
// 上面一行标签 + 中间大文本 + 下面输入行,就是你那道题的结构:
add(new JLabel("标题"), BorderLayout.NORTH); // 上:单个组件直接放
JTextArea ta = new JTextArea();
add(new JScrollPane(ta), BorderLayout.CENTER); // 中:文本域要套滚动条!
JPanel bottom = new JPanel(new FlowLayout()); // 下:多个组件→先建面板
bottom.add(new JTextField(20));
bottom.add(new JButton("添加"));
add(bottom, BorderLayout.SOUTH);
```

通用嵌套模板：

```java
JPanel 子面板 = new JPanel(new 某布局());
子面板.add(组件1);
子面板.add(组件2);
add(子面板, BorderLayout.某区); // 子面板作为一个整体放进主框架的某区
```

## 第 3 部分：常用组件(new 出来 + 取值/设值)

| 组件 | 创建 | 取内容 | 设内容 |
|---|---|---|---|
| 标签 | `new JLabel("文字")` | `getText()` | `setText("x")` |
| 单行输入框 | `new JTextField(列数)` | `getText()` | `setText("x")` |
| 密码框 | `new JPasswordField(列数)` | `getPassword()` (返回 char[]) | — |
| 多行文本域 | `new JTextArea(行,列)` | `getText()` | `setText` / `append("x\n")` 追加 |
| 按钮 | `new JButton("文字")` | — | `setText` |
| 复选框 | `new JCheckBox("文字")` | `isSelected()` (boolean) | `setSelected(true)` |
| 单选按钮 | `new JRadioButton("文字")` | `isSelected()` | 需加进 ButtonGroup 才互斥 |
| 下拉框 | `new JComboBox<>(数组)` | `getSelectedItem()` / `getSelectedIndex()` | `addItem("x")` |
| 列表 | `new JList<>(数组)` | `getSelectedValue()` | — |

两个必背陷阱：
1. JTextArea 必须包 JScrollPane 才有滚动条：`add(new JScrollPane(textArea), ...)`，直接 add textArea 不滚。
2. JRadioButton 要互斥：必须建 `ButtonGroup bg = new ButtonGroup(); bg.add(r1); bg.add(r2);`，否则能同时选中。ButtonGroup 只管逻辑互斥，不负责布局，组件还要单独 add 到面板。

## 第 4 部分：事件监听(四种写法，任选)

事件 = 给组件 add 一个监听器，组件被操作时执行 actionPerformed。

### 写法 A：Lambda(最简，Java 8+，推荐开卷用)

```java
btn.addActionListener(e -> {
    String s = textField.getText();
    textArea.append(s + "\n");
});
```

### 写法 B：匿名内部类(老教材最常见)

```java
btn.addActionListener(new ActionListener() {
    public void actionPerformed(ActionEvent e) {
        textArea.append(textField.getText() + "\n");
    }
});
```

### 写法 C：让主类 implements ActionListener

```java
public class Demo extends JFrame implements ActionListener {
    JButton btn;
    public Demo(){
        btn = new JButton("点");
        btn.addActionListener(this); // ← 注册 this
        add(btn);
    }
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == btn) { ... } // 多组件时用 getSource 区分
    }
}
```

### 写法 D：单独的监听器类

```java
class MyListener implements ActionListener {
    public void actionPerformed(ActionEvent e){ ... }
}
// btn.addActionListener(new MyListener());
```

事件挂哪个组件 = 哪个动作触发：

| 想响应 | 挂在 | 监听器 | 回调方法 |
|---|---|---|---|
| 按钮点击 | JButton | ActionListener | actionPerformed |
| 文本框回车 | JTextField | ActionListener | actionPerformed |
| 复选/单选/下拉 改变 | 对应组件 | ActionListener 或 ItemListener | actionPerformed / itemStateChanged |
| 鼠标点击/进入/离开 | 任意组件 | MouseListener (或 MouseAdapter) | mouseClicked 等 |
| 键盘按键 | 任意组件 | KeyListener (或 KeyAdapter) | keyPressed 等 |
| 窗口关闭 | JFrame | WindowListener (或 WindowAdapter) | windowClosing |

复用同一段逻辑挂给多个组件(你那题的做法)：

```java
Runnable task = () -> { /* 共同逻辑 */ };
textField.addActionListener(e -> task.run()); // 回车
btn.addActionListener(e -> task.run());       // 点击
```

Adapter 的意义：XxxListener 接口有多个方法必须全实现；XxxAdapter 是空实现的类，继承它只重写你要的那个方法，省代码。例：只想要鼠标点击 → `addMouseListener(new MouseAdapter(){ public void mouseClicked(MouseEvent e){...} })`。

## 第 5 部分：三种典型界面「整套」配方

### 配方①：输入区 + 显示区(你那道题)

```
布局: BorderLayout
  NORTH : JLabel 提示
  CENTER : JScrollPane(JTextArea) 显示
  SOUTH : JPanel(FlowLayout){ JTextField + JButton }
事件 : 按钮 + 文本框回车 共用一段 append 逻辑
```

### 配方②：登录框

```
布局: GridLayout(3,2) 或 BorderLayout+面板
  行1: JLabel("用户名") JTextField
  行2: JLabel("密码")   JPasswordField
  行3: JButton("登录")  JButton("取消")
事件: 登录按钮读取两框内容做判断
```

### 配方③：计算器/多按钮网格

```
布局: BorderLayout
  NORTH : JTextField(显示结果, setEditable(false))
  CENTER: JPanel(GridLayout(4,4)){ 16个JButton }
事件 : implements ActionListener,用 e.getSource()
       或 e.getActionCommand() 取按钮文字区分
```

## 一页极简口诀

- 骨架：title→size→close→center→layout→加组件→加事件→ main 里 invokeLater + setVisible(true)
- 布局：东南西北中=Border ｜ 一行流=Flow ｜ 网格=Grid ｜ JFrame默认Border,JPanel默认Flow
- 嵌套：一个区放多个组件 → 先塞进 JPanel 再 add
- 取值：文本框/域 `getText()`；勾选 `isSelected()`；下拉 `getSelectedItem()`
- 两大坑：JTextArea 要 `new JScrollPane(ta)` 才滚；JRadioButton 要 ButtonGroup 才互斥
- 事件：`组件.addActionListener(e -> {...})`；按钮点击和文本框回车都用 ActionListener；多组件用 `e.getSource()` 区分

═══════════════════════════════════════════════

# 第四部分　SQL 基础操作速查（配例子）

═══════════════════════════════════════════════

JDBC 里 executeUpdate / executeQuery 执行的就是这些 SQL。下面以一张学生表 Student 为例。增删改用 executeUpdate，查询用 executeQuery。

## 0. 建表（CREATE TABLE）

```sql
CREATE TABLE Student (
    sCode  VARCHAR(20) PRIMARY KEY, -- 学号,主键
    sName  VARCHAR(50) NOT NULL,    -- 姓名,非空
    sAge   INT,                     -- 年龄
    sMajor VARCHAR(50)              -- 专业
);
```

说明：`PRIMARY KEY` 主键（唯一、非空）；`NOT NULL` 该列不能为空；常用类型 `INT` 整数、`VARCHAR(n)` 变长字符串、`FLOAT` 小数、`DATE` 日期。

## 1. 插入数据（INSERT）—— executeUpdate

```sql
-- 指定列插入(推荐)
INSERT INTO Student(sCode, sName, sAge, sMajor)
VALUES('20230001', '孟宪喆', 20, '自动化');
-- 不写列名,必须按建表顺序给全部值
INSERT INTO Student VALUES('20230002', '徐海洋', 21, '自动化');
```

## 2. 查询数据（SELECT）—— executeQuery

```sql
-- 查全部列
SELECT * FROM Student;
-- 查指定列
SELECT sName, sAge FROM Student;
-- 条件查询 WHERE
SELECT * FROM Student WHERE sAge > 20;
SELECT * FROM Student WHERE sMajor = '自动化';
-- 多条件 AND / OR
SELECT * FROM Student WHERE sAge >= 20 AND sMajor = '自动化';
-- 模糊查询 LIKE ( % 匹配任意多字符, _ 匹配一个字符 )
SELECT * FROM Student WHERE sName LIKE '孟%'; -- 姓孟的
-- 范围 IN / BETWEEN
SELECT * FROM Student WHERE sCode IN ('20230001', '20230002');
SELECT * FROM Student WHERE sAge BETWEEN 18 AND 22;
```

例：查 `SELECT sName, sAge FROM Student` 的结果

| sName | sAge |
|---|---|
| 孟宪喆 | 20 |
| 徐海洋 | 21 |

## 3. 修改数据（UPDATE）—— executeUpdate

```sql
-- ★一定要带 WHERE,否则改全表!
UPDATE Student SET sName = '陈嘉宇' WHERE sCode = '20230001';
-- 同时改多列
UPDATE Student SET sAge = 22, sMajor = '计算机' WHERE sCode = '20230002';
```

## 4. 删除数据（DELETE）—— executeUpdate

```sql
-- ★一定要带 WHERE,否则删全表!
DELETE FROM Student WHERE sCode = '20230001';
-- 删除年龄小于18的所有行
DELETE FROM Student WHERE sAge < 18;
```

## 5. 排序与统计

```sql
-- 排序 ORDER BY ( ASC 升序默认 / DESC 降序 )
SELECT * FROM Student ORDER BY sAge DESC; -- 按年龄降序
-- 统计函数: COUNT 计数, AVG 平均, MAX/MIN 最值, SUM 求和
SELECT COUNT(*) FROM Student;          -- 总人数
SELECT AVG(sAge) FROM Student;         -- 平均年龄
SELECT MAX(sAge), MIN(sAge) FROM Student; -- 最大/最小年龄
-- 分组统计 GROUP BY: 每个专业多少人
SELECT sMajor, COUNT(*) FROM Student GROUP BY sMajor;
```

例：`SELECT sMajor, COUNT(*) FROM Student GROUP BY sMajor` 的结果

| sMajor | COUNT |
|---|---|
| 自动化 | 2 |
| 计算机 | 1 |

## 6. SQL 操作速查表

| 操作 | 关键字 | Java 执行方法 | 例子 |
|---|---|---|---|
| 建表 | CREATE TABLE | executeUpdate | `CREATE TABLE Student(...)` |
| 增 | INSERT INTO | executeUpdate | `INSERT INTO Student VALUES(...)` |
| 删 | DELETE FROM | executeUpdate | `DELETE FROM Student WHERE ...` |
| 改 | UPDATE...SET | executeUpdate | `UPDATE Student SET ... WHERE ...` |
| 查 | SELECT...FROM | executeQuery | `SELECT * FROM Student WHERE ...` |
| 条件 | WHERE | — | `WHERE sAge > 20` |
| 模糊 | LIKE | — | `WHERE sName LIKE '孟%'` |
| 排序 | ORDER BY | — | `ORDER BY sAge DESC` |
| 分组 | GROUP BY | — | `GROUP BY sMajor` |
| 统计 | COUNT/AVG/MAX/MIN/SUM | — | `SELECT COUNT(*) ...` |

记忆要点：增删改 = `executeUpdate`（返回受影响行数 int）；查 = `executeQuery`（返回 ResultSet）。UPDATE 和 DELETE 千万别忘 WHERE，否则操作整张表。


═══════════════════════════════════════════════

# 第五部分　Java 标准库 API 速查手册（类·方法·成员）

═══════════════════════════════════════════════


查法：按「分类目录」找到类所在大类 → 翻到该类小节 → 表格里查方法/成员的作用。
列说明：方法=怎么调用；作用=干什么；返回=返回类型/值。静态方法标 `[static]`，常量成员标 `[常量]`。

## 分类目录

1. **输入输出 IO**：File、FileInputStream、FileOutputStream、FileReader、FileWriter、BufferedReader、BufferedWriter、PrintWriter、ObjectInputStream、ObjectOutputStream、Scanner、System
2. **集合与工具**：ArrayList、LinkedList、List、Collections、Arrays、Random、Date
3. **字符串**：String、StringBuffer / StringBuilder
4. **包装类**：Integer、Double、Boolean、Character
5. **GUI 容器与组件**：JFrame、JPanel、JLabel、JButton、JTextField、JTextArea、JScrollPane、JOptionPane、JComboBox、JCheckBox、JRadioButton、ButtonGroup
6. **布局**：FlowLayout、BorderLayout、GridLayout、CardLayout、GridBagLayout
7. **绘图**：Graphics、Color、Font
8. **事件**：ActionListener、MouseListener、MouseAdapter、WindowAdapter、ActionEvent、MouseEvent
9. **多线程**：Thread、Runnable
10. **数据库 JDBC**：DriverManager、Connection、Statement、PreparedStatement、ResultSet
11. **异常**：Exception 体系常见类

---

## 一、输入输出 IO

### File（文件/目录对象，java.io）

| 方法/成员 | 作用 | 返回 |
|---|---|---|
| `new File("路径")` | 创建文件对象（不真正建文件） | File |
| `exists()` | 文件/目录是否存在 | boolean |
| `createNewFile()` | 创建新空文件 | boolean |
| `delete()` | 删除文件/目录 | boolean |
| `getName()` | 取文件名 | String |
| `getPath()` / `getAbsolutePath()` | 取路径 / 绝对路径 | String |
| `length()` | 文件字节数 | long |
| `isFile()` / `isDirectory()` | 是文件 / 是目录 | boolean |
| `list()` / `listFiles()` | 列目录下文件名 / 文件对象 | String[] / File[] |

### FileInputStream / FileOutputStream（字节流，读写任意文件）

| 方法 | 作用 | 返回 |
|---|---|---|
| `new FileInputStream("路径")` | 打开文件读字节 | — |
| `read()` | 读一个字节，文件尾返回 -1 | int |
| `read(byte[] b)` | 读一批字节进数组 | int(读到的个数) |
| `new FileOutputStream("路径")` | 打开文件写字节（覆盖） | — |
| `new FileOutputStream("路径", true)` | 追加方式写 | — |
| `write(int b)` | 写一个字节 | void |
| `write(byte[] b)` | 写一批字节 | void |
| `close()` | 关闭流（必须） | void |

### FileReader / FileWriter（字符流，读写文本）

| 方法 | 作用 |
|---|---|
| `new FileReader("路径")` | 打开文本文件读字符 |
| `read()` | 读一个字符，尾返回 -1 |
| `new FileWriter("路径")` | 写文本（覆盖） |
| `new FileWriter("路径", true)` | 追加写 |
| `write(String s)` | 写字符串 |
| `close()` | 关闭 |

### BufferedReader（带缓冲，按行读，最常用）

| 方法 | 作用 | 返回 |
|---|---|---|
| `new BufferedReader(new FileReader("路径"))` | 包装字符流 | — |
| `readLine()` | 读一整行，文件尾返回 null | String |
| `read()` | 读一个字符 | int |
| `close()` | 关闭 | void |

### BufferedWriter（带缓冲写）

| 方法 | 作用 |
|---|---|
| `new BufferedWriter(new FileWriter("路径"))` | 包装 |
| `write(String s)` | 写字符串（不自动换行） |
| `newLine()` | 写一个换行 |
| `flush()` / `close()` | 刷新 / 关闭 |

### PrintWriter（最方便的写，能 println）

| 方法 | 作用 |
|---|---|
| `new PrintWriter(new FileWriter("路径"))` | 创建 |
| `print(x)` / `println(x)` | 写 / 写并换行（任意类型，自动转字符串） |
| `printf(格式, ...)` | 格式化写 |
| `close()` | 关闭（不关可能写不进） |

### ObjectInputStream / ObjectOutputStream（对象序列化）

| 方法 | 作用 | 返回 |
|---|---|---|
| `writeObject(对象)` | 把对象写入文件（对象类要 implements Serializable） | void |
| `readObject()` | 从文件读回对象（要强转类型） | Object |
| `writeInt/writeUTF(...)` | 写基本类型/字符串 | void |
| `readInt/readUTF()` | 读基本类型/字符串 | int/String |
| `close()` | 关闭 | void |

### Scanner（控制台/文件输入，java.util）

| 方法 | 作用 | 返回 |
|---|---|---|
| `new Scanner(System.in)` | 读键盘 | — |
| `new Scanner(new File("路径"))` | 读文件 | — |
| `nextInt()` / `nextDouble()` | 读一个整数 / 小数 | int/double |
| `next()` | 读一个单词（到空白） | String |
| `nextLine()` | 读一整行 | String |
| `hasNextInt()` / `hasNextLine()` | 还有整数 / 还有行吗 | boolean |
| `close()` | 关闭 | void |

### System（系统类，全是静态）

| 成员/方法 | 作用 |
|---|---|
| `System.out` `[成员]` | 标准输出流（PrintStream），`System.out.println(x)` |
| `System.in` `[成员]` | 标准输入流 |
| `System.err` `[成员]` | 标准错误流 |
| `System.exit(0)` `[static]` | 退出程序（0=正常） |
| `System.currentTimeMillis()` `[static]` | 当前毫秒数 |
| `System.arraycopy(...)` `[static]` | 数组复制 |

---

## 二、集合与工具

### ArrayList\<E>（动态数组，最常用，java.util）

| 方法 | 作用 | 返回 |
|---|---|---|
| `new ArrayList<>()` | 建空表 | — |
| `add(e)` | 末尾加元素 | boolean |
| `add(i, e)` | 在下标 i 处插入 | void |
| `get(i)` | 取第 i 个 | E |
| `set(i, e)` | 改第 i 个 | E(旧值) |
| `remove(i)` | 删第 i 个 | E |
| `size()` | 元素个数 | int |
| `isEmpty()` | 是否空 | boolean |
| `contains(e)` | 是否包含 | boolean |
| `indexOf(e)` | 元素下标，无返回 -1 | int |
| `clear()` | 清空 | void |

### LinkedList\<E>（链表，可当队列/栈）

| 方法 | 作用 |
|---|---|
| `add(e)` / `get(i)` / `remove(i)` / `size()` | 同 ArrayList |
| `addFirst(e)` / `addLast(e)` | 头插 / 尾插 |
| `getFirst()` / `getLast()` | 取头 / 取尾 |
| `removeFirst()` / `removeLast()` | 删头 / 删尾 |

### List\<E>（接口）

List 是接口，ArrayList / LinkedList 实现它。常写 `List<Integer> list = new ArrayList<>();`。方法同 ArrayList。

### Collections（集合工具类，全静态，java.util）

| 方法 | 作用 |
|---|---|
| `Collections.sort(list)` `[static]` | 升序排序 |
| `Collections.sort(list, Collections.reverseOrder())` `[static]` | 降序 |
| `Collections.max(list)` / `min(list)` `[static]` | 最大 / 最小 |
| `Collections.reverse(list)` `[static]` | 反转 |
| `Collections.shuffle(list)` `[static]` | 随机打乱 |

### Arrays（数组工具类，全静态）

| 方法 | 作用 |
|---|---|
| `Arrays.sort(arr)` `[static]` | 数组排序 |
| `Arrays.toString(arr)` `[static]` | 数组转字符串打印 |
| `Arrays.fill(arr, v)` `[static]` | 全部填 v |
| `Arrays.copyOf(arr, n)` `[static]` | 复制成长度 n 的新数组 |

### Random（随机数，java.util）

| 方法 | 作用 | 返回 |
|---|---|---|
| `new Random()` | 创建 | — |
| `nextInt()` | 任意整数 | int |
| `nextInt(n)` | 0~n-1 的整数 | int |
| `nextDouble()` | 0.0~1.0 小数 | double |
| `nextBoolean()` | 随机真假 | boolean |

### Date（日期，java.util）

| 方法 | 作用 |
|---|---|
| `new Date()` | 当前时间 |
| `getTime()` | 毫秒数 |
| `toString()` | 转字符串 |
| `before(d)` / `after(d)` | 早于 / 晚于 |

---

## 三、字符串

### String（不可变字符串）

| 方法 | 作用 | 返回 |
|---|---|---|
| `length()` | 长度 | int |
| `charAt(i)` | 第 i 个字符 | char |
| `substring(a)` / `substring(a,b)` | 从 a 截到尾 / 截 [a,b) | String |
| `indexOf("x")` | 子串首次位置，无返回 -1 | int |
| `equals(s)` | 比内容是否相等（不能用 ==） | boolean |
| `equalsIgnoreCase(s)` | 忽略大小写比 | boolean |
| `compareTo(s)` | 字典序比较 | int |
| `split("分隔")` | 按分隔拆成数组 | String[] |
| `trim()` | 去首尾空格 | String |
| `toUpperCase()` / `toLowerCase()` | 转大写 / 小写 | String |
| `replace(a,b)` | 替换 | String |
| `contains("x")` | 是否含子串 | boolean |
| `startsWith/endsWith("x")` | 是否以…开头/结尾 | boolean |
| `String.valueOf(x)` `[static]` | 任意类型转字符串 | String |

### StringBuffer / StringBuilder（可变字符串，常用于拼接）

两者方法相同；StringBuffer 线程安全，StringBuilder 更快。

| 方法 | 作用 | 返回 |
|---|---|---|
| `new StringBuffer()` | 建空 | — |
| `append(x)` | 末尾追加（任意类型） | 自身 |
| `insert(i, x)` | 在 i 处插入 | 自身 |
| `delete(a,b)` | 删 [a,b) | 自身 |
| `reverse()` | 反转 | 自身 |
| `charAt(i)` / `length()` | 取字符 / 长度 | char / int |
| `toString()` | 转成 String | String |

---

## 四、包装类（基本类型↔对象）

### Integer

| 方法/成员 | 作用 | 返回 |
|---|---|---|
| `Integer.parseInt("123")` `[static]` | 字符串转 int | int |
| `Integer.valueOf(x)` `[static]` | 转 Integer 对象 | Integer |
| `Integer.toString(n)` `[static]` | int 转字符串 | String |
| `Integer.MAX_VALUE` `[常量]` | int 最大值 2147483647 | int |
| `Integer.MIN_VALUE` `[常量]` | int 最小值 | int |
| `intValue()` | 取出 int 值 | int |
| `compareTo(other)` | 比较 | int |

### Double / Boolean / Character

| 方法 | 作用 |
|---|---|
| `Double.parseDouble("3.14")` `[static]` | 字符串转 double |
| `Double.MAX_VALUE` `[常量]` | double 最大值 |
| `Boolean.parseBoolean("true")` `[static]` | 字符串转 boolean |
| `Character.isDigit(c)` `[static]` | 是否数字字符 |
| `Character.isLetter(c)` `[static]` | 是否字母 |
| `Character.toUpperCase(c)` `[static]` | 转大写字符 |

> ⚠ 包装类比较用 `.equals()` 或 `.intValue()`，别用 `==`（`==` 比的是对象地址）。

---

## 五、GUI 容器与组件（javax.swing）

### JFrame（顶层窗口）

| 方法 | 作用 |
|---|---|
| `new JFrame("标题")` | 建窗口 |
| `setTitle("标题")` | 设标题 |
| `setSize(宽,高)` | 设大小 |
| `setLayout(布局)` | 设布局管理器 |
| `add(组件)` | 加组件（JDK1.5+直接 add） |
| `getContentPane().add(组件)` | 加到内容面板（传统写法） |
| `setContentPane(面板)` | 设内容面板 |
| `setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE)` | 关窗即退出程序 |
| `setVisible(true)` | 显示窗口（必须） |
| `pack()` | 自动按组件调整大小 |
| `JFrame.EXIT_ON_CLOSE` `[常量]` | 关闭即退出 |

### JPanel（面板，装组件用，默认 FlowLayout）

| 方法 | 作用 |
|---|---|
| `new JPanel()` | 建面板 |
| `add(组件)` | 加组件 |
| `setLayout(布局)` | 设布局 |
| `paintComponent(Graphics g)` | （重写）在面板上绘图 |

### JLabel（标签，显示文字/图片）

| 方法 | 作用 |
|---|---|
| `new JLabel("文字")` | 建标签 |
| `setText("新文字")` | 改文字 |
| `getText()` | 取文字 |

### JButton（按钮）

| 方法 | 作用 |
|---|---|
| `new JButton("文字")` | 建按钮 |
| `addActionListener(监听器)` | 注册点击监听 |
| `setText("文字")` / `getText()` | 改 / 取文字 |
| `setBounds(x,y,宽,高)` | 设绝对位置（需 setLayout(null)） |
| `setEnabled(false)` | 禁用 |

### JTextField（单行输入框）

| 方法 | 作用 |
|---|---|
| `new JTextField(列数)` | 建输入框 |
| `getText()` | 取输入内容 |
| `setText("x")` | 设内容 |
| `addActionListener(...)` | 注册回车监听 |

### JTextArea（多行文本区）

| 方法 | 作用 |
|---|---|
| `new JTextArea(行,列)` | 建文本区 |
| `append("文字")` | 末尾追加 |
| `getText()` / `setText(s)` | 取 / 设全部内容 |
| `setEditable(false)` | 设为只读 |

### JScrollPane（滚动条容器）

| 方法 | 作用 |
|---|---|
| `new JScrollPane(组件)` | 给 JTextArea 等加滚动条（直接 add 文本区不会滚） |

### JOptionPane（弹窗对话框，全静态）

| 方法 | 作用 | 返回 |
|---|---|---|
| `showMessageDialog(null, "内容")` `[static]` | 提示弹窗 | void |
| `showInputDialog(null, "提示")` `[static]` | 输入弹窗 | String |
| `showConfirmDialog(null, "确认?")` `[static]` | 确认弹窗（是/否/取消） | int |
| `showOptionDialog(...)` `[static]` | 自定义选项弹窗 | int |

### JComboBox / JCheckBox / JRadioButton / ButtonGroup

| 类·方法 | 作用 |
|---|---|
| `new JComboBox<>(数组)` | 下拉框 |
| `JComboBox.getSelectedItem()` | 取选中项 |
| `new JCheckBox("文字")` | 复选框 |
| `JCheckBox.isSelected()` | 是否勾选 |
| `new JRadioButton("文字")` | 单选按钮 |
| `new ButtonGroup()` + `bg.add(单选按钮)` | 让单选按钮互斥（必须，否则能同时选中） |

---

## 六、布局管理器（java.awt）

| 类 | 构造 / 用法 | 特点 |
|---|---|---|
| FlowLayout | `new FlowLayout()` | 从左到右排，满了换行；JPanel 默认。参数 `FlowLayout.LEFT/CENTER/RIGHT` 对齐 |
| BorderLayout | `new BorderLayout()` + `add(组件,"North")` | 东南西北中 5 区；JFrame 默认。区名：North/South/East/West/Center |
| GridLayout | `new GridLayout(行,列)` | 等大网格，从左到右、上到下填 |
| CardLayout | `new CardLayout()` | 卡片叠放，一次显一张；`cl.next(面板)` 切换 |
| GridBagLayout | `new GridBagLayout()` + GridBagConstraints | 最灵活，组件可不等大、跨行列 |
| 通用 | `setLayout(new XxxLayout())` | 换布局只改这一行；`setLayout(null)` 用绝对坐标 |

---

## 七、绘图（java.awt，在 paintComponent(Graphics g) 里用）

### Graphics

| 方法 | 作用 |
|---|---|
| `setColor(Color.RED)` | 设画笔颜色 |
| `setFont(new Font(...))` | 设字体 |
| `drawLine(x1,y1,x2,y2)` | 画线段 |
| `drawRect(x,y,宽,高)` / `fillRect(...)` | 空心 / 实心矩形 |
| `drawOval(x,y,宽,高)` / `fillOval(...)` | 空心 / 实心椭圆 |
| `drawArc(x,y,宽,高,起角,弧度)` | 圆弧 |
| `drawPolygon(int[]x,int[]y,n)` / `fillPolygon(...)` | 空心 / 实心多边形 |
| `drawString("字",x,y)` | 写文字 |

### Color（颜色，多为常量成员）

| 成员 | 说明 |
|---|---|
| `Color.RED/BLUE/GREEN/BLACK/WHITE/GRAY/YELLOW` `[常量]` | 预定义颜色 |
| `new Color(r,g,b)` | 自定义 RGB（0~255） |

### Font（字体）

| 用法 | 说明 |
|---|---|
| `new Font("宋体", Font.BOLD, 20)` | 字体名、样式、字号 |
| `Font.PLAIN/BOLD/ITALIC` `[常量]` | 常规 / 粗体 / 斜体 |

> 改了内容要刷新：调 `repaint()` 触发重绘。

---

## 八、事件处理（java.awt.event）

### 监听接口与必须实现的方法

| 接口 | 必须实现的方法 | 触发时机 |
|---|---|---|
| ActionListener | `actionPerformed(ActionEvent e)` | 按钮点击 / 文本框回车（只1个方法，无 Adapter） |
| MouseListener | `mouseClicked/Pressed/Released/Entered/Exited`（5个） | 鼠标点击等 |
| MouseMotionListener | `mouseDragged` / `mouseMoved`（2个） | 鼠标移动/拖动 |
| KeyListener | `keyPressed` / `keyReleased` / `keyTyped`（3个） | 键盘 |
| WindowListener | `windowClosing` 等（7个） | 窗口操作 |
| ItemListener | `itemStateChanged(ItemEvent e)` | 复选/单选/下拉改变 |

### Adapter（适配器：空实现接口的类，只重写要的方法）

| 类 | 对应接口 | 说明 |
|---|---|---|
| MouseAdapter | MouseListener / MouseMotionListener | 继承它只写 mouseClicked 等需要的 |
| KeyAdapter | KeyListener | 同理 |
| WindowAdapter | WindowListener | 常用 windowClosing |

> ActionListener 只1个方法，没有 Adapter。

### 事件对象（拿信息用）

| 类·方法 | 作用 |
|---|---|
| `ActionEvent.getActionCommand()` | 取按钮文字 |
| `ActionEvent.getSource()` | 取事件源组件（多组件时区分谁触发） |
| `MouseEvent.getX()` / `getY()` | 取鼠标坐标 |
| `KeyEvent.getKeyCode()` | 取按键码 |

### 注册监听（通用）

`组件.addActionListener(监听器)` / `addMouseListener(...)` / `addKeyListener(...)` / `addWindowListener(...)`

---

## 九、多线程（java.lang）

### Thread（线程类）

| 方法/用法 | 作用 |
|---|---|
| `class X extends Thread` + 重写 `run()` | 方式一：继承 |
| `start()` | 启动线程（自动调 run，开新线程） |
| `run()` | 线程要做的事（直接调它=普通方法，不开线程！） |
| `Thread.sleep(毫秒)` `[static]` | 当前线程睡眠 |
| `getName()` / `setName(s)` | 取 / 设线程名 |
| `join()` | 等该线程结束 |
| `isAlive()` | 是否在运行 |

### Runnable（接口，推荐方式）

| 用法 | 作用 |
|---|---|
| `class X implements Runnable` + 实现 `run()` | 方式二：实现接口 |
| `new Thread(new X()).start()` | 包成 Thread 再启动 |

> ⚠ 启动必须 `start()` 不是 `run()`；继承 Thread 不能再继承别的类，用 Runnable 更灵活。

---

## 十、数据库 JDBC（java.sql）

### DriverManager（取连接，静态）

| 方法 | 作用 | 返回 |
|---|---|---|
| `Class.forName("驱动类名")` `[static]` | 加载驱动 | — |
| `DriverManager.getConnection(url, 用户, 密码)` `[static]` | 建立连接 | Connection |

### Connection（数据库连接）

| 方法 | 作用 | 返回 |
|---|---|---|
| `createStatement()` | 建 Statement | Statement |
| `prepareStatement("带?的SQL")` | 建 PreparedStatement | PreparedStatement |
| `prepareCall("{call 过程(?,?)}")` | 建 CallableStatement（调存储过程） | CallableStatement |
| `close()` | 关闭连接 | void |

### Statement（执行 SQL）

| 方法 | 作用 | 返回 |
|---|---|---|
| `executeUpdate("INSERT/UPDATE/DELETE")` | 执行增删改 | int(受影响行数) |
| `executeQuery("SELECT")` | 执行查询 | ResultSet |
| `close()` | 关闭 | void |

### PreparedStatement（预编译，带 ? 占位，防注入）

| 方法 | 作用 |
|---|---|
| `setInt(序号, 值)` | 给第几个 ? 填 int（序号从1开始） |
| `setString(序号, 值)` | 给 ? 填字符串 |
| `executeUpdate()` / `executeQuery()` | 执行 |
| `addBatch()` / `executeBatch()` | 加入批处理 / 批量执行 |

### ResultSet（查询结果集）

| 方法 | 作用 | 返回 |
|---|---|---|
| `next()` | 移到下一行，有数据返回 true | boolean |
| `getString("列名")` / `getString(列号)` | 取该列字符串 | String |
| `getInt("列名")` | 取该列整数 | int |
| `getDouble("列名")` | 取该列小数 | double |
| `close()` | 关闭 | void |

> 遍历套路：`while(rs.next()){ rs.getString("列名"); }`

---

## 十一、异常类（java.lang，常见）

| 异常类 | 什么时候抛 |
|---|---|
| Exception | 所有受检异常的父类，`catch(Exception e)` 兜底 |
| RuntimeException | 运行时异常父类 |
| NullPointerException | 用了 null 对象（空指针） |
| ArrayIndexOutOfBoundsException | 数组下标越界 |
| ArithmeticException | 算术错误（如除以 0） |
| NumberFormatException | parseInt 转非数字字符串 |
| ClassNotFoundException | 找不到类（如 JDBC 驱动没加载） |
| IOException | IO 操作出错（文件读写） |
| FileNotFoundException | 文件不存在 |
| SQLException | 数据库操作出错 |

### 异常对象常用方法

| 方法 | 作用 |
|---|---|
| `getMessage()` | 取错误信息字符串 |
| `printStackTrace()` | 打印完整错误堆栈（定位错误行） |
| `toString()` | 异常类名+信息 |
