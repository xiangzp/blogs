---
title: Arc 计算通用手册
author: Xiang
date: 2020-01-15 11:45:29
tags: [Geo,算法]
categories: 前端
featured_image: 'https://cdn.pixabay.com/photo/2018/01/31/05/43/web-3120321_1280.png'
intro: 整理一些常用的Arc计算方法，在进行地图应用开发的时候应该用得上。
---
#### 整理一些常用的Arc计算方法，在进行地图应用开发的时候应该用得上。

持续更新...
#### 常数值

```javascript

/**
 * 长半径
 */
const longRadius = 6378137
/**
 * 短半径
 */
const shortRadius = 6356752.3142
/**
 * 扁率
 */
const f = 1 / 298.257223563
```

#### 度和弧度转化

```javascript
/**
 * 度换算成弧度
 */
const rad = (d: number) => {
  return (d * Math.PI) / 180.0
}

/**
 * 弧度换算成度
 */
const deg = (x: number) => {
  return (x * 180.0) / Math.PI
}


```

#### 计算两点之间的距离
```javascript

 * 计算两点之间的距离
 */
export const calculateDistanceOf2Point = (
  coord1: LocPosition,
  coord2: LocPosition
) => {
  const lat1 = coord1.lat
  const lat2 = coord2.lat
  const lng1 = coord1.lng
  const lng2 = coord2.lng
  const radLat1 = rad(lat1)
  const radLat2 = rad(lat2)
  const radLng1 = rad(lng1)
  const radLng2 = rad(lng2)
  var a = radLat1 - radLat2
  var b = radLng1 - radLng2
  var s =
    2 *
    Math.asin(
      Math.sqrt(
        Math.pow(Math.sin(a / 2), 2) +
        Math.cos(radLat1) * Math.cos(radLat2) * Math.pow(Math.sin(b / 2), 2)
      )
    )
  s = s * longRadius
  return s
}
```

#### 根据经纬度，方位角(°)，距离(m) 计算下一个点的位置

```javascript
/**
 * 根据经纬度，方位角(°)，距离(m) 计算下一个点的位置
 */
export const calculateNewCoord = (
  latLng: LocPosition,
  angle: number,
  distance: number
): LocPosition => {
  const lon1 = latLng.lng
  const lat1 = latLng.lat

  const alpha1 = rad(angle)
  const sinAlpha1 = Math.sin(alpha1)
  const cosAlpha1 = Math.cos(alpha1)

  const tanU1 = (1 - f) * Math.tan(rad(lat1))
  const cosU1 = 1 / Math.sqrt(1 + tanU1 * tanU1)
  const sinU1 = tanU1 * cosU1
  const sigma1 = Math.atan2(tanU1, cosAlpha1)
  const sinAlpha = cosU1 * sinAlpha1
  const cosSqAlpha = 1 - sinAlpha * sinAlpha
  const uSq =
    (cosSqAlpha * (longRadius * longRadius - shortRadius * shortRadius)) /
    (shortRadius * shortRadius)
  const A = 1 + (uSq / 16384) * (4096 + uSq * (-768 + uSq * (320 - 175 * uSq)))
  const B = (uSq / 1024) * (256 + uSq * (-128 + uSq * (74 - 47 * uSq)))

  let sigma = distance / (shortRadius * A)
  let sigmaP = 2 * Math.PI
  let sinSigma: number = 0
  let cosSigma: number = 0
  let deltaSigma: number = 0
  let cos2SigmaM: number = 0
  while (Math.abs(sigma - sigmaP) > 1e-12) {
    cos2SigmaM = Math.cos(2 * sigma1 + sigma)
    sinSigma = Math.sin(sigma)
    cosSigma = Math.cos(sigma)
    deltaSigma =
      B *
      sinSigma *
      (cos2SigmaM +
        (B / 4) *
        (cosSigma * (-1 + 2 * cos2SigmaM * cos2SigmaM) -
          (B / 6) *
          cos2SigmaM *
          (-3 + 4 * sinSigma * sinSigma) *
          (-3 + 4 * cos2SigmaM * cos2SigmaM)))
    sigmaP = sigma
    sigma = distance / (shortRadius * A) + deltaSigma
  }

  var tmp = sinU1 * sinSigma - cosU1 * cosSigma * cosAlpha1
  var lat2 = Math.atan2(
    sinU1 * cosSigma + cosU1 * sinSigma * cosAlpha1,
    (1 - f) * Math.sqrt(sinAlpha * sinAlpha + tmp * tmp)
  )
  var lambda = Math.atan2(
    sinSigma * sinAlpha1,
    cosU1 * cosSigma - sinU1 * sinSigma * cosAlpha1
  )
  var C = (f / 16) * cosSqAlpha * (4 + f * (4 - 3 * cosSqAlpha))
  var L =
    lambda -
    (1 - C) *
    f *
    sinAlpha *
    (sigma +
      C *
      sinSigma *
      (cos2SigmaM + C * cosSigma * (-1 + 2 * cos2SigmaM * cos2SigmaM)))

  return { lat: deg(lat2), lng: lon1 + deg(L) }
}

```

#### 计算两个经纬度点之间的方位夹角

```javascript
/**
 * 计算两个点之间的方位夹角
 */
export const getBearingBetween2Points = (p1: LocPosition, p2: LocPosition) => {
  let lat1 = p1.lat
  let lat2 = p2.lat
  let lng1 = p1.lng
  let lng2 = p2.lng
  let dLon = rad(lng2 - lng1)
  let y = Math.sin(dLon) * Math.cos(rad(lat2))
  let x =
    Math.cos(rad(lat1)) * Math.sin(rad(lat2)) -
    Math.sin(rad(lat1)) * Math.cos(rad(lat2)) * Math.cos(dLon)
  let brng = deg(Math.atan2(y, x))
  return (brng + 360) % 360
}

```