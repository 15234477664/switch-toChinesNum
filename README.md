# switch-toChinesNum
switch、数字转中文、合计功能

## switch
```html
<el-table-column
  prop="chargeItem"
  label="支付方式"
  :formatter="paymentChannelFormatter"
  align="left"
  show-overflow-tooltip>
</el-table-column>
```
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
