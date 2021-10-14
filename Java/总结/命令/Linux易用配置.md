# Linux易用配置

## 安装ZSH

- 安装zsh

```bash
yum -y install zsh
```

- 查看shell列表

```bash
cat /etc/shells 
/bin/sh
/bin/bash
/sbin/nologin
/usr/bin/sh
/usr/bin/bash
/usr/sbin/nologin
/bin/tcsh
/bin/csh
/bin/zsh
```

- 切换shell为zsh

```ruby
chsh -s /bin/zsh
Changing shell for root.
Shell changed.
```

- 重启服务器

```bash
reboot
```

- 查看当前shell

```bash
echo $SHELL 
/bin/zsh
```

### 安装oh-my-zsh

## 下载码云安装包

```sh
wget https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh
```

- 编辑install.sh

- 找到以下部分

```sh
# Default settings
ZSH=${ZSH:-~/.oh-my-zsh}
REPO=${REPO:-ohmyzsh/ohmyzsh}
REMOTE=${REMOTE:-https://github.com/${REPO}.git}
BRANCH=${BRANCH:-master}
```

- 把

```sh
REPO=${REPO:-ohmyzsh/ohmyzsh}
REMOTE=${REMOTE:-https://github.com/${REPO}.git}
```

- 替换为

```sh
REPO=${REPO:-mirrors/oh-my-zsh}
REMOTE=${REMOTE:-https://gitee.com/${REPO}.git}
```

- 编辑后保存, 运行安装即可. (运行前先给install.sh权限)

```bash
chmod +x install.sh
./install.sh
```

- 修改仓库地址

```bash
cd ~/.oh-my-zsh
git remote set-url origin https://gitee.com/mirrors/oh-my-zsh.git
git pull
```

### 自动补全插件

- 下载该插件(`zsh-autosuggestions`)到`.oh-my-zsh`的插件目录

```bash
git clone https://gitee.com/hailin_cool/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

- 编辑`.zshrc`文件,找到`plugins=(git)`这一行，如果没有添加。更改为如下

```undefined
plugins=(git zsh-autosuggestions)
```

- 更新配置

```bash
source ~/.zshrc   
```

### 语法高亮插件

- 下载该插件(`zsh-syntax-highlighting`)到`.oh-my-zsh`的插件目录

```bash
git clone https://gitee.com/Annihilater/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

- 编辑`.zshrc`文件,找到`plugins=(git)`这一行，如果没有添加。更改为如下

```bash
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

- 更新配置

```bash
source ~/.zshrc   
```

### 安装Powerlevel10k 

- 安装字体

```bash
yum -y install fontconfig 
```

- 安装pip工具

```bash
yum -y install epel-release
yum install python-pip
```

- 安装powerline

```bash
pip install --user git+git://github.com/powerline/powerline
```

- 配置

```bash
## POWERLEVEL9K SETTINGS ##
POWERLEVEL9K_STATUS_VERBOSE=false
POWERLEVEL9K_STATUS_OK_IN_NON_VERBOSE=true
POWERLEVEL9K_PROMPT_ON_NEWLINE=true
POWERLEVEL9K_MULTILINE_FIRST_PROMPT_PREFIX=''
POWERLEVEL9K_MULTILINE_LAST_PROMPT_PREFIX="%K{white}%F{black} \UE12E `date +%T` %f%k%F{white}%f "
POWERLEVEL9K_SHORTEN_DIR_LENGTH=2
POWERLEVEL9K_CUSTOM_INTERNET_SIGNAL="zsh_internet_signal"
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(custom_internet_signal os_icon  dir vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status zsh_showStatus)
POWERLEVEL9K_OS_ICON_BACKGROUND="white"
POWERLEVEL9K_OS_ICON_FOREGROUND="blue"
POWERLEVEL9K_DIR_HOME_FOREGROUND="white"
POWERLEVEL9K_DIR_HOME_SUBFOLDER_FOREGROUND="white"
POWERLEVEL9K_DIR_DEFAULT_FOREGROUND="white"
POWERLEVEL9K_PROMPT_ADD_NEWLINE=true
```

- 更新zsh

```bash
sudo rpm -Uvh zsh-5.1-1.gf.el7.x86_64.rpm
```

- 安装

```bash
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

- 修改`~/.zshrc`文件

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"
```

- 进入配置页面

```bash
p10k configure
```

