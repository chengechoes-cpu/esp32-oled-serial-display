# ESP32 OLED 串口显示器（Arduino）

> 上位机通过串口发一行文本，ESP32 立刻显示到 0.96" OLED 上。
> 用来给电脑/树莓派/其他单片机做一个**即插即用的外挂小屏**。

---

## 硬件

| 部件 | 规格 |
|---|---|
| 开发板 | ESP32（Arduino 框架即可，任意 esp32 dev module） |
| 屏幕 | 0.96" OLED 128×64，I2C，SSD1306，地址 0x3C |

## 接线

| OLED | ESP32（默认 I2C） |
|---|---|
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO21 |
| SCL | GPIO22 |

## 协议

串口 **9600 8N1**，以换行 `\n` 为一帧：

```
温度=26.5 C\n        → 屏幕上以 2 倍字号显示 "温度=26.5 C"
```

开机自检时屏幕打印一行固定文字，用来验证接线和驱动是否正常。

## 依赖库（Arduino IDE 库管理器搜索安装）

- `Adafruit GFX Library`
- `Adafruit SSD1306`

## 使用

1. Arduino IDE 打开 `sketch4.10_mar26a.ino`
2. 板子选 ESP32 Dev Module，端口选实际 COM
3. 上传，打开串口监视器（9600），输入文字回车即可看到屏幕刷新

Python 侧发送示例：

```python
import serial
ser = serial.Serial("COM3", 9600)
ser.write("温度=26.5 C\n".encode("utf-8"))
```

## 已知限制

- 只处理单行文本，不做自动换行排版（超长会被截断）。
- SSD1306 内置字库不含中文；要显示中文需自备中文点阵字库并改写 `print/println` 路径。
- I2C 默认地址 0x3C；部分模块是 0x3D，接不上先改 `display.begin()` 的参数。

---

*嵌入式入门练手项目：串口通信 + I2C 显示的最小闭环。*
