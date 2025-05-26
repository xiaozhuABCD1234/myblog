---
title: Java图形用户界面设计
abbrlink: 1ac398c9
date: 2025-05-25 21:53:16
updated: 2025-05-25 21:53:16
tags:
  - java
  - swing
categories: 笔记
keywords:
cover:
---

## 作业要求

- 1 编写如下界面

  ![20250525224211-2025-05-25-22-42-11.png](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/mybolg@imgs/20250525224211-2025-05-25-22-42-11.png)

  当用户点击”Click Me”按钮，显示消息对话框,消息为” Click Me 按钮被点击”。

  ```java
  package ex6_1;

  import javax.swing.\*;

  public class ClickButton extends JFrame {

      private JButton button = new JButton("Click Me");

      public ClickButton() {
          this.setTitle("点击按钮");
          this.setSize(800, 450);
          this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

          // 使用Lambda表达式处理按钮点击事件
          button.addActionListener(e -> {
              JOptionPane.showMessageDialog(
                      this,
                      "Click Me 按钮被点击",
                      "提示",
                      JOptionPane.INFORMATION_MESSAGE);
          });

          this.add(button);
      }

      public static void main(String[] args) {
          SwingUtilities.invokeLater(() -> {
              new ClickButton().setVisible(true);
          });
      }

  }
  ```

  ![20250526091004-2025-05-26-09-10-05](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526091004-2025-05-26-09-10-05.png)

- 2 编写一个程序实现用户登录界面

  ![20250525233600-2025-05-25-23-36-00](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/mybolg@imgs/20250525233600-2025-05-25-23-36-00.png)

  当用户登录按下确定键，判断用户是否录入了用户名与密码，如果没有按或用户名不为 admin 密码不为 1234 都需要提示错误。

  ```java
  package ex6_2;

  import java.awt.*;
  import java.awt.event.ActionEvent;
  import java.awt.event.ActionListener;
  import java.util.Enumeration;

  import javax.swing.*;
  import javax.swing.plaf.FontUIResource;

  public class Login implements ActionListener {
      private JFrame frame;
      private JTextField userNameTextField;
      private JPasswordField pwdTextField;
      private JButton confirm = new JButton("确认");
      private JButton cancel = new JButton("取消");

      public Login() {
          frame = new JFrame("登录页面");
          frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
          frame.setSize(800, 450);

          JPanel jp = new JPanel();
          jp.setLayout(new GridLayout(3, 2, 20, 20));

          jp.add(new JLabel("用户名："));
          userNameTextField = new JTextField();
          jp.add(userNameTextField);

          jp.add(new JLabel("密码："));
          pwdTextField = new JPasswordField();
          jp.add(pwdTextField);

          confirm.addActionListener(this);
          jp.add(confirm);

          cancel.addActionListener(this);
          jp.add(cancel);

          frame.add(jp);
          frame.setVisible(true);
      }

      @Override
      public void actionPerformed(ActionEvent e) {
          if (e.getSource() == confirm) {
              loginAction();
          } else if (e.getSource() == cancel) {
              frame.dispose(); // 关闭窗口
          }
      }

      private void loginAction() {
          String username = userNameTextField.getText();
          String pwd = new String(pwdTextField.getPassword());
          if (username.equals("admin") && pwd.equals("1234")) {
              JOptionPane.showMessageDialog(frame, "登录成功!", "提示",
                      JOptionPane.INFORMATION_MESSAGE);
          } else {
              JOptionPane.showMessageDialog(frame, "登录失败!", "错误",
                      JOptionPane.WARNING_MESSAGE);
          }
          pwdTextField.setText("");
      }

      private static void InitGlobalFont(Font font) {
          FontUIResource fontRes = new FontUIResource(font);
          for (Enumeration<Object> keys = UIManager.getDefaults().keys(); keys.hasMoreElements();) {
              Object key = keys.nextElement();
              Object value = UIManager.get(key);
              if (value instanceof FontUIResource) {
                  UIManager.put(key, fontRes);
              }
          }
      }

      public static void main(String[] args) {
          InitGlobalFont(new Font("Maple Mono NF CN", Font.PLAIN, 16));
          SwingUtilities.invokeLater(Login::new);
      }
  }
  ```

  ![登录成功](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526091901-2025-05-26-09-19-02.png)

  ![登陆失败](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/登陆失败-2025-05-26-09-21-16.png)

