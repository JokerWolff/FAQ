# 1. iyuu 登录密码恢复

1.1 进入 `bash` (其他 `shell` 基本原理与此相同)，执行以下命令进入 `iyuu` 容器
```bash
sudo docker exec -it iyuu /bin/bash
```
1.2 在容器中执行以下命令
```bash
echo '<?php echo password_hash("自定义密码", PASSWORD_DEFAULT)."\n"; ?>' | php
```
> ps: 输出结果 `$2y$10xxxxx.xxxxxx.xxxx···` 为自定义密码的哈希

1.3 键入 `mysql -u iyuu -p` 进入数据库，提示输入密码，默认为 `iyuu`

1.4 依次执行以下指令:

```mysql
# 选中`iyuu`数据库
use iyuu;
# 查看iyuu安装时指定的管理员用户名
select username from wa_admins;
# 完成密码的修改
update wa_admins
set password='$2y$10xxxxx.xxxxxx.xxxx···'
where username='你的管理员用户名';
```
> 至此， `iyuu` 密码已更新为自定义的密码