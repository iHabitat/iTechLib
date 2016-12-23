# form data和request payload的区别
## 问题背景

> 今天在线上DomeOS环境中创建一个新部署时，通过Chrome Developer Tool查看请求提交的参数时，第一次看见了**Request Payload**这个选项，想弄明白具体什么含义。

![request payload](https://cloud.githubusercontent.com/assets/980216/20785313/843a2f06-b7da-11e6-98e2-8ad2ab54eeae.png)

## 解决方案

查阅相关资料后，二者的根本区别在于**请求中是否将请求头`Content-Type`的值设为`application/x-www-form-urlencoded`**，如果是，那么查询参数就会展现在**form data**区中，否则就会展现在**request payload**区中。具体的解释可以参阅[Stack Overflow](http://stackoverflow.com/questions/10494574/what-is-the-difference-between-form-data-and-request-payload)。