- 3 利用合适的布局和 Swing 控件完成下题

  按照界面使用相应控件与合适的布局完成下题，要求按生成随机数按纽产生三个随机整数 **0** 到 **100** 之间，按计算平均数按纽计算平均值，如图所示，初始界面

  ![20250526092343-2025-05-26-09-23-43](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526092343-2025-05-26-09-23-43.png)

  ```java
  package ex6_3;

  import java.awt.*;
  import java.util.Enumeration;
  import javax.swing.*;
  import java.awt.event.*;
  import javax.swing.plaf.FontUIResource;

  public class CalculateAverage extends JFrame implements ActionListener {
      private JTextField[] textFields;
      private JTextField resultField;
      private JButton button1, button2;

      public CalculateAverage() {
          this.setTitle("三个随机数字");
          this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
          this.setSize(400, 300);

          JPanel jp = new JPanel();
          textFields = new JTextField[3];
          for (int i = 0; i < 3; i++) {
              textFields[i] = new JTextField(10);
              textFields[i].setEditable(false);
          }

          jp.setLayout(new GridLayout(5, 2, 10, 10));
          for (int i = 0; i < 3; i++) {
              jp.add(new JLabel("随机数字" + (i + 1) + ":"));
              jp.add(this.textFields[i]);
          }

          jp.add(new JLabel("三数平均值"));
          this.resultField = new JTextField();
          resultField.setEditable(false);
          jp.add(this.resultField);

          button1 = new JButton("生成随机数");
          button1.addActionListener(this);
          button2 = new JButton("计算平均数");
          button2.addActionListener(this);
          jp.add(button1);
          jp.add(button2);

          this.add(jp);
          this.setVisible(true);
      }

      @Override
      public void actionPerformed(ActionEvent e) {
          if (e.getSource() == button1) {
              for (int i = 0; i < 3; i++) {
                  this.textFields[i].setText("" + (int) (Math.random() * 100));
              }
              this.resultField.setText("");
          } else {
              double sum = 0;
              for (int i = 0; i < 3; i++) {
                  sum += Integer.parseInt(this.textFields[i].getText());
              }
              this.resultField.setText("" + sum / 3);
          }
      }

      public static void main(String[] args) {
          SwingUtilities.invokeLater(() -> new CalculateAverage());
      }
  }
  ```

  ![20250526103424-2025-05-26-10-34-24](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526103424-2025-05-26-10-34-24.png)

- 4 编写程序实现如下界面

  实现事件如果按下座位 i 就在控制台中显示“座位 i 被选中” 例如按下 “座位 0“，则输出座位 0 被选中”

  ![20250526105209-2025-05-26-10-52-10](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526105209-2025-05-26-10-52-10.png)

  ```java
  package ex6_4;

  import javax.swing.*;

  import java.awt.*;

  public class Classroom extends JFrame {
      public Classroom() {
          setTitle("座位");
          setSize(400, 300);
          setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

          // 使用BorderLayout作为主布局，并设置组件间距
          setLayout(new BorderLayout(0, 0));

          // 创建讲台面板（顶部）
          JPanel podiumPanel = new JPanel(new GridLayout(1, 1));
          JButton podiumButton = new JButton("讲台");
          podiumPanel.add(podiumButton);
          add(podiumPanel, BorderLayout.NORTH);

          // 创建座位面板（中心）
          JPanel seatPanel = new JPanel(new GridLayout(2, 3, 0, 0));
          for (int i = 0; i < 6; i++) {
              JButton btn = new JButton("座位 " + i);
              btn.addActionListener(e -> {
                  String seat = ((JButton) e.getSource()).getText();
                  System.out.println(seat + " 被选中");
                  JOptionPane.showMessageDialog(btn, seat + " 被选中");
              });
              seatPanel.add(btn);
          }
          add(seatPanel, BorderLayout.CENTER);

          setVisible(true);
      }

      public static void main(String[] args) {
          SwingUtilities.invokeLater(() -> new Classroom());
      }
  }
  ```

  ![20250526105323-2025-05-26-10-53-24](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526105323-2025-05-26-10-53-24.png)

