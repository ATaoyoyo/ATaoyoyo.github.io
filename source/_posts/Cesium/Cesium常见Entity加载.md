---
title: Cesium常见Entity加载
date: 2024-08-22 15:15:51
tags: MYSQL
categories: MYSQL
cover: /img/article/Cesium/bg.jpg
top_img: /img/article/Cesium/bg.jpg
description: Cesium常见Entity加载
---

# Cesium 常见 Entity 加载

> 本文记录 Cesium 中常见的一些 Entity 的加载代码,方便巩固查询

## 初始化地图

```js
let viewer = new Cesium.Viewer("map", {
	// 配置信息
});
```

## 点 Point

```js
viewer.entities.add({
	position: new Cesium.Cartesian3.fromDegrees(109, 29),
	point: {
		pixelSize: 10.0,
		color: Cesium.Color.RED,
		show: visible,
		outlineColor: Cesium.Color.BLUE,
		outlineWidth: 2.0,
	},
});
```

## 线 Polyline

```js
viewer.entities.add({
	polyline: {
		positions: Cesium.Cartesian3.fromDegreesArray([
			106.0001, 29.0001, 106.0111, 29.0101, 106.0221, 29.0201, 106.0331,
			29.0301, 106.0441, 29.0401, 106.0551, 29.0501,
		]),
		width: 1.0,
		material: Cesium.Color.RED,
	},
});
```

## 三维线段 PolylineVolume

```js
viewer.entities.add({
	polylineVolume: {
		positions: Cesium.Cartesian3.fromDegreesArray([
			105.0, 32.0, 105.00182, 31.0, 106.10921, 31.0,
		]),
		shape: [
			new Cesium.Cartesian2(-10000, -10000),
			new Cesium.Cartesian2(10000, -10000),
			new Cesium.Cartesian2(10000, 10000),
			new Cesium.Cartesian2(-10000, 10000),
		],
		cornerType: Cesium.CornerType.ROUNDED,
		material: Cesium.Color.BLUE.withAlpha(0.5),
		outline: true,
		outlineColor: Cesium.Color.BLACK,
	},
});
```

## 面 Polygon

```js
viewer.entities.add({
	polygon: {
		hierarchy: Cesium.Cartesian3.fromDegreesArray([
			109, 29, 110, 29, 109.5, 28, 109.2, 28,
		]),
		material: Cesium.Color.BLUE,
		height: 100,
	},
});
```

## 文字 Label

```js
viewer.entities.add({
	position: Cesium.Cartesian3.fromDegrees(109, 29),
	label: {
		text: "Label Entity",
		font: "16px sans-serif",
		scale: 1.0,
		fillColor: Cesium.Color.RED,
		outlineColor: Cesium.Color.BLUE,
		outlineWidth: 2.0,
		style: Cesium.LabelStyle.FILL_AND_OUTLINE,
		showBackground: true,
		backgroundColor: Cesium.Color.BLACK.withAlpha(0.2),
		backgroundPadding: new Cesium.Cartesian2(10, 20),
		heightReference: Cesium.HeightReference.CLAMP_TO_GROUND,
	},
});
```

## 广告牌 Billboard

```js
viewer.entities.add({
	position: Cesium.Cartesian3.fromDegrees(109, 29),
  billboard: {
    image: image // 图片地址,
    scale: 1.0,
    width: 30,
    height: 26,
    heightReference: Cesium.HeightReference.CLAMP_TO_GROUND,
		verticalOrigin: Cesium.VerticalOrigin.BOTTOM,
  }
});
```

## 矩形 Rectangle

```js
viewer.entities.add({
	rectangle: {
		coordinates: Cesium.Rectangle.fromDegrees(90, 30, 100, 40),
		fill: true,
		materail: Cesium.Color.BLUE,
		height: 100,
		outline: true,
		outlineColor: Cesium.Color.RED,
	},
});
```

## 椭圆 Ellipse

```js
viewer.entities.add({
	position: new Cesium.Cartesian3.fromDegress(109, 30),
	ellipse: {
		// 长短半径一样为正圆
		semiMajorAxis: 10000,
		semiMinorAxis: 10000,
		material: Cesium.Color.RED,
		extrudedHeight: 1000, // 拉升高度
	},
});
```

## 盒子 Box

```js
viewer.entities.add({
  position: new Cesium.Cartesian3.fromDegress(109. 30),
  box: {
    dimensions: new Cesium.Cartesian3(10000, 20000, 3000),
    material: Cesium.Color.RED,
    fill: true
  }
})
```

## 墙体 Wall

```js
viewer.entities.add({
	wall: {
		positions: Cesium.Cartesian3.fromDegreesArrayHeights([
			100, 29, 10000, 108, 29, 10000, 108, 36, 10000, 100, 36, 10000, 100, 29,
			10000,
		]),
		material: Cesium.Color.RED,
	},
});
```

## 模型 Model

```js
viewer.entities.add({
	position: Cesium.Cartesian3.fromDegrees(106, 29),
	model: new Cesium.ModelGraphics({
		uri: "模型地址",
		scale: 6,
		minimumPixelSize: 128,
		maximumScale: 20000,
	}),
});
```
