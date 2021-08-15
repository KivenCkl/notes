# pyqt5

## 适配高分辨率显示

如果问题出在与分辨率不同的dpi上，则可以通过在创建 QApplication（或任何其他Qt类）之前向应用程序添加以下行来告诉 Qt 使用高 dpi 缩放比例/像素图，参考 [PyQt - Adjusting for Different Screen Resolution [duplicate]
](https://stackoverflow.com/questions/43904594/pyqt-adjusting-for-different-screen-resolution)

```python
# Handle high resolution displays:
if hasattr(QtCore.Qt, 'AA_EnableHighDpiScaling'):
    QtWidgets.QApplication.setAttribute(QtCore.Qt.AA_EnableHighDpiScaling, True)
if hasattr(QtCore.Qt, 'AA_UseHighDpiPixmaps'):
    QtWidgets.QApplication.setAttribute(QtCore.Qt.AA_UseHighDpiPixmaps, True)
```