- 5 完成以下窗体制作

  ![20250526105439-2025-05-26-10-54-40](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526105439-2025-05-26-10-54-40.png)

  ```java
  package ex6_5;

  import java.awt.*;
  import java.util.Enumeration;

  import javax.swing.*;
  import javax.swing.plaf.FontUIResource;

  public class LibraryUserManager extends JFrame {
      private JTextField name;
      private JComboBox<String> identity; // 新增身份字段
      private JComboBox<String> department; // 新增单位字段
      private JTextField idNumber; // 新增证件号字段
      private JTextField regDate; // 新增注册日期字段
      private JTextField expiryDate; // 新增有效日期字段
      private JComboBox<String> gender;

      public LibraryUserManager() {
          // 设置系统属性，使用系统默认的窗口装饰
          System.setProperty("awt.useSystemAAFontSettings", "on");
          System.setProperty("swing.aatext", "true");
          System.setProperty("swing.defaultlaf", UIManager.getSystemLookAndFeelClassName());
          setSize(300, 450);
          setLayout(new BorderLayout());
          setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); // 添加关闭操作

          // 顶部标题
          add(new JLabel("图书证注册", JLabel.CENTER), BorderLayout.NORTH);

          // 表单主体
          JPanel form = new JPanel(new GridLayout(7, 2, 10, 10));

          // 姓名
          form.add(new JLabel("姓名："));
          name = new JTextField();
          form.add(name);

          // 性别
          form.add(new JLabel("性别："));
          gender = new JComboBox<>(new String[] { "男", "女" });
          form.add(gender);

          // 身份
          form.add(new JLabel("身份："));
          identity = new JComboBox<>(new String[] { "学生", "老师", "管理员" });
          form.add(identity);

          // 单位
          form.add(new JLabel("单位："));
          department = new JComboBox<>(new String[] { "计算机系", ".系", "..系", "...系" });
          form.add(department);

          // 证件号码
          form.add(new JLabel("证件号码："));
          idNumber = new JTextField();
          form.add(idNumber);

          // 注册日期
          form.add(new JLabel("注册日期："));
          regDate = new JTextField();
          form.add(regDate);

          // 有效日期
          form.add(new JLabel("有效日期："));
          expiryDate = new JTextField();
          form.add(expiryDate);

          add(form, BorderLayout.CENTER);

          JPanel buttons = new JPanel();
          buttons.setLayout(new GridLayout(1, 4, 2, 2));
          buttons.add(new JButton("添加"));
          buttons.add(new JButton("删除"));
          buttons.add(new JButton("撤销"));
          buttons.add(new JButton("退出"));

          add(buttons,BorderLayout.SOUTH);

          setVisible(true);
      }

      public static void main(String[] args) {
          SwingUtilities.invokeLater(() -> new LibraryUserManager());
      }
  }
  ```

  ![20250526110126-2025-05-26-11-01-26](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526110126-2025-05-26-11-01-26.png)

- 6 完成以下窗体制作（使用 null 布局）

  ![20250526110504-2025-05-26-11-05-04](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526110504-2025-05-26-11-05-04.png)

  ```java
  package ex6_6;

  import javax.swing.*;
  import java.awt.*;

  public class StudentRegistrationQueryFrame extends JFrame {

      public StudentRegistrationQueryFrame() {
          setTitle("学生注册查询");
          setSize(600, 300);
          setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
          setLayout(null);

          JLabel idLabel = new JLabel("学号");
          idLabel.setBounds(100, 50, 50, 20);
          add(idLabel);

          JTextField idTextField = new JTextField("jTextField1");
          idTextField.setBounds(160, 50, 100, 20);
          add(idTextField);

          JLabel nameLabel = new JLabel("姓名");
          nameLabel.setBounds(270, 50, 50, 20);
          add(nameLabel);

          JTextField nameTextField = new JTextField("jTextField1");
          nameTextField.setBounds(330, 50, 100, 20);
          add(nameTextField);

          JLabel departmentLabel = new JLabel("系别");
          departmentLabel.setBounds(100, 100, 50, 20);
          add(departmentLabel);

          JTextField departmentTextField = new JTextField("jTextField1");
          departmentTextField.setBounds(160, 100, 100, 20);
          add(departmentTextField);

          JLabel majorLabel = new JLabel("专业");
          majorLabel.setBounds(270, 100, 50, 20);
          add(majorLabel);

          JTextField majorTextField = new JTextField("jTextField1");
          majorTextField.setBounds(330, 100, 100, 20);
          add(majorTextField);

          JLabel addressLabel = new JLabel("地址");
          addressLabel.setBounds(100, 150, 50, 20);
          add(addressLabel);

          JTextField addressTextField = new JTextField("jTextField1");
          addressTextField.setBounds(160, 150, 270, 20);
          add(addressTextField);

          JButton addButton = new JButton("增加");
          addButton.setBounds(100, 200, 100, 30);
          add(addButton);

          JButton queryButton = new JButton("查询");
          queryButton.setBounds(250, 200, 100, 30);
          add(queryButton);

          JButton cancelButton = new JButton("取消");
          cancelButton.setBounds(400, 200, 100, 30);
          add(cancelButton);

          setVisible(true);
      }

      public static void main(String[] args) {
          new StudentRegistrationQueryFrame();
      }
  }
  ```

  ![20250526112048-2025-05-26-11-20-48](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/myblog@imgs/20250526112048-2025-05-26-11-20-48.png)
