
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