/*文本超出长度用。。。*/
.textOverflow {
  display: block;/**内联对象需加**/
  width: 70px;
  word-break: keep-all;;/* 不换行 */
  white-space: nowrap;;/* 不换行 */
  overflow: hidden;/* 内容超出宽度时隐藏超出部分的内容 */
  text-overflow: ellipsis;/* 当对象内文本溢出时显示省略标记(...) ；需与overflow:hidden;一起使用。*/
}

对于表格文字溢出的定义：
对于表格超出范围显示省略号

table{
    width:30em;
    table-layout:fixed;/* 只有定义了表格的布局算法为fixed，下面td的定义才能起作用。 */
}
td{
    width:100%;
    word-break:keep-all;/* 不换行 */
    white-space:nowrap;/* 不换行 */
    overflow:hidden;/* 内容超出宽度时隐藏超出部分的内容 */
    text-overflow:ellipsis;/* 当对象内文本溢出时显示省略标记(...) ；需与overflow:hidden;一起使用。*/
}