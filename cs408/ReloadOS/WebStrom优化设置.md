# 1、WebStrom优化设置

## 1.0、WebStrom版本推荐

> [!NOTE]
>
> 推荐版本:WebStorm 2021.2.2

## 1.1、WebStrom终端设置

记得设置以管理员方式运行WebStrom，否则终端会不能支持某些命令。

1. 以管理员身份运行webstorm;
2. 执行：`get-ExecutionPolicy`，显示`Restricted`，表示状态是禁止的;
3. 执行：`set-ExecutionPolicy RemoteSigned`;
4. 这时再执行`get-ExecutionPolicy`，就显示RemoteSigned;