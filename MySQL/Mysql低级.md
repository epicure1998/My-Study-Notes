## 用户创建以及权限赋予
1. 用root登录，
```
create user 'test'@'localhost' identified by '密码';
```
2. 一定要记得先`flush privileges;`
3. 进行权限赋予：
```
grant all privileges on *.* to 'test'@'localhost' with grant option;
```
4.再次刷新权限：`flush privileges;`
***
