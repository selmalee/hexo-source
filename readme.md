#常用命令 mac 要加 sudo
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub

hexo d -g #生成加部署
hexo s -g #预览加部署

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy

#更换主题
$ hexo clean
$ hexo g
$ hexo s

$ git clone https://github.com/iissnan/hexo-theme-next.git

theme: hexo-theme-next

$ cd themes/hexo-theme-next
$ git pull

#修改
hexo\themes\next\layout\_partials\header.swig line41
{{ __('menu.' + itemName) }}

fix FATAL Permission denied (publickey)
$ ssh-add ~/.ssh/id_rsa
$ ssh-add -l

#换电脑的话...
_config.yml，theme/，source/，scaffolds/，package.json，.gitignore，是需要拷贝的
1. 密钥弄好
2. npm install -g hexo
3. npm install
4. npm install hexo-deployer-git --save
(千万不要hexo init)
