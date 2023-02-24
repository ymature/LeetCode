## 如何通过git将本地文件push到github仓库

1. 首先在要push的文件夹下初始化git

```bash
git init
```

2. 将修改过的文件添加到git

```bash
git add .
或
git add 修改过的文件路径
```

3. 将文件提交，同时添加提交信息

```bash
git commit -m "自定义信息"
```

4. 切换到相应的分支

```bash
git branch -M main
或
git branch -M 分支名称
```

5. git关联远程github仓库（第一次关联之后就无需重复关联）

```bash
git remote add origin 仓库网址
```

6. 将git中的内容push到github仓库

```bash
git push -u origin main
```

