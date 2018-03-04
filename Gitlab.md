Centos7下GitLab CI持续集成配置方案-.NET
本问主要记录如何在Linux系统为公司配置源代码管理系统Gitlab，并使用GitlabRunner进行持续集成。

<1> 安装GitLab
参照https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/，使用清华镜像安装
新建 /etc/yum.repos.d/gitlab-ce.repo，内容为
[gitlab-ce]
name=Gitlab CE Repository
baseurl=https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el$releasever/
gpgcheck=0
enabled=1
再执行
sudo yum makecache
sudo yum install gitlab-ce

<2> 配置GitLab 
GitLab安装后默认只允许本地访问，需要配置外部主机
修改/etc/gitlab/gitlab.rb文件
执行如下命令，使用gedit打开/etc/gitlab/gitlab.rb文件
sudo mkdir -p /etc/gitlab
sudo touch /etc/gitlab/gitlab.rb
sudo chmod 600 /etc/gitlab/gitlab.rb
sudo gedit /etc/gitlab/gitlab.rb

配置外部URL
找到external_url，修改external_url的值，可以为Ip:port或domain name
## GitLab URL
##! URL on which GitLab will be reachable.
##! For more details on configuring external_url see:
##! https://docs.gitlab.com/omnibus/settings/configuration.html#configuring-the-external-url-for-gitlab
external_url 'http://192.168.1.110:9999'

配置数据文件
### For setting up different data storing directory
###! Docs: https://docs.gitlab.com/omnibus/settings/configuration.html#storing-git-data-in-an-alternative-directory
###! **If you want to use a single non-default directory to store git data use a
###!   path that doesn't contain symlinks.**
git_data_dirs({
  "default" => {
    "path" => "/mnt/data/git-data"
   }
})


<3> 安装GitLabRunner宿主机
GitLabRunner宿主机可以时windows也可以是Linux系统，对于基于.Net Framework的程序，需要Windows系统，对于.Net Core和其他语言的程序，可使用Linux系统。
系统语言最好选择英语，否则会出现错误信息为乱码造成无法识别的问题。

<4> 宿主机安装Git
在宿主机上安装Git
下载Nuget到硬盘，最好放在根目录下，如D:\Nuget.exe

<5> 宿主机准备GitLabRunner
下载gitlab-ci-multi-runner-windows-amd64.exe，并改名为gitlab-runner.exe将文件移动到D:\GitLab-Runner\gitlab-runner.exe

<6> 注册Git Lab Runner
请参照https://gitlab.com/gitlab-org/gitlab-runner/blob/master/docs/register/index.md
注册完会生成config.toml文件，内容大概如下：

在runners下添加 shell = "powershell"


<7> 安装Git Lab Runner
gitlab-runner install
gitlab-runner start

<9> 安装msbuild
从微软下载vs_buildtools__1782844887.1516325221，选择安装编译工具。

注意：不要安装到默认路径，路径中不要有空格。


<9> 测试为项目配置ci



点击commit后，Gitlabrunner会自动get所有的项目代码，并进行编译。
在项目的ci/cd下的pipeline可以看到一条记录

