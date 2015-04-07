## QHexEdit

* QHexEdit v0.6.3     
* Authored by: Winfried Simon (Winni7fingic)
* Hosted at google codes: 

    http://code.google.com/p/qhexedit2/

* Published at qt-apps.org.

    http://qt-apps.org/content/show.php/QHexEdit?content=133189

* A git repo is based on v0.6.3 and ported to QT5

   All svn change record before v0.6.3 is not migrated to git.
   
## Winfried Simon's reply on license issue

I transferred the QHexEdit2 to https://github.com/Simsys/qhexedit2 and I will maintain it there.

The code is already licensed under LGPL 2.1, so I do not understand, what you want to change.

See: https://github.com/Simsys/qhexedit2/blob/master/src/license.txt


## Description of QHexEdit 
(original  by Winfried Simon 2010)

QHexEdit is a hex editor widget written in C++ for the Qt (Qt4) framework. It is a simple editor for binary data, just like QPlainTextEdit is for text data. There are sip configuration files included, so it is easy to create bindings for PyQt and you can use this widget also in python.

QHexEdit takes the data of a QByteArray (setData()) and shows it. You can use the mouse or the keyboard to navigate inside the widget. If you hit the keys (0..9, a..f) you will change the data. Changed data is highlighted and can be accessed via data().

Normaly QHexEdit works in the overwrite Mode. You can set overwriteMode(false) and insert data. In this case the size of data() increases. It is also possible to delete bytes (del or backspace), here the size of data decreases.

You can select data with keyboard hits or mouse movements. The copy-key will copy the selected data into the clipboard. The cut-key copies also but delets it afterwards. In overwrite mode, the paste function overwrites the content of the (does not change the length) data. In insert mode, clipboard data will be inserted. The clipboard content is expected in ASCII Hex notation. Unknown characters will be ignored.

QHexEdit comes with undo/redo functionality. All changes can be undone, by pressing the undo-key (usually ctr-z). They can also be redone afterwards. The undo/redo framework is cleared, when setData() sets up a new content for the editor. You can search data inside the content with indexOf() and lastIndexOf(). The replace() function is to change located subdata. This 'replaced' data can also be undone by the undo/redo framework.

This widget can only handle small amounts of data. The size has to be below 10 megabytes, otherwise the scroll sliders ard not shown and you can't scroll any more.

## QHexEdit v0.6.3 porting to Qt 5.x
see tutorial of hwo to migrate from QT4 to QT5

https://qt-project.org/wiki/Transition_from_Qt_4.x_to_Qt5

1. QT+=qtwidget at the end of qhexedit.pro

2. header substitution:  from <QtGui.h> to <QtWidget.h>  by fixqt4headers.pl (It is not included into QT SDE and is downloaded from qt-project.org)

```
D:\vbshare\opensource_projects\qhexedit\example>"D:\Program Files (x86)\Git\bin\perl.exe" D:\Software\Qt5.1\5.1.0\mingw48_32\bin\fixqt4headers.pl --qtdir=D:\Software\Qt5.1\5.1.0\mingw48_32\

    Warning - cannot find QtQuick1 headers
    Done. Modified 2 of 7 file(s).

D:\vbshare\opensource_projects\qhexedit\example>cd ../src

D:\vbshare\opensource_projects\qhexedit\src>"D:\Program Files (x86)\Git\bin\perl.exe" D:\Software\Qt5.1\5.1.0\mingw48_32\bin\fixqt4headers.pl --qtdir=D:\Software\Qt5.1\5.1.0\mingw48_32\

    Warning - cannot find QtQuick1 headers
    Done. Modified 4 of 8 file(s).
```
3. Compiling error at line: 

```c int key = int(event->text()[0].toAscii()); ```

## QCharRef has no member toAscii()
The QCharRef class is a helper class for QString.

When you get an object of type QCharRef, if you can assign to it, the  assignment will apply to the character in the string from which you got the reference. That is its whole purpose in life. The QCharRef becomes invalid once modifications are made to the string: if you want to keep  the character, copy it into a QChar.   Most of the QChar member functions also exist in QCharRef. However,  they are not explicitly documented here.
       
```
//int key = int(event->text()[0].toAscii()); 
	// QT4.x QKeyEvent  text() return unicode QString
int key = event->key(); 
	// QT >4.x,key() does not distinguish capitalized or not capitalized key
        
QByteArray QString::toAscii() const  //    This function is deprecated.    

//Returns an 8-bit representation of the string as a QByteArray.
//This function does the same as toLatin1(). 
```
        
##pyQT porting
pyQT4 sip is included


## binary application release version

