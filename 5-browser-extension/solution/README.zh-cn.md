# Carbon Trigger 浏览器扩展：完整代码

使用 tmrow 的 C02 Signal API 跟踪用电量，构建浏览器扩展，以便您可以在浏览器中提醒您所在地区的用电量有多大。临时使用此扩展将帮助您根据此信息对您的活动做出判断。

![extension screenshot](../extension-screenshot.png)

## 入门

您需要安装 [npm](https://npmjs.com)。将此代码的副本下载到您计算机上的文件夹中。

安装所有必需的软件包：

```
npm install
```

从 webpack 构建扩展

```
npm run build
```

要在 Edge 上安装，请使用浏览器右上角的“三点”菜单找到“扩展”面板。从那里，选择“加载已解压”以加载新的扩展。在提示符下打开“dist”文件夹，扩展程序将加载。要使用它，您需要 CO2 Signal 的 API 的 API 密钥（([通过电子邮件在此处获取一个](https://www.co2signal.com/)- 在本页的框中输入您的电子邮件）以及与[电力地图](http://api.electricitymap.org/v3/zones)对应的您所在[地区的代码](https://www.electricitymap.org/map)（例如，在波士顿，我使用“US-NEISO”）。

![installing](../install-on-edge.png)

将 API 密钥和区域输入扩展界面后，浏览器扩展栏中的彩色点应该发生变化，以反映您所在区域的能源使用情况，并为您提供适合您执行哪些高耗能活动的指示。这个“点”系统背后的概念是由加州排放的 [Energy Lollipop extension](https://energylollipop.com/) 扩展给我的。
