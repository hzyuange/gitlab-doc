简介：<br>
在Git项目仓库代码管理、团队协作非常好用，GitLab当然是不二之选，自己搭建服务器完全免费的喲。<br>
环境：<br>
注：仅指在CentOS7.4的64位版本上，进行Gitlab的相关安装）。

### Gitlab的安装
1、按顺序执行以下命令：
```
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
sudo yum install -y postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```

2、下载rpm安装包
```
cd /usr/local/src
wget http://yunwei.diycode.me/gitlab-ce-8.15.1-ce.0.el7.x86_64.rpm
rpm -ivh gitlab-ce-8.15.1-ce.0.el7.x86_64.rpm
```

3、执行配置命令(可重复执行)
```
gitlab-ctl reconfigure
```

4、修改配置文件
```
#一定要改配置
chown -R git:git /var/opt/gitlab/git-data/repositories
vim /etc/gitlab/gitlab.rb
    external_url 'http://git.diycode.me'
    nginx['listen_addresses'] = ['git.diycode.me']
    nginx['listen_port'] = 8888
vim /var/opt/gitlab/nginx/conf/gitlab-http.conf
chown -R git:git /opt/gitlab
gitlab-ctl reconfigure

#检测安装情况
sudo gitlab-rake gitlab:check SANITIZE=true

#不用改的配置
vim /opt/gitlab/embedded/service/gitlab-shell/config.yml
self_signed_cert: false
vim /var/opt/gitlab/gitlab-rails/etc/gitlab.yml
cp /opt/gitlab/etc/gitlab.rb.template /opt/gitlab/etc/gitlab.rb
```

5、备份还原数据
```
#停数据连接服务
gitlab-ctl stop unicorn
gitlab-ctl stop sidekiq
cd /var/opt/gitlab/backups
#备份
gitlab-rake gitlab:backup:create
#还原
gitlab-rake gitlab:backup:restore BACKUP=xxxxx
```

### 安装成功效果

    [http://git.diycode.me/](http://git.diycode.me/)

注：完成上述步骤即实现Gitlab的原版安装，本文有任何错误或有任何疑问，欢迎留言说明。
