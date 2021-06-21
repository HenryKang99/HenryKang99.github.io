### 0verview

　　原本没打算进行记录，但是手贱使用了一下某网盘的同步盘功能，导致本地一些文件被覆盖，最终又得重新配置一遍。为了让以后悲剧再次发生时能够将损失减到最小，特此记录一下此次过程。
### 1. Hello Hexo

1. **安装`Node.js`，命令行`node -v` 验证；**

   - 安装时要选择自动写入环境变量，免得自己再配置；

2. **安装`Git`，命令行`git version`验证；**

   2.1  配置姓名与邮箱

   ```bash
   git config --global user.name "your name"
   git config --global user.email "email@example.com"
   ```

   2.2 生成密钥，会在用户目录下生成`,ssh`文件夹，里面包含公钥`id_rsa.pub`和私钥`id_rsa`，后面部署要用

   ```bash
   ssh-keygen -t rsa -C "email@example.com"
   ```

   

3. **安装`Hexo`**

   3.1 换淘宝源

   ```bash
   npm config set registry https://registry.npm.taobao.org
   ```

   3.2 安装cnpm，后面安装都通过cnpm命令

   ```bash
   npm install -g cnpm
   ```

   3.3 安装Hexo

   ```bash
   cnpm install -g hexo-cli
   ```

   3.4 说一下`-D,-S,-g`的区别

   - -D 依赖写入 devDependencies，用于开发环境；
   - -S 依赖写入 Dependencies，用于生产环境；
   - -g 全局安装到用户目录下，可以通过 `npm config set prefix`设置默认路径；
     - 不使用 -g 则安装到当前命令窗口所指的目录；

4. **初始化Hexo**

   4.1 选择一个空文件夹用于初始化 Hexo，例如在D盘下新建一个`_Blog`文件夹

   ```bash
   // 进入到该文件夹右键GitBash Here
   // 或者cd进入
   d:
   cd _Blog
   // 执行Hexo初始化命令
   hexo init
   ```

   4.2 安装`hexo-server`插件，这样就能在本地运行看效果了

   ```bash
   cnpm install hexo-server
   ```

   4.3 构建博客（生成静态页面）并在本地浏览

   ```bash
   hexo g
   hexo s
   ```

   - g 是 generate，s 是 server
   - 访问本地 4000 端口，看见默认主题的博客即成功；

   4.4 写博客并在本地浏览

   ```bash
   hexo new "博客名"
   hexo clean && hexo g && hexo s
   ```

   - hexo new 会在博客的`source/_posts/`下创建博客名文件，通过编辑器打开编写博客；
   - hexo clean 用于清除上一次生成的缓存；

   4.5 常用命令

   ```bash
   hexo clean 清理缓存
   hexo g	   构建博客
   hexo s	   本地服务器
   hexo d	   推送到远端，下面讲
   ```

   

----

### 2. 部署到GiteePages

1. **Gitee 设置**

   1.1 在Gitee密钥管理设置中添加公钥，将Git公钥`id_rsa.pub`中的内容复制进去；

   1.2 创建 blog 仓库，注意下**仓库的ssh地址**，下面用；

   1.3进入仓库选择服务->Pages->启用，注意下分配的**站点url**，下面用；

2. **配置博客目录下的站点配置文件`_config.yml`**

   2.1 配置站点url

   ```yml
   # URL
   ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
   url: Pages服务得到的站点url
   root: /仓库名
   ```

   2.2 配置部署地址，就是仓库的ssh地址

   ```yml
   deploy:
       type: git
       #repo: git@github.com:HenryKang99/HenryKang99.github.io.git       
       repo: git@gitee.com:henrykang99/blog.git
       branch: master                           
       message:
   ```

3. **推送到仓库**

   3.1 安装`hexo-deployer-git`插件用于Git推送

   ```bash
   cnpm install hexo-deployer-git --save
   ```

   3.2 运行`hexo -d`命令推送到仓库

   3.3 因为GiteePages普通版不支持自动部署，所以需要去Pages页面手动点击更新；

   - 可以免费体验一个月GiteePagesPro服务，自动部署且可以自定义域名和Https；

   3.4 等上两三分钟，访问你的站点url，成功；

   3.5 [点点看我的博客](https://www.henrykang.site)

----

### 3. 更换主题与个性化

1. 挑选主题

   网上自己找（关键字：Hexo主题top10），下载（解压）放在博客目录下的`themes`文件夹中，推荐选择用的人多的主题，碰到问题容易找到解决方法；

2. 站点配置文件`_config,yml`中配置

   ```yml
   # Extensions
   ## Plugins: https://hexo.io/plugins/
   ## Themes: https://hexo.io/themes/
   theme: 你的主题文件夹名称
   ```

3. 再hexo三连，进行本地浏览

   ```bash
   hexo clean && hexo g && hexo s
   ```

4. 遇到问题就查找本主题的文档（一般都有）或一些使用者写的博客，主题文件夹下也有一个`_config.yml`配置文件，用于对主题的一些个性化的配置；

