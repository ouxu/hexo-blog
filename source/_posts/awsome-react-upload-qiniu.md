
---
title: 图片上传七牛并处理 ——  Antd Upload
date: 2017年11月01日22:07:50
tags:
  - React
  - JavaScript
  - Utils
categories: 工具推荐
comments: false
---

> Awsome React Libiary——介绍开发中实用的 React 库，都是自己已经在项目中用过的，介绍库的同时会附上最佳实践。

### 简介
`Antd` 开发和服务于企业级后台的 React UI 组件库。

`Antd Upload` 文件选择上传和拖拽上传控件。

`七牛对象存储` 实用的对象存储和 CDN 服务使我们网站静态资源更好的托管

+ [上传组件](https://ant.design/components/upload-cn/)
+ [Github](https://github.com/ant-design/ant-design)
+ [七牛SDK](https://developer.qiniu.com/sdk#official-sdk)

![](http://netdisk.qn.outxu.cn/17-11-2/44118404.jpg)

### 特性

+ Upload 控件能高度自定义，能够应对大部分的应用场景。
+ 七牛提供对象存储服务，文件直传七牛，减少服务器存储压力。
+ 七牛 cdn 加速图片等资源访问速度。
+ ...

### 评价
+ 大小：29k (antd-upload)
+ 优点：见特性
+ 缺点：
	+ 考虑安全性，需要服务器提供一次性上传 token
	+ 七牛免费额度有限，资源过多小心欠费

### 使用分析

首先根据七牛官方文档，配置好 Upload 控件上传所需参数或返回上传参数的方法，此处只需要添加一个自定义头部属性—— `token`，这个 `token` 是根据七牛的 `ACCESS_KEY、SECRET_KEY、Bucket` 生成的。正常使用时应该由后端提供这个 token，前端在 `Upload` 控件的 `BeforeUpload`周期中获取。不同语言有不同 `token` 的生成 SDK，七牛都有提供，可以参考[七牛SDK](https://developer.qiniu.com/sdk#official-sdk)配置。

这里献上 `Node.js` 的关键代码，这是最简单，复杂的参考七牛文档。

```javascript
import qiniu from 'qiniu'
qiniu.conf.ACCESS_KEY = ''
qiniu.conf.SECRET_KEY = ''
const bucket = ''

const getToken = () => {
  const putPolicy = new qiniu.rs.PutPolicy({
    scope: bucket
  })
  return putPolicy.uploadToken()
}

export default getToken
```

上传的前置条件配置好后，我们将关注点回到上传图片并获取`url`。通过控件选择文件后，文件开始上传，http 请求触发 `Upload` 控件的 `onChange` 方法，直到图片上传完成停止查询。我们格式化结果可以放在这一步进行，值得注意的是，不管请求的 `status` 是否为 `done`，都要更新 fileList。因为我们需要将 fileList 存储到组件的 `state` 中，这样文件的 `fileList` 属性才能同步。

说了这么多，没看懂的话看看 `demo` 吧。

### 代码

```jsx
import React, { Component } from 'react'
import { Icon, Modal, Upload } from 'antd'


const QINIU_SERVER = 'http://upload.qiniup.com' // 根据存储区域修改上传域名
const BASE_QINIU_URL = ''  // 空间 bucket 绑定的域名

class UploadDemo extends Component {
  constructor (props) {
    super(props)
    this.state = {
      previewVisible: false,
      previewImage: '',
      fileList: [{
        uid: -1,
        name: 'xxx.png',
        status: 'done',
        url: 'https://zos.alipayobjects.com/rmsportal/jkjgkEfvpUPVyRjUImniVslZfWPnJuuZ.png'
      }]
    }
  }

  handleCancel = () => this.setState({previewVisible: false})

  handlePreview = (file) => {
    this.setState({
      previewImage: file.url || file.thumbUrl,
      previewVisible: true
    })
  }

  handleChange = ({file, fileList}) => {
    const {uid, name, type, thumbUrl, status, response = {}} = file
    const fileItem = {
      uid,
      name,
      type,
      thumbUrl,
      status,
      url: BASE_QINIU_URL + (response.hash || '')
    }
    fileList.pop()
    fileList.push(fileItem)
    this.setState({fileList})
  }
  getUploadToken = () =>{
    const token = getToken() // 从服务器获取 token
    this.setState({token})
  }
  render () {
    const {previewVisible, previewImage, fileList} = this.state
    const uploadProps = {
      action: QINIU_SERVER,
      data: {
        token: this.state.token
      },
      listType: 'picture-card',
      className: 'upload-list-inline',
      fileList,
      beforeUpload: this.getUploadToken,
      onPreview: this.handlePreview,
      onChange: this.handleChange
    }
    return (
      <div>
        <Upload {...uploadProps}>
          <div>
            <Icon type='plus' />
            <div className='ant-upload-text'>Upload</div>
          </div>
        </Upload>
        <Modal visible={previewVisible} footer={null} onCancel={this.handleCancel}>
          <img style={{width: '100%'}} src={previewImage} />
        </Modal>
      </div>
    )
  }
}

export default UploadDemo
```

---
相关链接：

+ [Ant Design upload 组件快速配置使用七牛云](https://segmentfault.com/a/1190000011174923)
