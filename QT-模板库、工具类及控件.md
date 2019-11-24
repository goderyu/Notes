[TOC]

# QVariant类

类似于C++中的`union`数据类型。

QVariant::type枚举类型变量

| 变量               | 对应类型  | 变量                 | 对应类型       |
| ------------------ | --------- | -------------------- | -------------- |
| QVariant::Invalid  | 无效类型  | QVariant::Time       | QTime          |
| QVariant::Region   | QRegion   | QVariant::Line       | QLine          |
| QVariant::Bitmap   | QBitmap   | QVariant::Palette    | QPalette       |
| QVariant::Bool     | bool      | QVariant::List       | QList          |
| QVariant::Brush    | QBrush    | QVariant::SizePolicy | QSizePolicy    |
| QVariant::Size     | QSize     | QVariant::String     | QString        |
| QVariant::Char     | QChar     | QVariant::Map        | QMap           |
| QVariant::Color    | QColor    | QVariant::StringList | QStringList    |
| QVariant::Cursor   | QCursor   | QVariant::Point      | QPoint         |
| QVariant::Date     | QDate     | QVariant::Pen        | QPen           |
| QVariant::DateTime | QDateTime | QVariant::Pixmap     | QPixmap        |
| QVariant::Double   | double    | QVariant::Rect       | QRect          |
| QVariant::Font     | QFont     | QVariant::Image      | QImage         |
| QVariant::Icon     | QIcon     | QVariant::UserType   | 用户自定义类型 |

# 算法与正则表达式

`qAbs/qMax/qRound/qSwap`，其中`qRound`是将浮点数四舍五入为整数的。

`QRegExp`类是正则表达式的表示类，由**表达式**、**量词**和**断言**组成。

正则表达式的量词

| 量词 | 含义          | 量词   | 含义           |
| ---- | ------------- | ------ | -------------- |
| E?   | 匹配0次或1次  | E[n,]  | 至少匹配n次    |
| E+   | 匹配1次或多次 | E[,m]  | 最多匹配m次    |
| E*   | 匹配0次或多次 | E[n,m] | 至少n次最多m次 |
| E[n] | 匹配n次       |        |                |

正则表达式的断言

| 符号 | 含义                     | 符号  | 含义                      |
| ---- | ------------------------ | ----- | ------------------------- |
| ^    | 表示在字符串开头进行匹配 | \B    | 非单词边界                |
| $    | 表示在字符串结尾进行匹配 | (?=E) | 表示表达式后紧跟E才匹配   |
| \b   | 单词边界                 | (?!E) | 表示表达式后不跟随E才匹配 |

`\s`在表达式中表示一个空白字符

# 模型视图

视图与模型绑定时，模型必须使用`new`创建，否则视图不能随着模型的改变而改变。

# QString中浮点数设置格式问题

```c++
QString QString::arg(double a, int fieldWidth = 0, char format = 'g', int precision = -1, QChar fillChar = QLatin1Char(' ')) const
```

```c++
[static] QString QString::number(double n, char format = 'g', int precision = 6)
```

以上两个方法是QString中用到double时最常用的方法。其中的precision是精确度，和format有关。如果format设置为'e','E','f'，则precision指示小数点后的位数；如果format设置为'g','G'，precision指示整数部分与小数部分位数和。

> 官方文档中的解释如下：
> Argument Formats
> In member functions where an argument *format* can be specified (e.g., arg(), number(), the argument *format* can be one of the following:
> 
> | Format | Meaning                                          |
| ------ | ------------------------------------------------ |
| e      | format as [-]9.9e[+\|-]999                       |
| E      | format as [-]9.9E[+\|-]999                       |
| f      | format as [-]9.9                                 |
| g      | use e or f format, whichever is the most concise |
| G      | use E or f format, whichever is the most concise |
> A *precision* is also specified with the argument *format*. For the 'e', 'E', and 'f' formats, the *precision* represents the number of digits *after* the decimal point. For the 'g' and 'G' formats, the *precision* represents the maximum number of significant digits (trailing zeroes are omitted). 

# 综合小例子

使用`QLineEdit`控件，为控件设置了正则表达式，只能按照限制字元进行输入。用到了`QLineEdit::setValidator(QValidator* v)`，对输入框的输入判断使用了`QLineEdit::hasAcceptableInput()`。

```c++
Dialog::Dialog(QWidget *parent)
    : QDialog(parent)
    , ui(new Ui::Dialog) {
    ui->setupUi(this);
    QRegExp regExp("[A-Za-z][1-9][0-9]{0,2}");
    ui->lineEdit->setValidator(new QRegExpValidator(regExp, this));
    connect(ui->lineEdit, &QLineEdit::textChanged, [=]() {
        ui->okButton->setEnabled(ui->lineEdit->hasAcceptableInput());
    });
}
```

