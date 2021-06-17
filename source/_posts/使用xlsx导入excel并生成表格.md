---
title: 使用xlsx导入excel并生成表格
date: 2021-06-16 15:42:15
tags:
cover: /img/article/IMG_20210616.PNG
top_img: /img/article/IMG_20210616.PNG
description: 使用xlsx对Excel文件进行导入，并且生成表格。
---

# 使用 xlsx 导入 excel 并生成表格

最近项目上有一个新需求，要求前端导入 excel 文件，并且在页面上生成表格。刚好还没接触过这方面的东西，在一顿噼里啪啦的搜索查询资料之后，发现也不是特别难，折腾了一下午左右给弄出来了。为了后续的开发，特此记录一下整个过程。

## 开发环境

目前项目是基于`Vue`开发，使用`Vue-cli`脚手架生成项目，`UI`框架使用的`Element-ui`。导入 excel 文件之后，需要将文件中的单元格数据取出，并且做格式化的处理，使其满足`Element-ui`表格组件的格式。

## 文件上传

文件上传这个环节，可以直接使用[`el-upload`](https://element.eleme.cn/#/zh-CN/component/upload)这个组件。**需要注意的是，要对上传文件进行一个限制，可以加入`accept=".xls,.xlsx"`，让每次点击上传，都默认只展示 excel 文件。**

`el-upload`有许多文件上传，改变时的钩子，可以通过这些钩子做相应的逻辑，获取对应的文件信息。我在此使用到了`on-change`，文件状态变化时改变，返回两个参数，第一个是单个上传的文件，第二个则是全部上传的文件。

```html
<el-upload
  action=""
  accept=".xls,.xlsx"
  :auto-upload="false"
  :show-file-list="false"
  :limit="1"
  :on-change="handChangeFile"
>
  <el-button type="primary">Upload File</el-button>
</el-upload>
```

```js
handChangeFile(file, fileList) {
  console.log(file, fileList)
}
```

在这里取第一个参数使用就可以了，没有做多选。将`file`打印之后，可以看到上传文件的详细信息。为了防止用户上传的文件不是 excel，可以做一个简单的校验。

```js
handChangeFile(file) {
  const fileName = file.name

  if(!/^.*\.(?:xls|xlsx)$/i.test(fileName)) {
    this.$message.error('文件格式不正确，请上传后缀为xls或者xlsx格式的excel文件！')
  }
}
```

这个正则验证比较简单，仅支持了较新的 excel 格式，如果需要支持较老的 excel，可以自行查阅正则规则。

## 读取 excel 内容

拿到上传的 excel 文件之后，需要对文件内容进行读取。一番查阅之后，发现有一个`Web`接口：`FileReader`。

以下是[MDN](https://developer.mozilla.org/zh-CN/)对`FileReader`的解释：

> FileReader 对象允许 Web 应用程序异步读取存储在用户计算机上的文件（或原始数据缓冲区）的内容，使用 File 或 Blob 对象指定要读取的文件或数据。

`FileReader`是一个构造函数，并且异步读取，那么在后续操作中，可以加入一个回调函数对其进行操作。

### 事件处理

`FileReader`有多个事件处理方法，最主要的，并且可以使用的为以下三种：

- `FileReader.onabort` 该事件在读取操作被中断时触发。
- `FileReader.onload` 该事件在读取操作发生错误时触发。
- `FileReader.onerror` 该事件在读取操作完成时触发。

在这里，选取`onload`事件，新建一个函数，对文件做处理。

```js
handChangeFile(file) {
  const fileName = file.name

  if(!/^.*\.(?:xls|xlsx)$/i.test(fileName)) {
    this.$message.error('文件格式不正确，请上传后缀为xls或者xlsx格式的excel文件！')
  } else {
    // raw 为文件的原始信息，而不是file本身
    this.readFile(file.raw)
  }
},

readFile(file) {
  const fileReader = new FileReader()

  fileReader.onload = function(event) {
    // result 为文件内的内容
    const result = event.target.result

  }
}
```

### 方法

在对文件内容做了事件处理之后，可以通过`FileReader`自带的方法对数据进行保存操作。

`FileReader`有以下方法：

- `FileReader.abort()` 中止读取操作。
- `FileReader.readAsArrayBuffer()` 保存的将是被读取文件的 ArrayBuffer 数据对象.
- `FileReader.readAsBinaryString()` 保存所读取文件的原始二进制数据。 **不推荐**
- `FileReader.readAsDataURL()` 保存 Base64 字符串。
- `FileReader.readAsText()` 保存字符串。

我选取了`readAsArrayBuffer`这个方法，保存`Buffer`数据对象。

```js
readFile(file) {
  const fileReader = new FileReader()

  fileReader.onload = function(event) {
    // result 为文件内的内容
    const result = event.target.result

    console.log(result)
  }

  // fileReader.readAsBinaryString(file) 打印出来为看不懂的乱码字符串
  fileReader.readAsArrayBuffer(file) // 打印出来为ArrayBuffer数据类型
}
```

### 结合 xlsx 操作 excel 文件数据

首先在项目中安装`xlsx`：

```
yarn add xlsx
```

安装好之后，在项目中导入：

```js
import XLSX from 'xlsx'
```

在之前的`readFile`方法中加一个回调，并修改`handChangeFile`函数；新增`dealFile`函数，用来处理数据。

```js
handChangeFile(file) {
  const fileName = file.name

  if(!/^.*\.(?:xls|xlsx)$/i.test(fileName)) {
    this.$message.error('文件格式不正确，请上传后缀为xls或者xlsx格式的excel文件！')
  } else {
    // raw 为文件的原始信息，而不是file本身
    this.readFile(file.raw, this.dealFile)
  }
},

readFile(file, cb) {
  const fileReader = new FileReader()

  fileReader.onload = function(event) {
    // result 为文件内的内容
    const result = event.target.result

    cb(result)
  }

  // fileReader.readAsBinaryString(file) 打印出来为看不懂的乱码字符串
  fileReader.readAsArrayBuffer(file) // 打印出来为ArrayBuffer数据类型
},

// 新增
dealFile(file) {
  console.log(file)
}
```

在`dealFile`函数中，需要使用到`XLSX`的`read`方法，读取文件中的数据，并将其转换，得到的是一个对象，里面的`SheetName`与`Sheets`分别对应 excel 中的工作簿名称以及数据。

然后使用`XLSX.utils.sheet_to_json`将工作簿中的表数据转换成数组格式，在对其做一些调整，得到`element-ui`表格所需要的数据格式，就大功告成了！

```js
dealFile(file) {
  const wb = xlsx.read(file, { type: 'buffer' }) // type 为保存的数据类型，因为之前FileReader使用的保存为Buffer格式，所以这里也必须填buffer。 其他的有base64， binary等。

  const sheet = xlsx.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]], {
    // header: 1, // 表头所占行数
    defval: '', // 使用空字符串占位，解决为null的数据不展示
  })

  console.log(wb)
  console.log(sheet)

  const title = Object.keys(sheet[0])

  const titles = title.map((item, index) => {
    return { label: item, prop: item }
  })

  const data = sheet.splice(1, sheet.length)

  this.titles = titles // 表头
  this.data = data // 表格 data
},
```

得到对应的表格标题及表格数据之后，将其赋值之后，就可以看到上传的 excel 文件数据被生成表格了。

但是目前还存在一些缺陷，对于多表头的 excel 文件没有进行处理；并且表格中若有多张表，也没有进行处理。

后续慢慢改进吧......

## 总结

- 使用`el-upload`获取文件数据
- 利用`FileReader`构造函数读取并保存文件内容
- 用`xlsx.read`方法将文件内容转化，再使用`xlsx.utils.sheet_to_json`方法转换数据格式
- 将得到的数据格式化为`el-table`所需要的格式
