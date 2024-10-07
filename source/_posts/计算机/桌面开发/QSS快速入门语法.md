---
title: QSS快速入门语法
img: https://images.pexels.com/photos/11582120/pexels-photo-11582120.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500
excerpt: Qt的桌面开发
top: false
toc: true
tocOpen: true
onlyTitle: false
comments: true
share: true
copyright: true
donate: false
prismjs: default
categories: 计算机
tags: [GUI开发,QT6,跨平台桌面开发,QSS]
mathjax: true
date: 2022-05-21 01:59:22
---

# QSS快速入门语法

## 一、基础语法

> 版本 Qt6.3 官网文档：<https://doc.qt.io/qt-6/stylesheet.html>

### 1.选择器

> 基本使用

```css
选择器 {
  属性: 属性值;
}
```

> 选择器的种类

| 选择器     | 实例                        | 作用                                                                                                                                                                                                                                                                                        |
| ---------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 通配选择器 | `*`                         | 匹配所有组件                                                                                                                                                                                                                                                                                |
| 类型选择器 | `QPushButton`               | 匹配[QPushButton](https://doc.qt.io/qt-6/qpushbutton.html) 及其子类的实例                                                                                                                                                                                                                   |
| 属性选择器 | `QPushButton[flat="false"]` | 匹配非`flat`的`QPushButton`实例。您可以使用此选择器测试任何支持`QVariant::toString()`的Qt`property`(有关详细信息，请参阅`toString()`函数文档)。此外，对于类的名称，还支持特殊的`class`属性。此选择器也可用于测试动态特性。有关使用动态特性进行自定义的详细信息。除了`=`，您还可以使用`~=`。 |
| 类选择器   | `.QPushButton`              | 匹配[QPushButton](https://doc.qt.io/qt-6/qpushbutton.html)的实例，但不匹配其子类的实例。 这相当于 `*[class~="QPushButton"]`                                                                                                                                                                 |
| ID选择器   | `QPushButton#okButton`      | 匹配对象名称为`okButton`的所有[QPushButton](https://doc.qt.io/qt-6/qpushbutton.html)实例。                                                                                                                                                                                                  |
| 后代选择器 | `QDialog QPushButton`       | 匹配[QPushButton](https://doc.qt.io/qt-6/qpushbutton.html)的所有实例，这些实例是[QDialog](https://doc.qt.io/qt-6/qdialog.html)的子代（子代、孙代等）                                                                                                                                        |
| 子代选择器 | `QDialog > QPushButton`     | 匹配作为[QDialog](https://doc.qt.io/qt-6/qdialog.html)直接子级的[QPushButton](https://doc.qt.io/qt-6/qpushbutton.html) 的所有实例                                                                                                                                                           |

> 访问子控件

对于复杂的小部件的样式，有必要访问小部件的子控件，例如 QComboBox 的下拉按钮或 QSpinBox 的上下箭头。选择器可能包含子角色，使其能够将规则的应用限制到特定的小部件子角色。例如：

```css
QComboBox::drop-down { image: url(dropdown.png) }
```

### 2.伪状态

选择器可能包含伪状态，表示根据小部件的状态限制规则的应用。伪状态显示在选择器的末尾，中间有冒号（：）

```css
QPushButton:hover { color: white }
```

伪状态可以使用感叹号运算符求反。

```css
QRadioButton:!hover { color: red }
```

伪状态可以增加约束，在这种情况下，类似逻辑AND。下面例子表示鼠标悬停在被选中的QCheckBox上

```css
QCheckBox:hover:checked { color: white }

QPushButton:hover:!pressed { color: blue; }
```

如果需要，可以使用逗号运算符表示逻辑OR

```css
QCheckBox:hover, QCheckBox:checked { color: white }
```

伪状态可以与子控件一起出现。

```css
QComboBox::drop-down:hover { image: url(dropdown_bright.png) }
```

**样式冲突问题**：

选择器谁指定的控件更具体按照谁的标准

**样式继承问题**：

控件并不会自动的继承父类的样式，必须显式指定控件样式
