## 主要依赖两个模块：html2canvas和jspdf：

> （1）npm install --save html2canvas（将页面html转换成图片）
> （2）npm install --save jspdf（将图片生成pdf）

## 定义HtmlToPdf插件

创建一个htmlToPdf.js文件在指定位置内容如下：

```js
// 导出页面为PDF格式

import html2canvas from 'html2canvas'

import JSPDF from 'jspdf'

export default {

    install(Vue, options) {
        Vue.prototype.ExportSavePdf = function (htmlTitle, currentTime) {
            var element = document.getElementById('pdfContent')
            html2canvas(element, {

                logging: false

            }).then(function (canvas) {
                var pdf = new JSPDF('p', 'mm', 'a4') // A4纸，纵向

                var ctx = canvas.getContext('2d')

                var a4w = 170;
                var a4h = 257 // A4大小，210mm x 297mm，四边各保留20mm的边距，显示区域170x257

                var imgHeight = Math.floor(a4h * canvas.width / a4w) // 按A4显示比例换算一页图像的像素高度

                var renderedHeight = 0

                while (renderedHeight < canvas.height) {
                    var page = document.createElement('canvas')

                    page.width = canvas.width

                    page.height = Math.min(imgHeight, canvas.height - renderedHeight) // 可能内容不足一页

                    // 用getImageData剪裁指定区域，并画到前面创建的canvas对象中
                    page.getContext('2d').putImageData(ctx.getImageData(0, renderedHeight, canvas.width, Math.min(imgHeight, canvas.height - renderedHeight)), 0, 0)
                    pdf.addImage(page.toDataURL('image/jpeg', 1.0), 'JPEG', 10, 10, a4w, Math.min(a4h, a4w * page.height / page.width)) // 添加图像到页面，保留10mm边距
                    renderedHeight += imgHeight

                    if (renderedHeight < canvas.height) {
                        pdf.addPage()
                    } // 如果后面还有内容，添加一个空页
                    // delete page;
                }
                pdf.save(htmlTitle + currentTime)
            })
        }
    }
}
```

## 安装插件

接下来我们需要把上面实现的插件安装到Vue中： 在main.js中安装插件

```js
import htmlToPdf from '@/components/utils/htmlToPdf'
Vue.use(htmlToPdf)
```

## 使用ExportSavePdf转换

定义好要转换的dom容器，然后直接调用ExportSavePdf函数，传入参数即可：

```vue
<template>
  <div>
    <div ref="pdfContent" id="pdfContent">
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column fixed prop="date" label="日期" width="150"></el-table-column>
        <el-table-column prop="name" label="姓名" width="120"></el-table-column>
        <el-table-column prop="province" label="省份" width="120"></el-table-column>
        <el-table-column prop="city" label="市区" width="120"></el-table-column>
        <el-table-column prop="address" label="地址" width="300"></el-table-column>
        <el-table-column prop="zip" label="邮编" width="120"></el-table-column>
        <el-table-column fixed="right" label="操作" width="100">
          <template slot-scope="scope">
            <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
            <el-button type="text" size="small">编辑</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <el-button type="danger" @click="ExportSavePdf()">导出PDF</el-button>
  </div>
</template>

<script>
import html2canvas from 'html2canvas'
import JSPDF from 'jspdf'
export default {
  methods: {
    handleClick(row) {
      console.log(row)
    },
  },

  data() {
    return {
      htmlTitle: 'PDF名称',
      nowTime: '',
      tableData: [
        {
          date: '2016-05-03',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
        },
        {
          date: '2016-05-02',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
        },
        {
          date: '2016-05-04',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
        },
        {
          date: '2016-05-01',
          name: '王小虎',
          province: '上海',
          city: '普陀区',
          address: '上海市普陀区金沙江路 1518 弄',
          zip: 200333,
        },
      ],
    }
  },
}
</script>

```
