laravel-blog 用户表调用接口信息
====

> 目前博客用户角色分为管理员和普通用户，只有管理员才可以调用以下接口

<br>
权限校验
----
> 非管理员调用返回

```bash
{
    "code":"500",  
    "msg":"索厘！您没有这个权限呢！"     
}
  ```
> 未登录用户调用直接返回登录页面

<br>
新建用户信息接口
----
|项目     | 内容                  |
|--------|:--------------------:|
|地址     | {hostname:port}/user |
|请求方式 |post                  |
|校验规则 |name => 必须填写，最长为255字符<br> email => 必须填写，最长为255字符，满足邮件格式，且在数据库中唯一 <br> password => 必须填写，最短为8个字符，最长为60字符 <br> role_id =>  必须填写，int类型 |   

> 校验失败返回

```bash
{
    "code":"500",  
    "data":
            {
              "name":"The name field is required.",
              "email":"The email field is required.",
              "password":"The password field isrequired.",
              "role_id":"The role id field is required."
            }
}
  ```      

> 调用成功后跳转到index方法

<br>
修改用户信息接口
----
|项目     | 内容                        |
|--------|:---------------------------:|
|地址     | {hostname:port}/user/id    |
|请求方式 |put                         |
|校验规则 |name => 必须填写，最长为255字符 <br> password => 必须填写，最短为8个字符，最长为60字符 <br> role_id =>  必须填写，int类型 |

> 找不到修改用户返回

```bash
{
    "code":"400",  
    "msg":"亲，查找不到对应的用户信息，请检查后重试哦！"  
}
```  

> 调用成功返回

```bash
{
    "code":"200",  
    "msg":"yep！"  
}
```   

<br>
删除用户信息接口
----
|项目     | 内容                        |
|--------|:---------------------------:|
|地址     | {hostname:port}/user/id    |
|请求方式 |delete                      |

> 找不到修改用户返回

```bash
{
    "code":"400",  
    "msg":"亲，查找不到对应的用户信息，请检查后重试哦！"  
}
```  
> 调用成功返回

```bash
{
    "code":"200",  
    "msg":"删除成功了！"  
}
```

<br>
获取指定用户信息接口
----
|项目     | 内容                        |
|--------|:---------------------------:|
|地址     | {hostname:port}/user/id    |
|请求方式 |get                         |

> 找不到修改用户返回

```bash
{
    "code":"400",  
    "msg":"亲，查找不到对应的用户信息，请检查后重试哦！"  
}
```  
> 调用成功放回

```bash
{
    "code":"200",  
    "data":
            {
              "name":"荣哥",
              "email":"email@email.com",
              "role_id":1
            }
}
  ```  

<br>
获取用户信息列表接口
----
|项目     | 内容                        |
|--------|:---------------------------:|
|地址     | {hostname:port}/user       |
|请求方式 |get                         |

> 调用成功放回到user下面的index文件，获取信息方式如下

```bash
foreach ($user as $value) {
  $value->name;
  $value->email;
  $value->role_id;
}
  ```
