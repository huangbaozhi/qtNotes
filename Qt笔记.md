
## QPushButton三态

调用方式
```c++
QPushButton *pBtn = new QPushButton(this);
pBtn->SetIcons(":/icon/previous.png",
                ":/icon/previous_hover.png",
                ":/icon/previous_press.png");
```
继承QPushButton实现
.h文件
```C++
#include <QPushButton>

class PicButton : public QPushButton
{
public:
    PicButton(QWidget *parent = nullptr);
    void SetIcons(const QString& normal,
                  const QString& hover, const QString& pressed);

protected:
    bool event(QEvent *e) override;

private:
    void setNormalIcon();
    void setHoverIcon();
    void setPressIcon();
    QString _normal;
    QString _hover;
    QString _pressed;

};
```
.cpp文件
```C++
#include "picbutton.h"
#include <QEvent>
PicButton::PicButton(QWidget *parent)
{

}

void PicButton::SetIcons(const QString &normal, const QString &hover, const QString &pressed)
{
    _normal = normal;
    _hover = hover;
    _pressed = pressed;

    QPixmap tmpPixmap;
    tmpPixmap.load(normal);
    this->resize(tmpPixmap.size());
    this->setIcon(tmpPixmap);
    this->setIconSize(tmpPixmap.size());
}


void PicButton::setNormalIcon(){
    QPixmap tmpPixmap;
    tmpPixmap.load(_normal);
    this->setIcon(tmpPixmap);
}

void PicButton::setHoverIcon(){
    QPixmap tmpPixmap;
    tmpPixmap.load(_hover);
    this->setIcon(tmpPixmap);
}

void PicButton::setPressIcon(){
    QPixmap tmpPixmap;
    tmpPixmap.load(_pressed);
    this->setIcon(tmpPixmap);
}

bool PicButton::event(QEvent *event)
{
    switch (event->type())
        {
        case QEvent::Enter:
            setHoverIcon();
            break;
        case QEvent::Leave:
            setNormalIcon();
            break;
        case QEvent::MouseButtonPress:
            setPressIcon();
            break;
        case QEvent::MouseButtonRelease:
            setHoverIcon();
            break;
        default:
            break;
        }
         return QPushButton::event(event);
}
```

### dynamic_cast
**dynamic_cast** 是一种运行时类型检查，用于在类继承体系中安全地进行类型转换

### 析构释放内存
保证下一页的窗体中的数据不与上一页的重复。

### 防止子窗口重复
1. 窗口模态：无法操作另外窗口，若不关掉此窗口，则无法操作。
2. 非模态窗口：可重复出现窗口

#### 解决重复出现窗口问题：
先创建一个空指针，在创建窗体的时候判断该指针是否为NULL，如果为NULL，表示窗体未创建，则创建窗体；如果不为NULL，则表示已经创建窗体，则不再创建新窗体。
**用null作为判断的条件,会默认返回false**
```C+++
// class A;
// class B;
 
void A::openB() {
    if(!mB) { // 如果为空，则需要先创建该对象
        mB = new B(this);
    }
        mB->show(); // 或者其他打开一个窗口的函数
}
```
分页的情况：需要释放内存
```C++
delete B;
B = nullptr;
```
保证下一页的窗体中的数据不与上一页的重复。

### Qt按钮左边图片右边文字
样式：
```C++
    previewCaseButton->setStyleSheet("QToolButton{"
                                     "background: #72977B;"
                                     "border-radius: 29px;"
                                     "font-family: Microsoft YaHei;"
                                     "font-weight: 400;"
                                     "font-size: 25px;"
                                     "color: #FFFFFF;"
                                     "line-height: 23px;"
                                     "background-image: url(:/images/icon/new_preview.png);"
                                     "background-repeat: no-repeat;"
                                     "background-origin: padding;"
                                     "background-position: left;"
                                     "padding-left:0px;"
                                     "border: none;"
                                     "text-align:left;"
                                     "}");
```
### 设置当前时间
实现方式：
```c++
    // 当前时间
    QString styleTime = QString("QLabel{font-family: Microsoft YaHei;\
                                font-weight: 300;\
                                font-size: 21px;\
                                color: #666666;\
                                line-height: 38px;}");

    pTimeLbl = new QLabel(this);
    pTimeLbl->setGeometry(60, 1860, 218, 25);
    pTimeLbl->setStyleSheet(styleTime);
    pTimeLbl->setAlignment(Qt::AlignCenter);

    pWeekLbl = new QLabel(this);
    pWeekLbl->setGeometry(60, 1888, 218, 25);
    pWeekLbl->setStyleSheet(styleTime);
    pWeekLbl->setAlignment(Qt::AlignCenter);

    pDataLbl = new QLabel(this);
    pDataLbl->setGeometry(60, 1923, 218, 25);
    pDataLbl->setStyleSheet(styleTime);
    pDataLbl->setAlignment(Qt::AlignCenter);

    slotUpdateTime();

    QTimer* time = new QTimer(this);
    connect(time, &QTimer::timeout, this, &Widget::slotUpdateTime);
    time->start(1000);
```
槽函数
```c++
void Widget::slotUpdateTime()
{
    QDateTime curTime = QDateTime::currentDateTime();
    pTimeLbl->setText(curTime.toString("hh:mm:ss"));
    pWeekLbl->setText(curTime.toString("dddd"));
    pDataLbl->setText(curTime.toString("yyyy-MM-dd"));

}
```

### 加密
添加头文件 #include< QCryptographicHash>
```c++
                QString MD5;
                QString passWd = newPasswdEdit->text();
                QByteArray str;
                str = QCryptographicHash::hash(passWd.toLatin1(),QCryptographicHash::Md5);
                MD5.append(str.toHex());
```