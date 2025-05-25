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

- 1、编写如下界面

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
          this.setLocationRelativeTo(null);

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

- 2、编写一个程序实现用户登录界面

  ![20250525233600-2025-05-25-23-36-00](https://cdn.jsdelivr.net/gh/xiaozhuABCD1234/mybolg@imgs/20250525233600-2025-05-25-23-36-00.png)
