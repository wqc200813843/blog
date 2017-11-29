# Linux 常用命令
1. **ls** [选项] [目录名] | 列出相关目录下的所有目录和文件
    - -a 列出包括.a开头的隐藏文件的所有文件
    - -A 通-a，但不列出"."和".."
    - -l 列出文件的详细信息
    - -c 根据ctime排序显示
    - -t 根据文件修改时间排序
    
      一般采用`ls`和`ls -al`
2. **mv** [选项] 源文件或目录 目录或多个源文件 | 移动或重命名文件
    - -b  覆盖前做备份
    - -f  如存在不询问而强制覆盖
    - -i  如存在则询问是否覆盖
    - -u  较新才覆盖
    - -t  将多个源文件移动到统一目录下，目录参数在前，文件参数在后

      一般采用`mv a /tmp/`把a移动到tmp目录下 和 `mv a b`把a重命名为b
3. **cp** [选项] 源文件或目录 目录或多个源文件 | 将源文件复制至目标文件，或将多个源文件复制至目标目录
    - -r -R 递归复制该目录及其子目录内容
    - -p  连同档案属性一起复制过去
    - -f  不询问而强制复制
    - -s  生成快捷方式
    - -a  将档案的所有特性都一起复制
4. **scp** [参数] [原路径] [目标路径] | 在Linux服务器之间复制文件和目录
    - 从本地复制到远程 `scp [-r] local_file/local_folder [remote_name]@remote_ip:remote_folder`

      `-r`代表复制文件夹     填写remote_name之后只需输入密码，否则还需要输入用户名
    - 从远程复制到本地 `scp [-r] [remote_name]@remote_ip:remote_folder local_file/local_folder`
5. **rm** [选项] 文件 | 删除文件
    - -r 删除文件夹
    - -f 删除不提示
    - -i 删除提示
    - -v 详细显示进行步骤
6. **touch** [选项] 文件 | 创建空文件或更新文件时间
    - -a 只修改存取时间
    - -m 值修改变动时间
    - -r eg:touch -r a b ,使b的时间和a相同
    - -t 指定特定的时间 eg:touch -t 201211142234.50 log.log
    - -t time [[CC]YY]MMDDhhmm[.SS],C:年前两位
7. **pwd** 查看当前所在路径
8. **cd** 改变当前目录
    - -：返回上层目录
    - ..：返回上层目录
    -   ：返回主目录
    - /：根目录
9. **mkdir** [选项] 目录… | 创建新目录
    - -p 递归创建目录，若父目录不存在则依次创建
    - -m 自定义创建目录的权限 eg:mkdir -m 777 hehe
    - -v 显示创建目录的详细信息
10. **rmdir** 删除空目录
    - -v 显示执行过程
    - -p 若自父母删除后父目录为空则一并删除
11. **rm** [选项] 文件… | 一个或多个文件或目录
    - -f 忽略不存在的文件，不给出提示
    - -i 交互式删除
    - -r 将列出的目录及其子目录递归删除
    - -v 列出详细信息
    
      常用`rm -rf`
12. **echo** 显示内容（不常用）
    - -n  输出后不换行
    - -e  遇到转义字符特殊处理 
13. **cat** [选项] [文件]..| 一次显示整个文件或从键盘创建一个文件或将几个文件合并成一个文件（**不常用**）
14. **tac**反向显示