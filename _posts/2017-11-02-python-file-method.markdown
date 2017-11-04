---
layout: post
author: 徐生梅
title: "Python 常用文件及目录操作方法记录"
catalog: true
tags: file 
---

- 获得当前Python脚本工作的目录路径：os.getcwd()。
- 返回指定目录下的所有文件和目录名：os.listdir()。例如返回C盘下的文件：os.listdir("C:\\")
- 删除一个文件：os.remove(filepath)。
- 删除多个空目录：os.removedirs(r"d:\python")。
---
- 检验给出的路径是否是一个文件：os.path.isfile(filepath)。
- 检验给出的路径是否是一个目录：os.path.isdir(filepath)。
- 判断是否是绝对路径：os.path.isabs()。
- 检验路径是否真的存在：os.path.exists()。例如检测D盘下是否有Python文件夹：os.path.exists(r"d:\python")
---
- 分离一个路径的目录名和文件名：os.path.split()。例如：os.path.split(r"/home/qiye/qiye.txt")，返回结果是一个元组：('/home/qiye', 'qiye.txt')。
- 分离扩展名：os.path.splitext()。例如os.path.splitext(r"/home/qiye/qiye.txt")，返回结果是一个元组：('/home/qiye/qiye', '.txt')。
---
- 获取路径名：os.path.dirname(filetpah)。
- 获取文件名：os.path.basename(filepath)。
- 读取和设置环境变量：os.getenv()与os.putenv()。
- 给出当前平台使用的行终止符：os.linesep。Windows使用’\r\n', Linux使用’\n’而Mac使用’\r'。
- 指示你正在使用的平台：os.name。对于Windows，它是’nt'，而对于Linux/Unix用户，它是’posix'。
- 重命名文件或者目录：os.rename(old, new)。
---
- 创建多级目录：os.makedirs(r"c:\python\test")。
- 创建单个目录：os.mkdir("test")。
- 获取文件属性：os.stat(file)。
- 修改文件权限与时间戳：os.chmod(file)。
- 获取文件大小：os.path.getsize(filename)。
---
- 复制文件夹：shutil.copytree("olddir", "newdir")。olddir和newdir都只能是目录，且newdir必须不存在。
- 复制文件：shutil.copyfile("oldfile", "newfile"), oldfile和newfile都只能是文件；shutil. copy("oldfile", "newfile"), oldfile只能是文件，newfile可以是文件，也可以是目标目录。
- 移动文件（目录）:shutil.move("oldpos", "newpos")。
- 删除目录：os.rmdir("dir")，只能删除空目录；shutil.rmtree("dir")，空目录、有内容的目录都可以删。