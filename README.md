# switch-toChinesNum
switch、合计功能、数字转中文

## switch
html
```html
<el-table-column
  prop="chargeItem"
  label="支付方式"
  :formatter="paymentChannelFormatter"
  align="left"
  show-overflow-tooltip>
</el-table-column>
```
js
```js
paymentChannelFormatter (row, column) {
  if (row.chargeItem) {
    switch (row.chargeItem) {
      case 1 :
        return '转账'
      case 2 :
        return '支付宝'
      case 3 :
        return '网银'
      case 4 :
        return '现金'
      case 5 :
        return '汇款'
      case 6 :
        return '其他'
    }
  } else {
    return '------'
  }
}
```
## element 表格自定义列合计
html
```html
<el-table
    :data="projecttableData"
    border
    show-summary
    :summary-method="getSummaries"
    header-cell-class-name="table_header">
    <el-table-column
      type="index"
      label="序号"
      width="50"
      align="left"
      :index="indexMethod">
    </el-table-column>
    <el-table-column
      prop="applyDate"
      label="申请时间"
      align="left"
      show-overflow-tooltip>
    </el-table-column>
    <el-table-column
      prop="creditCode"
      label="统一社会信用代码"
      align="left"
      show-overflow-tooltip>
    </el-table-column>
    <el-table-column
      prop="biaoDuanNumber"
      label="标段"
      align="left"
      :formatter="biaoDuanNumberFormatter"
      show-overflow-tooltip>
    </el-table-column>
    <el-table-column
      prop="phone"
      label="付款方联系电话"
      align="left"
      show-overflow-tooltip>
    </el-table-column>
    <el-table-column
      prop="invoicesContent"
      label="发票内容"
      align="left"
      show-overflow-tooltip>
    </el-table-column>
    <el-table-column
      prop="chargeItem"
      label="支付方式"
      :formatter="paymentChannelFormatter"
      align="left"
      show-overflow-tooltip>
    </el-table-column>
    <el-table-column
      prop="invoicesPrice"
      label="金额"
      align="left"
      show-overflow-tooltip>
    </el-table-column>
    <el-table-column
      prop="invoicesPrice"
      label="1次报价"
      :label="toChinesNum(1) + `次报价`"
      align="left"
      :formatter="invoicesPriceFormatter"
      show-overflow-tooltip>
    </el-table-column>
    <el-table-column
      label="操作" width="100" align="center" fixed="right">
      <template slot-scope="scope">
        <el-button type="text" size="small" class="handle_btn">
          详情
        </el-button>
        <el-button type="text" size="small" class="handle_btn">
          删除
        </el-button>
      </template>
    </el-table-column>
</el-table>
```
js
```js
    // 合计
    getSummaries (param) {
      const { columns, data } = param
      const sums = []
      let sumPrice = 0
      data.map((item) => {
        sumPrice += Number(item.invoicesPrice)
      })
      columns.forEach((column, index) => {
        if (index === 0) {
          sums[index] = '合计'
          return
        }
        if (index === 7) {
          sums[index] = sumPrice.toFixed(2)
        } else if (index === 8) {
          sums[index] = this.toChinesNum(sumPrice)
        } else {
          sums[index] = ''
        }
      })
      return sums
    },
```
## 数字转换中文
html
```html
<el-table-column
  prop="invoicesPrice"
  label="1次报价"
  :label="toChinesNum(1) + `次报价`"
  align="left"
  :formatter="invoicesPriceFormatter"
  show-overflow-tooltip>
</el-table-column>
```
js
```js
// 数字转化成文字
toChinesNum (num) {
  let changeNum = ['零', '一', '二', '三', '四', '五', '六', '七', '八', '九'] // changeNum[0] = "零"
  let unit = ['', '十', '百', '千', '万']
  num = parseInt(num)
  let getWan = (temp) => {
    let strArr = temp.toString().split('').reverse()
    let newNum = ''
    for (var i = 0; i < strArr.length; i++) {
      newNum = (i === 0 && strArr[i] === 0 ? '' : (i > 0 && strArr[i] === 0 && strArr[i - 1] === 0 ? '' : changeNum[strArr[i]] + (strArr[i] === 0 ? unit[0] : unit[i]))) + newNum
    }
    return newNum
  }
  let overWan = Math.floor(num / 10000)
  let noWan = num % 10000
  if (noWan.toString().length < 4) noWan = '0' + noWan
  return overWan ? getWan(overWan) + '万' + getWan(noWan) : getWan(num)
},
```
