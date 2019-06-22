
# Rest URL

## URL例

| CRUD | 方式   | 非rest      | rest例1  | rest例2   |
| ---- | ------ | ----------- | -------- | --------- |
| 增   | POST   |             | /order   | /products |
| 删   | DELETE | delete?id=1 | /order/1 |           |
| 改   | PUT    | update?id=1 | /order/1 |           |
| 查   | GET    | get?id=1    | /order/1 | /products |


consumes: 指定处理请求的提交内容类型(Content-Type), 例如application/json, text/html;
produces: 指定返回的内容类型, 仅当request请求头中的(Accept)类型中包含该指定类型才返回;


[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
