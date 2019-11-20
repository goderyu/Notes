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

