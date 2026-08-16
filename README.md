# Finger Frame Pinch Switch

这个仓库是基于 Sophia Yang 的原项目
[`sophiamyang/finger-frame-effect`](https://github.com/sophiamyang/finger-frame-effect)
修改而来的版本。

原项目在线演示：
https://sophiamyang.github.io/finger-frame-effect/

本版本保留了原项目的 MediaPipe 手部识别和 Canvas 实时滤镜效果，并新增了一个手势交互：
双手拇指和食指闭合，再重新张开成取景框手势时，会自动切换到下一个滤镜。

## 效果

举起双手，用食指和拇指围出一个取景框。页面会识别两只手的位置，并只在手指围出的四边形区域里应用实时视觉效果。

可用滤镜包括：

1. **Pixelate**：像素化马赛克
2. **Blur**：模糊玻璃效果
3. **Invert**：颜色反相
4. **Noir**：高对比黑白
5. **Glitch**：故障风、色差、扫描线
6. **Toon**：卡通描边效果
7. **Van Gogh**：实时梵高风格笔触效果

切换方式：

- 双手同时做“闭合 -> 张开”的动作，切换到下一个滤镜
- 点击底部工具栏
- 按键盘数字键 `1` 到 `7`

## 本地运行

这个项目没有构建步骤，直接用任意静态文件服务器打开即可。

```bash
python3 -m http.server 8123
```

然后在浏览器里打开 Python 输出的本地预览地址，并允许摄像头权限。

摄像头访问需要安全来源，例如 HTTPS 或本机回环地址。

## Demo 模式

如果不想打开摄像头，可以在本地预览地址后面加上 `?demo`。

Demo 模式会用合成动画画面和假的手部关键点替代摄像头输入。假手也会循环做闭合再张开的动作，所以可以不用摄像头测试滤镜切换。

## 实现方式

- 使用 [MediaPipe Hand Landmarker](https://ai.google.dev/edge/mediapipe/solutions/vision/hand_landmarker) 在浏览器中识别双手关键点
- 使用 Canvas 2D 绘制镜像摄像头画面
- 根据两只手的食指尖和拇指尖计算取景框四个角点
- 对角点做平滑处理，减少手部抖动带来的画面跳动
- 将当前滤镜裁剪到手指围出的四边形区域内
- 通过“闭合 -> 张开”的状态机判断滤镜切换手势，避免连续误触发

## 致谢

原始创意和主要实现来自
[`sophiamyang/finger-frame-effect`](https://github.com/sophiamyang/finger-frame-effect)。

这个仓库是在原项目基础上做的小改版，重点是加入更适合现场互动的手势切换滤镜功能。
