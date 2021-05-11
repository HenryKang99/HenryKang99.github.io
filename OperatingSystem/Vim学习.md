[参考鸟哥的Linux私房菜]()

### 三种模式

1. 一般指令模式：使用 vim 命令打开文件后的默认模式，在这个模式中可以移动光标，复制、粘贴、删除数据。
2. 编辑模式：在一般指令模式下，按 i、o、a 等进入编辑模式，这时才可以对文件内容进行编辑，按 esc 退出到一般指令模式。
3. 指令列命令模式：在一般指令模式下，按 : 进入该模式，可以使用一些扩展的功能。

### 常用指令

#### 一般指令模式

##### 移动光标

![移动光标](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510095209315.png)

##### 复制、粘贴、删除

![复制、粘贴、删除](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510095633623.png)

##### 搜索与取代

![搜索与取代](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510095427939.png)

#### 编辑模式

![编辑模式](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510095755876.png)

#### 指令列模式

![指令列模式](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510095915211.png)



### 恢复功能

每次使用Vim编辑文件时，都会生成一个同名的.swap暂存文件，只有当本次编辑正常退出时，该文件才会被自动删除。

当多个用户同时编辑某个文件，或上次编辑未正常结束，意图使用Vim编辑该文件时，系统发现有该文件的暂存文件，就会提示警告信息：

- [O]pen Read-Only：以只读的方式打开。
- [E]dit anyway：以正常编辑的方式打开，不使用暂存文件。
- [R]ecover：载入暂存文件的内容，恢复后需要手动删除暂存文件，不然下次打开还会提示警告信息。
- [D]elete it：确定暂存文件没用，直接删掉。
- [Q]uit：退出。
- [A]bort：退出。

### 其他常用功能

#### 区块选择

![区块选择](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510150712676.png)

#### 多文件编辑

使用 vim file1 file2 ...同时编辑多个文件，但同时只会显示一个文件。

![多文件编辑](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510151022381.png)

#### 多窗口编辑

当不同文件或同一个文件的不同部分需要对照查看时，可以选择使用该功能。用法如下：

![多窗口编辑](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510151922341.png) 

#### 智能补全

1. `ctrx+x -> ctrx+n`：以当前文件出现过的内容作为关键字作为补全选项。
2. `ctrx+x -> ctrx+f`：以当前文件所在目录出现的文件名作为关键字作为补全选项。
3. `ctrx+x -> ctrx+o`：以当前文件的扩展名判断，使用 vim 内置的关键字作为补全选项。

### 配置文件

两个相关的文件：

 `~/.viminfo`：自动创建，会记录用户之前的操作，支撑的功能如再次编辑同一个文件时，光标还停留在上一次编辑的位置。

`~/.vimrc`：用于配置 vim 的环境，需要手动创建。下面是一些常用参数：

![~/.vimrc 常用配置参数](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20210510153328107.png)

